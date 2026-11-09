# Module 8 — Windows Update for Business avec Microsoft Intune

**État : exercices de configuration et de supervision réalisés — novembre 2026**

## Objectifs

Administrer les mises à jour Windows 11 à distance depuis Intune : anneaux de mises à jour, reports, échéances, version cible, vérification de l'application des stratégies et analyse des journaux Windows Update.

## Environnement

- VM Windows 11 Pro `FRANCK`, jointe à Microsoft Entra ID et inscrite dans Intune.
- Groupe de test : `GRP-INTUNE-WIN11-LAB`.
- Version relevée localement : **26H2**, build **26300.9711**.
- La propriété de registre `ProductName` renvoie « Windows 10 Pro », alors que la version et le build relevés sont ceux de l'environnement Windows 11 du laboratoire. Ne pas utiliser ce seul libellé pour identifier le système.

## 8.1–8.2 — Anneau de mises à jour

Stratégie Intune : **`RING-PILOT-WIN11`**.

La capture du rapport d'affectation Intune montre **1 réussite, 0 erreur, 0 conflit**, avec l'appareil `FRANCK`.

### Paramètres confirmés sur la VM

| Paramètre | Valeur relevée |
|---|---|
| Mises à jour automatiques | `AllowAutoUpdate = 1` |
| Autres produits Microsoft | `AllowMUUpdateService = 1` |
| Pilotes via Windows Update | `ExcludeWUDriversInQualityUpdate = 0` |
| Report des mises à jour qualité | 0 jour |
| Report des mises à jour de fonctionnalités | 7 jours |
| Heures d'activité | 08:00–18:00 |
| Période de désinstallation d'une mise à jour de fonctionnalités | 10 jours |
| Échéance qualité | 2 jours |
| Échéance fonctionnalités | 7 jours |
| Période de grâce | 2 jours |
| Pas de redémarrage automatique avant échéance | `ConfigureDeadlineNoAutoReboot = 1` |
| Notifications | `UpdateNotificationLevel = 0` |

> Les heures d'activité ne garantissent pas l'absence de redémarrage après expiration des échéances et des périodes de grâce. Tester ces paramètres sur un groupe pilote.

### Vérification PowerShell

```powershell
$p = Get-ItemProperty "HKLM:\SOFTWARE\Microsoft\PolicyManager\current\device\Update"
[PSCustomObject]@{
    HeuresActivite = "$($p.ActiveHoursStart)h - $($p.ActiveHoursEnd)h"
    ReportQualite = $p.DeferQualityUpdatesPeriodInDays
    ReportFonctionnalites = $p.DeferFeatureUpdatesPeriodInDays
    EcheanceQualite = $p.ConfigureDeadlineForQualityUpdates
    EcheanceFonctionnalites = $p.ConfigureDeadlineForFeatureUpdates
    PeriodeGrace = $p.ConfigureDeadlineGracePeriod
    PasDeRedemarrageAvantEcheance = $p.ConfigureDeadlineNoAutoReboot
} | Format-List
```

**Résultat :** toutes les valeurs affichées correspondaient à la configuration attendue.

## 8.3–8.4 — Diagnostic et contrôle des redémarrages

Windows Update a initialement affiché un échec concernant une mise à jour de définitions Microsoft Defender (KB2267602) et une mise à jour de préversion facultative (KB5127936).

L'utilisateur a ensuite constaté qu'aucune erreur ni mise à jour en attente n'apparaissait dans l'interface Windows Update. Ce constat ne prouve pas, à lui seul, l'installation de tous les correctifs proposés auparavant.

Commande de consultation des correctifs recensés :

```powershell
Get-HotFix |
    Sort-Object InstalledOn -Descending |
    Select-Object -First 10 HotFixID, Description, InstalledOn
```

Correctifs relevés : KB5121794, KB5127648, KB5127694, KB5127647, KB5128942 et KB5054156. `Get-HotFix` n'inventorie pas toutes les catégories de mises à jour Windows.

## 8.5 — Stratégie de mises à jour de fonctionnalités

Stratégie : **`FEATURE-WIN11-LAB`**.

| Élément | Valeur observée dans Intune |
|---|---|
| Version cible | Windows 11, version 26H2 |
| Option de déploiement | `ImmediateStart` |
| Mise à jour | Obligatoire |
| Groupe inclus | `GRP-INTUNE-WIN11-LAB` |
| État de l'affectation du groupe | Actif |

**Validation :** stratégie créée et groupe affecté. Le contrôle local confirme que la VM exécute déjà Windows 11 26H2, build 26300.9711. Aucun déploiement d'une nouvelle version n'a été démontré par ce test.

### Point d'attention : Windows Autopatch

Une première tentative de création d'une « mise en production » Autopatch a été bloquée car aucun groupe Autopatch n'était disponible. La stratégie classique de mises à jour de fonctionnalités a ensuite été créée. Ne pas confondre la création d'un déploiement Autopatch avec celle d'une stratégie Feature Update.

## 8.6 — Supervision et incident 0x80240438

Commande de lecture des événements Windows Update :

```powershell
Get-WinEvent -FilterHashtable @{
    LogName = "Microsoft-Windows-WindowsUpdateClient/Operational"
    StartTime = (Get-Date).AddDays(-7)
} -MaxEvents 20 |
Select-Object TimeCreated, Id, LevelDisplayName, Message |
Format-List
```

### Événements observés le 8 novembre 2026

| Heure | Événement |
|---|---|
| 15:37 | Recherche : 4 mises à jour détectées |
| 15:40–16:01 | Événements de téléchargement |
| 16:14:01 | Erreur de recherche `0x80240438` (ID 25) |
| 16:14:20 | Nouvelles recherches : 2 et 3 mises à jour détectées |
| 16:14:39–16:14:40 | Nouveaux événements de téléchargement |
| 16:21:33 | Recherche : 0 mise à jour détectée |

**Interprétation prudente :** une erreur de recherche s'est produite, puis les recherches et téléchargements ont repris. La cause réseau/proxy n'a pas été démontrée. Aucune réinitialisation du composant Windows Update n'a été effectuée.

Une recherche complémentaire des événements **ID 19** (installation réussie) et **ID 20** (échec d'installation) sur les sept derniers jours n'a retourné aucun résultat. **L'installation effective de nouveaux correctifs pendant cette période n'a donc pas été confirmée par ces journaux.**

```powershell
Get-WinEvent -FilterHashtable @{
    LogName = "Microsoft-Windows-WindowsUpdateClient/Operational"
    Id = 19,20
    StartTime = (Get-Date).AddDays(-7)
} -ErrorAction SilentlyContinue |
Select-Object TimeCreated, Id, Message |
Format-List
```

## Bilan et limites

**Validé :** création et affectation d'un anneau de mises à jour, présence des paramètres dans le registre local, création d'une stratégie Feature Update ciblant 26H2, interrogation de l'historique des correctifs et analyse des événements Windows Update.

**Non démontré :** installation d'un nouveau correctif provoquée par les stratégies du laboratoire, déploiement d'une nouvelle version de fonctionnalités, remédiation formelle de l'erreur `0x80240438`, vérification d'une conformité de mise à jour de bout en bout.

## Prochaine étape

Module 9 : Microsoft Defender et BitLocker — configuration des protections, chiffrement et vérification des états depuis Intune.

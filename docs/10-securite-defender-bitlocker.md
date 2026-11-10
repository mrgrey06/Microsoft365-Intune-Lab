# Module 9 — Sécurisation Windows 11 : Defender, Firewall, ASR et BitLocker

**Dernière vérification : 10 novembre 2026.** Environnement de test : machine virtuelle Windows 11 Pro **FRANCK** sous VMware Workstation, inscrite dans Intune et jointe à Entra ID.

## Objectif
Mettre en œuvre et contrôler la protection antivirus, le pare-feu, les règles de réduction de la surface d'attaque (ASR) et le chiffrement du disque système avec récupération de clé.

## 9.1 — État initial
- Microsoft Defender Antivirus, protection temps réel, surveillance comportementale, analyse des téléchargements et des scripts actifs.
- Pare-feu Domain, Private et Public actif.
- TPM virtuel VMware présent, prêt et activé.
- Disque C: initialement `FullyDecrypted`, `ProtectionStatus = Off`, 0 %.
- Sauvegarde complète du dossier de la VM effectuée avant chiffrement.

## 9.2 — Microsoft Defender Antivirus
Stratégie : `SEC-DEFENDER-AV-WIN11-LAB`, affectée au groupe de laboratoire `GRP-INTUNE-WIN11-LAB`. Le rapport de configuration Intune a affiché **Réussite**.

Dernière vérification PowerShell :
| Paramètre | Résultat |
|---|---|
| `AntivirusEnabled` | `True` |
| `RealTimeProtectionEnabled` | `True` |
| `BehaviorMonitorEnabled` | `True` |
| Dernière mise à jour des signatures | 10/11/2026 à 13:53:49 |
| `ScanAvgCPULoadFactor` | `30` |
| `SignatureUpdateInterval` | `4` |
| `PUAProtection` | `0` (**désactivé**) |

**Point à corriger :** la protection PUA est désactivée localement, malgré l'objectif de la configurer en mode blocage. Ne pas déclarer ce paramètre conforme tant que `PUAProtection = 1` n'est pas constaté.

```powershell
Get-MpComputerStatus |
  Select-Object AntivirusEnabled, RealTimeProtectionEnabled, BehaviorMonitorEnabled, AntivirusSignatureLastUpdated

Get-MpPreference |
  Select-Object PUAProtection, ScanAvgCPULoadFactor, SignatureUpdateInterval
```

## 9.3 — Microsoft Defender Firewall
Stratégie : `SEC-FIREWALL-WIN11-LAB` ; rapport Intune **Réussite**.

Vérification des trois profils avec `Get-NetFirewallProfile -PolicyStore ActiveStore` et `netsh advfirewall show allprofiles` :

| Profil | Activé | Entrant | Sortant |
|---|---|---|---|
| Domain | True | Block | Allow |
| Private | True | Block | Allow |
| Public | True | Block | Allow |

## 9.4 — Attack Surface Reduction
Stratégie : `SEC-ASR-WIN11-LAB` ; rapport Intune **Réussite**.

La lecture des préférences Microsoft Defender a retourné **18 identifiants de règles ASR**, chacun avec l'action `2` (**Audit**). La recherche des événements ASR 1121 et 1122 sur les sept derniers jours n'a retourné aucun résultat. L'absence d'événement ne prouve pas qu'une simulation d'attaque a été exécutée.

```powershell
$p = Get-MpPreference
[PSCustomObject]@{
  ReglesASR = @($p.AttackSurfaceReductionRules_Ids).Count
  Actions = $p.AttackSurfaceReductionRules_Actions -join ', '
}
```

## 9.5 — Chiffrement BitLocker
Stratégie préparée : `SEC-BITLOCKER-WIN11-LAB`. La sauvegarde de la VM a été réalisée avant l'activation.

**Incident VMware :** l'assistant BitLocker a signalé la présence d'un média de démarrage amovible (CD/DVD). Correction : déconnexion du lecteur CD/DVD virtuel contenant potentiellement l'ISO, puis redémarrage de la VM. Le chiffrement a ensuite démarré et atteint 100 %.

Dernier contrôle local :

| Paramètre | Résultat |
|---|---|
| `VolumeStatus` | `FullyEncrypted` |
| `ProtectionStatus` | `On` |
| `EncryptionPercentage` | `100` |
| `TpmPresent`, `TpmReady`, `TpmEnabled` | `True` |
| Types de protecteurs | `TpmPin`, `RecoveryPassword` |

**Écart par rapport au plan initial :** le protecteur constaté est **TPM + PIN**, et non TPM seul. Conserver le PIN en sécurité. Le protecteur `RecoveryPassword` existe ; sa présence locale ne démontre pas à elle seule la sauvegarde distante.

## 9.6 — Récupération de clé
Une première capture Intune affichait « Aucune clé de récupération BitLocker trouvée » avant le chiffrement. L'utilisateur a ensuite indiqué que les étapes de récupération et de chiffrement étaient terminées. **La présence finale de la clé dans Entra ID n'a pas été montrée par une nouvelle capture** ; à revérifier dans Intune → FRANCK → Clés de récupération BitLocker, sans publier le secret à 48 chiffres.

## 9.7 — Commandes de contrôle
```powershell
Get-MpComputerStatus |
  Select-Object AntivirusEnabled, RealTimeProtectionEnabled, BehaviorMonitorEnabled, AntivirusSignatureLastUpdated

Get-MpPreference |
  Select-Object PUAProtection, ScanAvgCPULoadFactor, SignatureUpdateInterval

Get-NetFirewallProfile -PolicyStore ActiveStore |
  Select-Object Name, Enabled, DefaultInboundAction, DefaultOutboundAction

Get-BitLockerVolume -MountPoint 'C:' |
  Select-Object MountPoint, VolumeStatus, ProtectionStatus, EncryptionPercentage

Get-Tpm | Select-Object TpmPresent, TpmReady, TpmEnabled

(Get-BitLockerVolume -MountPoint 'C:').KeyProtector |
  Select-Object KeyProtectorType
```

## Bilan
- **Validés localement :** Defender actif, pare-feu sur trois profils, 18 règles ASR en audit, TPM opérationnel, disque C: entièrement chiffré et protégé.
- **À finaliser :** protection PUA en blocage (`1`), confirmation visuelle de la clé de récupération dans Entra ID, contrôle du rapport BitLocker Intune et explication du précédent compteur de synchronisation « 8 sur 9 ».
- **Prochain module :** Windows Autopilot.

> Aucune clé BitLocker, aucun PIN, secret ou identifiant personnel n'est publié dans ce document.

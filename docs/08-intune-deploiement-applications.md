# Module 7 — Déploiement et gestion des applications avec Microsoft Intune

**État : terminé — 4 novembre 2026**

## Contexte du laboratoire

Administration d'un poste Windows 11 Pro virtualisé dans VMware Workstation, joint à Microsoft Entra ID et inscrit dans Microsoft Intune. Poste de test : `FRANCK`. Groupe cible : `GRP-INTUNE-WIN11-LAB`.

Les manipulations décrites ci-dessous sont des exercices réalisés en environnement de laboratoire et non des déploiements en production.

## Scénarios étudiés

| Fonctionnalité | Mise en pratique |
|---|---|
| Applications Microsoft Store | Déploiement et suivi d'applications Store |
| Applications métier (LOB) | Déploiement d'un package MSI, notamment VLC |
| Applications Win32 | Packaging `.intunewin` et déploiement d'applications EXE/MSI |
| Microsoft 365 Apps | Déploiement de Word, Excel et PowerPoint |
| Liens web | Publication d'une application de type Web Link |
| Affectations | Utilisation des affectations obligatoires et de désinstallation |
| Détection | Contrôle de présence, de version de fichier et interprétation des statuts |
| Supersedence | Remplacement de Notepad++ 8.9.7 par 8.9.8.1 |
| Dépendances Win32 | Installation automatique de 7-Zip avant Notepad++ |

## Cas pratique 1 — Supersedence Notepad++

**Objectif :** mettre à niveau Notepad++ 8.9.7 vers 8.9.8.1 sans désinstaller préalablement l'ancienne version.

1. Préparer les installateurs Win32 des versions 8.9.7 et 8.9.8.1.
2. Déployer et vérifier la présence de Notepad++ 8.9.7 sur `FRANCK`.
3. Configurer dans l'application 8.9.8.1 une relation **Supersedence** vers l'application 8.9.7, avec **Désinstaller la version précédente : Non**.
4. Retirer l'affectation obligatoire de l'ancienne application, puis affecter la nouvelle application au groupe de test.
5. Synchroniser le poste et vérifier la version réellement installée.

### Incident : faux positif de détection

Le portail Intune indiquait **Installé** pour Notepad++ 8.9.8.1, alors que la VM exécutait toujours **8.9.7**.

**Cause identifiée :** la règle de détection de la nouvelle application vérifiait seulement l'existence de `C:\Program Files\Notepad++\notepad++.exe`. Ce fichier existait déjà avec l'ancienne version.

**Correction :** remplacer la détection par une vérification de **version du fichier supérieure ou égale à 8.9.8.1**.

```powershell
(Get-Item "C:\Program Files\Notepad++\notepad++.exe").VersionInfo.ProductVersion
```

**Résultat observé :** mise à niveau vers **8.9.8.1** confirmée localement, puis statut **Installé** confirmé dans Intune.

**Enseignement :** le statut de déploiement dépend de la pertinence des règles de détection ; il faut vérifier la version réellement présente sur le poste.

## Cas pratique 2 — Dépendances Win32

**Objectif :** faire installer automatiquement un prérequis avant l'application principale.

> Dépendance pédagogique : Notepad++ ne nécessite pas réellement 7-Zip. La relation sert uniquement à démontrer l'orchestration Intune.

- **Application principale :** Notepad++ 8.9.8.1 — Win32 EXE.
- **Dépendance :** 7-Zip 26.05 — Win32 MSI.
- **Paramètre :** `Installer automatiquement = Oui`.

### Procédure

1. Ajouter 7-Zip dans les **Dépendances** de l'application Notepad++.
2. Retirer l'affectation obligatoire directe de 7-Zip pour isoler le mécanisme de dépendance.
3. Désinstaller localement 7-Zip et Notepad++ du poste de test.
4. Conserver l'affectation obligatoire de Notepad++ et synchroniser la VM.
5. Observer la réinstallation des applications.

**Résultat observé :** après un délai de traitement, **7-Zip s'est installé en premier**, suivi de **Notepad++**. Le mécanisme de dépendance a été validé.

**Enseignement :** la synchronisation Windows ne déclenche pas nécessairement instantanément le traitement des applications Win32 ; l'Intune Management Extension dispose de son propre cycle d'évaluation.

## Bonnes pratiques retenues

- Adapter les règles de détection à la version réellement attendue, et pas seulement à l'existence d'un exécutable.
- Vérifier les affectations avant les tests pour éviter les déploiements contradictoires.
- Contrôler à la fois l'état dans Intune et le résultat réel sur le poste.
- Utiliser les journaux de l'Intune Management Extension en cas de blocage persistant.
- Tester les dépendances et les remplacements sur un groupe pilote avant une généralisation.
- Éviter de publier dans un dépôt public des identifiants, secrets, captures non anonymisées ou fichiers de configuration sensibles.

## Suite du parcours

**Module 8 : Windows Update for Business** — anneaux de mises à jour, stratégies de report, déploiement et supervision.

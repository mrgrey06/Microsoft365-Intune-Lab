# 01 — Installation VMware et Windows 11 Pro

## Objectif
Préparer un poste client Windows 11 Pro pour un laboratoire Microsoft 365 / Intune.

## Configuration utilisée
- Hôte : Windows 11 Pro.
- Hyperviseur : VMware Workstation 17.5.2.
- VM : 4 Go RAM, 2 vCPU, disque virtuel d'environ 64 Go, firmware UEFI et TPM virtuel.
- Réseau : NAT.

## Déroulement
1. Création de la VM Windows 11 Pro.
2. Première tentative d'installation interrompue pendant l'application de l'image Windows (voir incident dédié).
3. Nouvelle installation depuis une autre image ISO Windows 11, réussie.
4. Installation de VMware Tools.
5. Validation de l'accès Internet.
6. Lancement d'une mise à jour vers Windows 11 26H2 (achèvement à vérifier avec `winver`).

## Vérifications recommandées
- `winver` : édition et version de Windows.
- `Get-ComputerInfo | Select-Object WindowsProductName, WindowsVersion, OsBuildNumber` : informations système.
- `ipconfig /all` : configuration réseau.
- Snapshot VMware avant la jonction Entra ID.

## État
VM fonctionnelle après résolution d'un verrouillage VMware. Snapshot de référence recommandé mais non confirmé.

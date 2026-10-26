# Microsoft 365 & Intune — Laboratoire d'administration

Laboratoire pratique de gestion des identités, des terminaux Windows et de la sécurité avec **Microsoft 365 Business Premium**, **Microsoft Entra ID**, **Microsoft Intune** et **VMware Workstation**.

> Projet de formation personnel, sans affiliation avec Microsoft. Les comptes présentés sont fictifs. Documentation initiale au 20 octobre 2026, mise à jour le 26 octobre 2026.

## Objectifs
- Déployer un poste Windows 11 Pro virtualisé.
- Administrer les utilisateurs, groupes et licences Microsoft 365.
- Configurer et tester l'authentification multifacteur (MFA).
- Préparer l'inscription automatique MDM et intégrer un poste à Intune.
- Documenter les incidents et leur résolution avec une démarche de support N2/N3.

## Environnement
| Composant | Configuration observée |
|---|---|
| Hyperviseur | VMware Workstation 17.5.2 |
| Poste virtuel | Windows 11 Pro, 4 Go de RAM, 2 vCPU, disque virtuel ~64 Go |
| Réseau VM | NAT, accès Internet vérifié |
| VMware Tools | Installés |
| Mise à jour Windows | Windows 11 26H2, build 26300.9711 vérifiée avec winver |
| Services cloud | Essai Microsoft 365 Business Premium, 25 licences annoncées disponibles initialement |

## Avancement
| Activité | État |
|---|---|
| Installation de Windows 11 Pro et VMware Tools | Terminé |
| Création du tenant Microsoft 365 Business Premium | Terminé |
| Création de l'utilisateur fictif Jean Dupont | Terminé |
| Création du groupe `GRP-IT-Utilisateurs` | Terminé |
| Stratégie `LAB-MFA-Utilisateurs` | Créée en mode rapport uniquement ; activation ultérieure non confirmée |
| Test MFA avec code TOTP via Google Authenticator | Connexion réussie ; origine exacte de l'exigence MFA non vérifiée |
| Paramétrage de l'étendue MDM pour le groupe | Configuration engagée ; enregistrement final à confirmer |
| Jonction Entra ID de la VM | Terminé — AzureAdJoined YES vérifié sur la VM |
| Inscription effective dans Intune | Appareil FRANCK visible dans Intune |
| Stratégie de verrouillage de session à 15 minutes | Appliquée et vérifiée |
| Stratégie de conformité Windows 11 | Validée : non-conformité simulée, diagnostiquée et corrigée |

## Documentation
- [Installation VMware et Windows 11](docs/01-installation-vmware-windows11.md)
- [Tenant Microsoft 365 Business Premium](docs/02-microsoft365-business-premium.md)
- [Entra ID : utilisateurs et groupes](docs/03-entra-id-utilisateurs-groupes.md)
- [MFA et accès conditionnel](docs/04-mfa-acces-conditionnel.md)
- [Intune : préparation MDM](docs/05-intune-inscription-mdm.md)

- [Intune : stratégie de verrouillage de session à 15 minutes](docs/06-intune-verrouillage-15-min.md)
- [Intune : conformité Windows 11, diagnostic et remédiation](docs/07-intune-conformite-windows11.md)

## Incidents documentés
- [Échec d'installation Windows — 0x8007000D](troubleshooting/installation-windows-erreur-0x8007000D.md)
- [VMware — Taking ownership failed](troubleshooting/vmware-taking-ownership-failed.md)

## Sécurité et confidentialité
Ce dépôt ne contient **aucun mot de passe, secret, jeton, QR code MFA, capture de compte personnel, donnée de paiement, nom de domaine réel du tenant ou image de disque virtuel**. Les exemples sont fictifs et les captures futures doivent être anonymisées avant publication.

## Suite
Voir [roadmap.md](roadmap.md).

# Roadmap — Microsoft365-Intune-Lab

## Phase 1 — Socle [réalisé]
- [x] Installer VMware Workstation et Windows 11 Pro.
- [x] Installer VMware Tools et valider la connexion Internet.
- [x] Créer un tenant Microsoft 365 Business Premium.
- [x] Créer un utilisateur et un groupe de sécurité fictifs.
- [x] Créer une stratégie MFA en mode rapport uniquement.
- [x] Tester une connexion avec code TOTP.
- [x] Résoudre les incidents d'installation Windows et de verrouillage VMware.

## Phase 2 — Entra ID et Intune [en cours]
- [ ] Confirmer la licence attribuée au compte de test.
- [ ] Confirmer l'enregistrement de l'étendue MDM pour le groupe.
- [ ] Vérifier la version Windows finale (`winver`).
- [ ] Créer un snapshot VMware de référence.
- [x] Joindre la VM à Microsoft Entra ID.
- [x] Vérifier l'inscription dans Intune.
- [x] Déployer une stratégie de conformité et un profil de configuration.
- [x] Tester le déploiement d'applications (module 7 : Win32, Supersedence, dépendances).

## Windows Update for Business — module 8
- [x] Déployer et vérifier un anneau pilote Windows Update.
- [x] Configurer une stratégie Feature Update ciblant Windows 11 26H2.
- [x] Analyser les événements Windows Update et documenter l'erreur 0x80240438.
- [ ] Confirmer une installation de correctif de bout en bout avec les journaux d'installation.

## Sécurité des terminaux — module 9
- [x] Vérifier Defender Antivirus et les signatures.
- [x] Déployer et contrôler le pare-feu Windows.
- [x] Déployer 18 règles ASR en mode audit.
- [x] Chiffrer C: avec BitLocker et TPM virtuel VMware.
- [x] Documenter l'incident de média CD/DVD virtuel.
- [ ] Activer et vérifier la protection PUA en mode blocage.
- [ ] Reconfirmer la présence de la clé de récupération dans Entra ID.

## Exchange Online — module 11
- [x] Créer et ouvrir une boîte aux lettres partagée.
- [x] Configurer les délégations d'accès.
- [x] Envoyer un message et confirmer sa réception.
- [ ] Vérifier les autorisations avec Exchange Online PowerShell.

## Entra ID avancé — module 12
- [x] Créer un groupe dynamique et vérifier l'ajout automatique de Sory.
- [x] Consulter le rôle Lecteur général dans les affectations.
- [x] Examiner les journaux d'audit Entra ID.
- [ ] Vérifier les permissions effectives du rôle en session de test.
- [ ] Identifier les événements d'audit propres au groupe et au rôle.

## Collaboration Microsoft 365 — module 13
- [x] Créer un site SharePoint et un document dans une bibliothèque.
- [x] Créer une équipe Teams et accéder à ses fichiers.
- [x] Résoudre l'erreur de validation de sécurité de création de dossier via SharePoint.
- [ ] Confirmer le partage OneDrive et les tests multi-utilisateurs.

## Phase 3 — Administration avancée [prévue]
- [ ] Exchange Online : boîtes partagées et délégations.
- [ ] Gestion des rôles et du moindre privilège.
- [ ] Gestion du cycle de vie des appareils et des utilisateurs.
- [ ] PowerShell / Microsoft Graph : inventaire et automatisation.
- [ ] Procédures de support et scénarios d'incidents supplémentaires.

## Qualité documentaire
- [ ] Ajouter des captures anonymisées et vérifiées.
- [ ] Ajouter les résultats mesurables des tests.
- [ ] Tenir un journal de changements daté.

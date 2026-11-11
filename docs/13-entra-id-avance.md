# Module 12 — Microsoft Entra ID avancé

**Date : 11 novembre 2026**

## Objectifs
Automatiser l'appartenance à des groupes avec des règles dynamiques, explorer le contrôle d'accès fondé sur les rôles (RBAC) et consulter les journaux d'audit.

## 1. Groupe dynamique : test validé
- Groupe de sécurité : `GRP-DYN-IT-LAB`
- Type d'appartenance : utilisateur dynamique
- Règle : `(user.department -eq "IT")`
- Utilisateur de laboratoire : Sory
- Modification du champ Département de l'utilisateur : `IT`
- **Résultat : Sory apparaît automatiquement dans les membres du groupe**, confirmé par l'utilisateur.

## 2. RBAC : rôle Lecteur général
- Rôle étudié : **Lecteur général / Global Reader**.
- La navigation du portail affichait le chemin `Rôles et administrateurs > Rôles Microsoft Entra ID > Lecteur général > Affectations > Utilisateurs > sory`.
- Cela documente la consultation de l'affectation du rôle dans l'interface.
- **Limite :** aucun résultat de test en session distincte ne prouve encore les permissions effectives en lecture seule ; le détail de l'affectation n'a pas été montré.

## 3. Journaux d'audit
L'utilisateur a ouvert les journaux d'audit Microsoft Entra. La capture affichait plusieurs événements réussis du 11 novembre 2026 :
- `Update PasswordProfile`
- `Update user`
- `Reset password (by admin)`
- `Update StsRefreshTokenValidFrom`

**Limite :** la capture ne montre pas spécifiquement l'événement d'ajout au rôle Lecteur général ni celui de création du groupe dynamique. Ces événements restent à rechercher si le tenant est encore accessible.

## Compétences démontrées
- Création d'un groupe de sécurité à appartenance dynamique.
- Utilisation d'une règle fondée sur l'attribut `department`.
- Vérification de l'ajout automatique d'un membre.
- Navigation dans les rôles Entra ID et consultation de journaux d'audit.

## Bonnes pratiques
- Appliquer le principe du moindre privilège.
- Préserver un compte administrateur de secours.
- Éviter de publier des identifiants sensibles ou des exports bruts de journaux sur GitHub.

## Bilan
**Validé :** groupe dynamique et ajout automatique ; accès aux journaux d'audit.

**À approfondir :** confirmation des permissions RBAC avec un compte de test et vérification des événements d'audit spécifiques.

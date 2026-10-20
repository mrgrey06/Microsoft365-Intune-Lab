# 03 — Microsoft Entra ID : utilisateurs et groupes

## Objectif
Mettre en place une première organisation fictive et préparer un ciblage par groupe pour Intune.

## Objets créés
| Type | Valeur fictive |
|---|---|
| Utilisateur | Jean Dupont |
| Groupe de sécurité | `GRP-IT-Utilisateurs` |
| Type d'appartenance | Attribué |

## Procédure de création d'un utilisateur
1. Ouvrir `https://entra.microsoft.com`.
2. Accéder à **Entra ID > Utilisateurs > Tous les utilisateurs**.
3. Créer un utilisateur interne fictif.
4. Vérifier le pays d'utilisation avant d'attribuer une licence.
5. Appliquer le principe du moindre privilège : rôle utilisateur standard.

## Procédure de création d'un groupe
1. **Entra ID > Groupes > Tous les groupes > Nouveau groupe**.
2. Type : **Sécurité**.
3. Nom : `GRP-IT-Utilisateurs`.
4. Ajouter l'utilisateur fictif comme membre.

## Contrôles à effectuer
- Vérifier l'appartenance effective de l'utilisateur au groupe.
- Vérifier l'attribution d'une licence Business Premium à l'utilisateur avant l'inscription MDM.
- Ne pas exposer d'identifiants réels dans les captures.

# 04 — MFA et accès conditionnel

## Objectif
Créer une stratégie d'accès conditionnel MFA pour un compte de test, puis observer son évaluation dans les journaux.

## Stratégie créée
| Paramètre | Valeur |
|---|---|
| Nom | `LAB-MFA-Utilisateurs` |
| Cible | Utilisateur fictif Jean Dupont |
| Ressources | Toutes les ressources |
| Contrôle d'octroi | Exiger une authentification multifacteur |
| État initial | Rapport uniquement |

## Étapes réalisées
1. Création de la stratégie depuis **Entra ID > Protection > Accès conditionnel**.
2. Ciblage limité à l'utilisateur de laboratoire.
3. Sélection du contrôle **Exiger MFA**.
4. Enregistrement en **Rapport uniquement**.
5. Recherche de la stratégie dans les journaux de connexion, finalement retrouvée.
6. Connexion de l'utilisateur avec un code à usage unique (TOTP) fourni par Google Authenticator : réussite rapportée.

## Interprétation importante
Le mode **Rapport uniquement** ne force pas lui-même l'authentification MFA. Le défi observé peut être dû aux paramètres de sécurité par défaut ou à une autre règle. La réussite de la connexion prouve qu'une vérification supplémentaire a fonctionné, mais ne démontre pas à elle seule que `LAB-MFA-Utilisateurs` a été activée et appliquée.

## Contrôles à poursuivre
- Examiner le résultat de la stratégie dans les journaux de connexion.
- Confirmer si la stratégie a été activée après la phase de test.
- Prévoir un compte administrateur de secours et vérifier les paramètres de sécurité avant tout changement global.

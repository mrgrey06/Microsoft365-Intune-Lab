# 05 — Microsoft Intune : préparation de l'inscription MDM

## Objectif
Inscrire la VM Windows 11 Pro dans Microsoft Entra ID et permettre sa gestion par Intune.

## Configuration préparée
- **Étendue utilisateur MDM** : `Partiel` / `Certains`.
- Groupe ciblé : `GRP-IT-Utilisateurs`.
- **Étendue utilisateur MAM** : `Aucun`.
- URL MDM : valeurs fournies par Microsoft, sans personnalisation.

> Les captures de configuration ont été examinées, mais la confirmation définitive de l'enregistrement MDM n'est pas établie. L'inscription de la VM dans Intune n'est pas encore réalisée.

## Prérequis à confirmer
1. Licence Microsoft 365 Business Premium attribuée à l'utilisateur test.
2. Utilisateur membre du groupe MDM ciblé.
3. Étendue MDM enregistrée.
4. VM stable, mise à jour et snapshot effectué.
5. Compte administrateur local de secours conservé.

## Étapes suivantes (non réalisées)
1. Sur la VM : **Paramètres > Comptes > Accès professionnel ou scolaire > Se connecter**.
2. Choisir **Joindre cet appareil à Microsoft Entra ID** (ne pas confondre avec le simple ajout d'un compte).
3. Se connecter avec l'utilisateur de test licencié.
4. Vérifier l'appareil dans **Entra ID > Appareils** et **Intune > Appareils > Windows**.
5. Tester une stratégie de conformité, puis un profil de configuration.

## Précaution
Ne pas inscrire le PC physique personnel à titre expérimental ; utiliser uniquement la VM dédiée.

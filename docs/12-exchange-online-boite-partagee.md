# Module 11 — Exchange Online : boîte aux lettres partagée

**Date : 11 novembre 2026**

## Objectif
Administrer une boîte aux lettres partagée pour un service informatique fictif et tester son utilisation sans identifiants communs.

## Environnement
Microsoft 365 Business Premium, centre d'administration Exchange Online et Outlook sur le Web. Deux boîtes utilisateur existaient dans le tenant de laboratoire.

## Manipulations réalisées
1. Création de la boîte aux lettres partagée **Support IT LAB** (adresse de laboratoire `support-it-lab@ciber1.onmicrosoft.com`).
2. Configuration des délégations nécessaires à l'utilisateur de test pour ouvrir et utiliser la boîte.
3. Ouverture de la boîte partagée depuis Outlook avec le compte personnel autorisé, sans mot de passe propre à la boîte partagée.
4. Envoi d'un message depuis la boîte partagée vers une autre adresse.
5. Vérification de la réception effective du message par le destinataire.

## Incident rencontré
À l'ouverture de la boîte partagée, Outlook sur le Web a initialement affiché `BootResult: accessDenied` et `Microsoft.Exchange.Data.StoreObjects.AccessDeniedException` (HTTP 500). Les droits ont été ajustés, puis l'accès et l'envoi ont fonctionné. Le détail précis du changement de droits n'a pas été consigné.

## Résultat
**Validé par test utilisateur :** accès à Support IT LAB, envoi depuis la boîte partagée et réception du message.

**Limites :** pas de contrôle PowerShell des permissions fourni et pas de test indépendant de la réception de messages entrants dans la boîte partagée. Le message d'erreur ne suffit pas, à lui seul, à établir la cause exacte de l'incident.

## Compétences
Administration Exchange Online, boîte partagée, délégation d'accès, dépannage Outlook sur le Web et test fonctionnel de messagerie.

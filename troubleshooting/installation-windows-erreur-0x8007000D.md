# Incident — Échec d'installation Windows 11 (`0x8007000D`)

## Symptôme
L'installation de Windows 11 dans VMware échoue lors de l'application de l'image Windows.

## Indices observés
- Journaux d'installation : `CApplyWIM: Failure while applying image 6`.
- Erreur principale : `0x8007000D` (données invalides).
- Autres erreurs remontées : `0x80004005` (générique).
- `chkdsk C:` ne détecte pas d'erreur de structure NTFS.
- Les métadonnées de l'image Windows pouvaient être lues par DISM ; cela ne garantit pas l'intégrité complète du contenu.

## Hypothèse
Défaillance de lecture/décompression de l'image d'installation, possiblement liée au premier ISO. L'origine exacte de la défaillance n'a pas été démontrée par un test d'intégrité complet.

## Résolution appliquée
Nouvelle tentative d'installation avec **une autre image ISO Windows 11** : installation réussie. VMware Tools et réseau ensuite fonctionnels.

## Leçon retenue
Un échec `Apply WIM` n'implique pas que la VM ou le disque virtuel doivent être supprimés. Vérifier le support d'installation avant de reconstruire tout l'environnement.

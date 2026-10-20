# Incident — VMware « Taking ownership failed »

## Symptôme
VMware Workstation refuse d'ouvrir la VM avec les messages :
- `This virtual machine appears to be in use`.
- `Taking ownership failed`.
- `The virtual machine is in use by an application on your host computer`.

## Diagnostic
1. Redémarrage du PC hôte : problème persistant.
2. Vérification du Gestionnaire des tâches : pas de processus `vmware-vmx.exe` observé.
3. Inspection du dossier de la VM : présence d'éléments terminés par `.lck`.

## Correction validée
Après fermeture de VMware et vérification qu'aucune instance n'utilisait la VM, les éléments `.lck` résiduels ont été **renommés en `.lck.old`**. La VM a ensuite redémarré correctement.

## Explication
VMware utilise des verrous pour éviter des accès concurrents à certaines ressources. Après un arrêt anormal ou une libération incomplète, des verrous résiduels peuvent empêcher le redémarrage.

## Précautions
- **Ne jamais supprimer ou renommer des verrous d'une VM encore active** : risque de corruption.
- Ne pas toucher aux fichiers `.vmdk`, `.vmx`, `.nvram` ou aux snapshots.
- Privilégier le renommage réversible des seuls verrous résiduels.
- Ne pas conclure qu'un disque virtuel est corrompu sur la seule base de ce message.

## Résultat
VM récupérée sans réinstallation de Windows 11. Cause précise de la persistance du verrou non établie.

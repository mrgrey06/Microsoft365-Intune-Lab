# Déploiement d'une stratégie de verrouillage Windows 11 avec Intune

**Date :** 22 octobre 2026  
**Environnement :** laboratoire Microsoft 365 Business Premium, Entra ID, Intune et VMware Workstation.

## Objectif
Configurer le verrouillage automatique de la session Windows 11 après 15 minutes d'inactivité (900 secondes) et vérifier la présence du paramètre sur le poste de test.

## Pré-requis observés
- VM Windows 11 Pro nommée `FRANCK`, hébergée dans VMware Workstation.
- Jonction Microsoft Entra ID effectuée : `AzureAdJoined : YES` et `DeviceAuthStatus : SUCCESS` observés dans la VM avec `dsregcmd /status`.
- Appareil visible dans le centre d'administration Microsoft Intune.
- Groupe de sécurité Entra ID `GRP-INTUNE-WIN11-LAB`, contenant l'appareil de test.

## Procédure suivie
1. Créer un groupe de sécurité dans Microsoft Entra ID destiné au ciblage pilote.
2. Ajouter l'objet appareil `FRANCK` au groupe.
3. Dans Intune, ouvrir **Appareils > Configuration** et créer un profil Windows avec le **Catalogue des paramètres**.
4. Nommer le profil `verrouillage 15 min`.
5. Dans les options de sécurité des stratégies locales, sélectionner la limite d'inactivité de connexion interactive **non déclassée** et définir la valeur à `900` secondes.
6. Affecter le profil au groupe `GRP-INTUNE-WIN11-LAB`.
7. Déclencher une synchronisation de l'appareil depuis Intune, puis vérifier la configuration sur la VM.

> Attention : les libellés peuvent varier selon la langue et l'évolution du portail. Ne pas confondre la limite d'inactivité de connexion interactive avec le délai d'inactivité d'un affichage externe ou une option marquée « Déclassé ».

## Vérification sur le poste
Exécuter **dans la VM** PowerShell :

```powershell
Get-ItemPropertyValue -Path "HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System" -Name InactivityTimeoutSecs -ErrorAction SilentlyContinue
```

**Résultat observé :** `900`.

Cela confirme la présence de la valeur de configuration dans Windows. Ce contrôle seul ne démontre pas son origine : le statut de déploiement du profil dans Intune doit également être vérifié.

## Test fonctionnel et limite d'interprétation
Après une période d'inactivité, la VM a affiché un état associé à une pause VMware. À la reprise, la session Windows était verrouillée.

Le verrouillage a donc été observé, mais cette observation ne permet pas d'isoler avec certitude le moment précis du verrouillage par rapport à la pause de VMware.

## Contrôles complémentaires recommandés
- Vérifier **Appareils > Configuration > verrouillage 15 min > État des appareils** : résultat attendu **Réussi**.
- Tester le verrouillage avec la VM maintenue active, sans mise en veille/suspension.
- Conserver une capture anonymisée de l'état de la stratégie et une sortie de commande sans informations sensibles.

## Enseignements
- **Entra ID** gère les identités et les groupes servant au ciblage.
- **Intune** distribue et surveille les paramètres de configuration.
- Un groupe pilote limite l'impact d'un mauvais paramétrage.
- Il faut distinguer une configuration présente dans le registre, un déploiement Intune déclaré réussi et un test fonctionnel concluant.

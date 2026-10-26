# Intune — Conformité Windows 11 : diagnostic et remédiation

**Statut : terminé — 26 octobre 2026**  
**Contexte :** laboratoire personnel Microsoft 365 Business Premium / Microsoft Intune, VM Windows 11 Pro sous VMware.

## Objectif

Définir une stratégie de conformité, vérifier les contrôles de sécurité, simuler une non-conformité sans désactiver les protections, identifier le paramètre en échec, puis restaurer la conformité.

## Périmètre

- Appareil pilote : `FRANCK`, Windows 11 Pro, inscrit dans Intune.
- Groupe cible : `GRP-INTUNE-WIN11-LAB`.
- Stratégie : `LAB-CONFORMITE-WIN11`.
- Action en cas de non-conformité : marquer immédiatement l'appareil comme non conforme (0 jour).

## Configuration initiale

Dans **Appareils > Conformité > Stratégies**, créer une stratégie pour **Windows 10 et versions ultérieures**.

Dans **Sécurité système** :

| Paramètre | Exigence |
|---|---|
| Pare-feu | Exiger |
| Antivirus | Exiger |
| Microsoft Defender Antimalware | Exiger |

Les autres paramètres sont laissés non configurés. Affecter la stratégie uniquement au groupe pilote `GRP-INTUNE-WIN11-LAB`.

**Validation initiale :** rapport de stratégie avec **1 appareil conforme**, **0 non conforme**.

## Simulation contrôlée d'une non-conformité

La commande `winver` sur la VM a indiqué :

- Windows 11 version **26H2**
- Build **26300.9711** (version complète **10.0.26300.9711**)

Dans **Propriétés > Paramètres de conformité > Propriétés de l'appareil**, définir temporairement :

```text
Version minimale du système d'exploitation : 10.0.26301.0
```

Cette version minimale est volontairement supérieure à celle du poste. Elle est utilisée **uniquement pour le test** ; ce n'est pas une recommandation de configuration de production.

Enregistrer la stratégie, synchroniser le poste et consulter les rapports Intune.

### Résultat observé

Le poste `FRANCK` est passé à **Non conforme**. Dans le détail de la stratégie `LAB-CONFORMITE-WIN11` :

| Paramètre évalué | État observé |
|---|---|
| Pare-feu | Conforme |
| Antivirus | Conforme |
| Microsoft Defender Antimalware | Conforme |
| Version minimale du système d'exploitation | **Non conforme** |

**Diagnostic :** l'échec provenait exclusivement de la version minimale exigée. Le pare-feu et les contrôles antivirus restaient conformes.

## Remédiation et validation finale

1. Ouvrir `LAB-CONFORMITE-WIN11` > **Propriétés** > **Paramètres de conformité** > **Modifier**.
2. Dans **Propriétés de l'appareil**, remettre **Version minimale du système d'exploitation** à **Non configuré**.
3. Enregistrer, synchroniser `FRANCK`, attendre la nouvelle évaluation et actualiser le rapport.
4. Vérifier le retour de l'appareil à l'état **Conforme**.

**Résultat final observé : FRANCK de nouveau Conforme.**

## Enseignements

- Une stratégie de conformité évalue un état ; elle ne remplace pas une stratégie de configuration.
- Les rapports par paramètre permettent d'identifier la cause exacte d'une non-conformité.
- Une modification de stratégie et une synchronisation peuvent nécessiter un délai avant la mise à jour du statut.
- Une simulation maîtrisée permet de valider le cycle **Conforme → Non conforme → Diagnostic → Correction → Conforme** sans désactiver les protections de sécurité.

## Confidentialité

Cette documentation ne contient ni identifiant de tenant, ni adresse de compte, ni identifiant unique d'appareil, ni numéro de série, ni secret. Ne publier des captures d'écran qu'après anonymisation.

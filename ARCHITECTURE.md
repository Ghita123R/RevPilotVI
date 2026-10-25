# RevPilot — Architecture cible proposée

## Principe

Avant d'appliquer cette architecture, l'agent doit inspecter le dépôt et préserver la stack existante. Si le dépôt est vide, proposer et faire valider une stack. Ne pas considérer les choix ci-dessous comme des décisions déjà prises.

## Couches recommandées

1. **Interface** : dashboard exécutif, écrans modules, filtres, tableaux, visualisations, formulaires et exports.
2. **Application / services** : cas d'usage tels que valider facture, calculer revenu reconnu, clôturer un mois, générer forecast, classifier NEPI, enregistrer attrition.
3. **Domaine financier** : fonctions pures testées pour proratisation, rapprochements, forecast, NEPI, churn/attrition et consolidation.
4. **Persistance** : référentiel client, contrats, factures, revenus reconnus, budgets, versions forecast, NEPI, attrition, alertes et audit.
5. **Intégrations** : import Excel, extraction PDF, espace connecté, génération PDF/Excel, modèle IA.
6. **Sécurité / observabilité** : authentification, rôles, audit, logs, erreurs d'import et état des intégrations.

## Flux de données de référence

Import manuel/PDF/Excel
→ normalisation et validation
→ client / BU / contrat / facture
→ moteur de revenu reconnu
→ contrôles et rapprochement Excel
→ revenu mensuel et restant à reconnaître
→ forecast et alertes
→ NEPI et attrition à examiner
→ dashboard et exports.

La validation est un point de contrôle explicite. Aucun import brut ne doit alimenter les chiffres officiels sans validation métier.

## Principes d'implémentation

- Moteur de calcul déterministe et indépendant de l'IA.
- Import idempotent : réimporter le même fichier ne doit pas doubler les factures ou les revenus.
- Historisation et versions.
- Traçabilité par source et par règle.
- API ou fonctions de service partagées par les vues ; pas de logique de calcul dupliquée dans les composants.
- Gestion explicite des erreurs, champs manquants, conflits et doublons.
- Tests unitaires sur les calculs ; tests d'intégration sur import, validation et propagation.
- Les données de démo sont synthétiques et marquées comme telles.

# RevPilot — Plan d'implémentation recommandé

Le plan est ordonné par dépendances. L'agent doit le réconcilier avec l'état réel du dépôt avant de commencer.

## Phase 0 — Audit du dépôt
- Détecter stack, structure, scripts, déploiement GitHub Pages éventuel, secrets, tests et composants existants.
- Capturer l'état initial et identifier les fonctionnalités déjà implémentées.
- Livrer un plan technique et une liste des écarts, sans refonte non demandée.

## Phase 1 — Fondations
- Référentiel canonique des BU, clients, produits/modules et périodes.
- Mapping CSTLO/Constellio configurable ; ne pas décider silencieusement.
- Modèle d'audit, statuts et import batch.
- Données de démonstration clairement étiquetées.
- Fonctions de formatage, filtres et gestion des états incomplets.

## Phase 2 — Module 5 : Contrats & Factures
- Registre de factures, contrats/BDC et pièces source.
- Import Excel et saisie manuelle.
- Détection de doublons, validations et workflow.
- PDF extraction et surveillance de dossier selon intégrations disponibles.
- Avoirs et factures remplacées.
- Tests d'idempotence et d'historique.

## Phase 3 — Module 1 : Monthly Revenue Overview
- Moteur de reconnaissance par période de service.
- Proratisation mensuelle et journalière selon règle autorisée.
- Revenu reconnu et restant à reconnaître.
- Comparaison Excel vs RevPilot.
- Analyse des variations et contrôles de clôture.
- Tests de sommes, arrondis, dates inclusives et dénominateurs nuls.

## Phase 4 — Module 4 : Clients
- Fiche client et contacts.
- Segmentation, statut historisé, classement et revenus.
- Comptage unique dans le GED consolidé.
- Contrôles de doublons et données manquantes.

## Phase 5 — Module 2 : Revenue Forecast
- Forecast sécurisé/non sécurisé.
- Indexation manuelle client/année.
- Budget vs forecast vs réalisé.
- Forecast initial figé, versions et accuracy.
- Rolling forecast 12 mois, risques et simulations séparées.

## Phase 6 — Module 3 : NEPI
- Comparaison N vs même mois N-1.
- Classification utilisateur et justification.
- Synthèse par BU et GED consolidé.
- Réconciliation et exports.
- Tests : seule la catégorie prix validée entre dans le NEPI.

## Phase 7 — Module 6 : Attrition
- Registre de départs, raison manuelle, date, dernière facture payée.
- Montant mensuel perdu et annualisé si validé.
- Rapports par BU/année/période.
- Synchronisation avec clients, forecast, NEPI et dashboard.
- Tests de perte partielle vs client globalement perdu.

## Phase 8 — Dashboard principal
- Revenue Pulse avec composantes visibles.
- Revenue Gap, Revenue at Risk, Forecast Confidence.
- Trajectoire, waterfall/NEPI, secure vs unsecured, classement BU.
- Alert Center et navigation vers détails.
- Morning Brief, Explain My Numbers et Ask RevPilot avec sources.
- Rapport exécutif PDF.
- Tests cohérence KPI vs modules sources.

## Phase 9 — Durcissement et mise en production
- Rôles et permissions.
- Tests d'intégration et tests de bout en bout.
- Accessibilité et responsive.
- Sécurité des documents et secrets.
- Gestion des erreurs, logs, backups et migrations.
- Déploiement selon l'environnement réel du dépôt.

## Définition de « terminé »
Une fonctionnalité est terminée uniquement si : elle respecte la règle métier, gère les cas limites, possède des tests pertinents, conserve la traçabilité, est documentée et n'affiche pas de chiffres fictifs comme des données réelles.

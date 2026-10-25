# RevPilot — Spécification fonctionnelle maître

Version documentaire : 1.0  
Statut : consolidation des cahiers des charges transmis. Les décisions métier ouvertes sont listées dans `DECISIONS_TO_CONFIRM.md`.

## 1. Vision du produit

RevPilot est un agent IA de pilotage des revenus SaaS. Il centralise les données de facturation, calcule le revenu reconnu, prépare le forecast, analyse les mouvements NEPI, suit le portefeuille client et l'attrition, contrôle les contrats et factures et fournit un cockpit de décision exécutif.

L'agent doit aider l'utilisateur à répondre à cinq questions :
1. Où en sont les revenus ?
2. Où vont-ils évoluer ?
3. Quels mouvements expliquent les écarts ?
4. Quels revenus et clients sont exposés à un risque ?
5. Quelles actions et décisions faut-il prendre ?

## 2. Principes transverses

- Source de vérité commune : les modules partagent les mêmes identifiants client, BU, facture, contrat, période et revenu.
- Traçabilité : chaque chiffre doit pouvoir être relié à sa source, à sa période et à la règle de calcul.
- Séparation des concepts : facturé ≠ payé ≠ reconnu ≠ restant à reconnaître ≠ forecast.
- Contrôle humain : les données importées ou extraites sont vérifiées avant validation financière.
- Historisation : conserver les versions de forecast, les validations, les corrections et les imports.
- IA explicable : l'IA résume et explique des résultats calculés ; elle ne crée pas de causes financières non démontrées.
- Consolidation sans double comptage.
- Les valeurs manquantes et incohérences doivent être signalées, jamais remplacées silencieusement.

## 3. Périmètre et navigation

Navigation principale proposée :
- Dashboard principal
- Monthly Revenue Overview
- Revenue Forecast
- NEPI
- Clients
- Contrats & Factures
- Attrition

Les filtres globaux peuvent inclure période, BU, client, produit/module, devise et comparaison. Chaque module peut ajouter des filtres propres. Les filtres actifs doivent être visibles et repris dans les exports et les réponses IA.

Business Units mentionnées dans les spécifications : `SAAS CSP`, `SAAS CSTLO`, `SAAS SYGED`; les documents Clients mentionnent aussi `Constellio`. Le mapping officiel CSTLO/Constellio doit être confirmé avant mise en production. Le périmètre GED/NEPI exclut Legal selon la spécification NEPI, sauf décision métier ultérieure explicite.

## 4. Dashboard principal — cockpit exécutif

Fonctionnalités sélectionnées :
1. Revenue Pulse
2. Revenue Gap
3. Revenue Growth Engine
4. Forecast Confidence
5. Revenue at Risk
6. Revenue Waterfall / NEPI
7. Revenue Trajectory sur 12 mois
8. Secure vs Unsecured Revenue
9. RevPilot Morning Brief
10. Explain My Numbers
11. Ask RevPilot
12. Alertes financières prioritaires
13. BU Performance League
14. What-if Simulator
15. One-Click Executive Report

### KPI et règles
- **Revenue Pulse** : score de santé synthétique sur 100, avec composantes visibles et pondérations configurables. Proposition initiale : respect du budget, dynamique de croissance, couverture sécurisée, risque revenu et qualité des données. Les poids/seuils doivent être validés ; score non calculable si les données sont insuffisantes.
- **Revenue Gap** : forecast de période − budget de période. Gap % = (forecast − budget) / budget × 100 si budget non nul.
- **Revenue Growth Engine** : variation revenu N − revenu de comparaison ; ventilation par New Client, Lost Client, New Module, Lost Module, NEPI / Variation de prix et Other / To Validate.
- **Forecast Confidence** : afficher séparément la couverture sécurisée, la qualité des données et, si disponible, la précision historique. Le taux sécurisé n'est pas une probabilité de réalisation.
- **Revenue at Risk** : montant des revenus futurs exposés à des risques identifiés. Dédupliquer les montants ; distinguer montant brut exposé et montant pondéré si une méthode est validée.
- **Revenue Waterfall / NEPI** : pont entre revenu de départ et revenu d'arrivée. NEPI prix = somme des mouvements explicitement classés et validés comme effet prix.
- **Revenue Trajectory** : graphique mensuel 12 mois avec réalisé, forecast et budget, sans présenter le forecast comme du réalisé.
- **Secure vs Unsecured** : total forecast = sécurisé + non sécurisé, sans double compte.
- **Morning Brief** : synthèse courte sur la situation, les écarts, risques, changements et actions. Chaque chiffre doit provenir des calculs disponibles.
- **Explain My Numbers** : expliquer la définition, la formule, les contributeurs et les lignes source d'un KPI.
- **Ask RevPilot** : interface conversationnelle limitée aux données et droits de l'utilisateur ; réponses avec période, périmètre et sources.
- **Alertes** : forecast sous budget, baisse de revenus, contrat expirant, facture attendue manquante, anomalie de rapprochement, NEPI à valider et données non fiables. Seuils configurables.
- **BU Performance League** : comparaison de BU avec définitions et périodes identiques ; classement par KPI visible, pas de score opaque.
- **What-if Simulator** : tester indexation, renouvellement, perte client, date de démarrage ou autres hypothèses sans modifier le forecast officiel.
- **One-Click Executive Report** : PDF avec résumé, KPI, trajectoire, waterfall, couverture sécurisée, alertes, performance BU, commentaire IA sourcé et annexes facultatives.

## 5. Module 1 — Monthly Revenue Overview

### Objectif
Calculer, contrôler et analyser le revenu reconnu par mois à partir des factures et des périodes de service. Importer le classeur Excel existant, notamment les onglets `SAAS CSP`, `SAAS CSTLO`, `SAAS SYGED`, sans ressaisie inutile. Conserver les colonnes mensuelles du fichier source comme référence de comparaison.

### Données source principales
BU, client, produit/plan, numéro de facture, date de facturation, fréquence de facturation, début et fin de service, montant HT, indicateur de période partielle, durée en mois, statut contrat, statut paiement, remarques et montants mensuels de référence.

### Reconnaissance du revenu
- Période complète avec répartition linéaire autorisée : revenu mensuel = montant HT pris en compte / nombre de mois de service.
- Période partielle avec proratisation journalière autorisée : revenu du mois = montant HT × jours de service du mois / jours totaux de la période. Début et fin inclus.
- Afficher la méthode appliquée pour chaque facture/mois.
- Contrôler la somme reconnue, les arrondis, les avoirs et corrections.
- Calculer le revenu reconnu jusqu'à une date et le revenu restant à reconnaître.

### Contrôles et analyses
- Excel vs RevPilot : montant de référence, montant recalculé, écart et statut conforme/écart/comparaison impossible. Écart = calcul RevPilot − référence Excel. Pas de correction automatique sans justification.
- Comparer le mois au mois précédent et au même mois de N-1.
- Variation = revenu période analysée − revenu période de comparaison.
- Variation % = variation / revenu de comparaison × 100, seulement si le dénominateur est non nul ; sinon afficher « Non calculable ».
- Identifier hausses, baisses, BU, clients et produits contributeurs.
- Filtres combinables : année, mois, BU, produit/plan, client.
- Préparer une clôture mensuelle traçable, avec anomalies et commentaires financiers.

## 6. Module 2 — Revenue Forecast

### Objectif
Prévision mensuelle et annuelle, sécurisée et non sécurisée, mesure des risques, comparaison forecast/réalisé et aide à la décision.

### Règles métier
- Exemple source : facture `INV-005248`, durée de référence 12 mois ; dates exactes à confirmer.
- Forecast sécurisé : factures existantes et périodes de service restantes à reconnaître.
- Forecast non sécurisé : renouvellements non confirmés et hypothèses futures, y compris indexation projetée hors période couverte.
- Taux d'indexation saisi manuellement par client et par année.
- Le forecast initial est figé et sert de référence de précision.
- Forecast total = sécurisé + non sécurisé.
- Ne pas compter deux fois un même revenu sur la même période.

### Fonctionnalités V1
- Executive Dashboard
- Revenue Bridge
- Revenue at Risk
- Simulation de scénarios
- Forecast Accuracy
- Rolling Forecast 12 mois
- Budget vs Forecast vs Realized
- Version History
- Smart Alerts
- AI Forecast Analyst

Les simulations sont séparées du forecast officiel jusqu'à validation. Les versions historiques sont conservées.

## 7. Module 3 — NEPI (Net Effective Price Increase)

### Objectif
Processus mensuel pour comparer le mois N au même mois de N-1, classer les variations, calculer les contributions, rapprocher les totaux et produire les exports.

### Catégories
- `New Client`
- `Lost Client`
- `New Module`
- `Lost Module`
- `NEPI / Variation de prix`
- `Other / To Validate`

### Règles
- Variation signée = revenu N − revenu N-1.
- La catégorie est choisie par l'utilisateur via liste déroulante ; commentaire/justification lorsque nécessaire.
- NEPI = somme des seuls mouvements explicitement classés `NEPI / Variation de prix`, selon les règles métier validées.
- Le résiduel inexpliqué ne devient jamais automatiquement un effet prix.
- Vue par BU : CSP, CSTLO, SYGED ; plus NEPI GED consolidé.
- Legal est explicitement exclu du NEPI GED.
- Le consolidé est calculé à partir des lignes détaillées, sans addition de sous-totaux qui doubleraient le résultat.
- Contrôler les lignes non classées, les doublons, les clients/modules non appariés, les devises et la réconciliation.
- Exports : Excel détaillé et PDF de synthèse mensuelle.

## 8. Module 4 — Clients

### Objectif
Référentiel client unique et dashboard portefeuille.

### Fonctions
- Compter les clients par SYGED, CSP, Constellio et GED consolidé.
- Top clients par revenu et concentration du portefeuille.
- Créer, consulter et modifier une fiche client.
- Type de client, secteur, modules/produits, BU et historique annuel.
- Statuts nouveaux, actifs, inactifs et perdus, avec règles explicites.
- Contact principal avec fonction et téléphone ; contacts secondaires en option.
- Filtres par année, BU, secteur, type et module.
- Un client ne doit pas être compté deux fois dans le GED consolidé.
- Garder un historique daté des statuts pour ne pas appliquer rétroactivement le statut actuel aux années précédentes.
- Mapping CSTLO/Constellio à confirmer. Ne pas inclure Legal dans le GED consolidé sans décision explicite.

## 9. Module 5 — Contrats & Factures

### Objectif
Point d'entrée documentaire et contractuel. Centraliser contrats, bons de commande (BDC), factures, avoirs et pièces justificatives, puis propager les données validées aux autres modules.

### Champs
Client sélectionné depuis le référentiel, BU, numéro de contrat/BDC, dates, montant contractuel, fréquence, période de service, numéro et date de facture, montant, devise, type de pièce, fichier source, statut, auteur, date et historique.

### Entrées
- Saisie manuelle
- Import PDF avec extraction intelligente et confirmation utilisateur
- Import Excel
- Dépôt automatique dans un espace connecté si la connexion est disponible

### Workflow
Brouillon/importé → à vérifier → validé ou rejeté → annulé/remplacé si nécessaire. Les données n'alimentent pas définitivement les KPI avant les contrôles requis.

### Automatisations retenues
- Synchronisation après validation vers Revenue Overview, Forecast, fiche Client et alertes.
- Moteur de reconnaissance du revenu par période de service.
- Suivi contrat/BDC, échéances et renouvellements.
- Contrôle contrat vs facture.
- Détection de doublons, avoirs et factures remplacées.
- Rapprochement facturé vs reconnu vs restant à reconnaître.
- Enrichissement de la fiche client.
- Signaux de mouvement à examiner pour NEPI, sans classification automatique de l'effet prix.
- Alertes de clôture et conservation de l'historique du forecast.
- La surveillance automatique d'un dossier et les fonctions IA plus avancées peuvent être V2 si les intégrations ne sont pas disponibles en V1.

### Règle d'or
Une facture validée déclenche les recalculs nécessaires, sans double comptage ni écrasement silencieux de l'historique.

## 10. Module 6 — Attrition

### Objectif
Registre centralisé des clients perdus, date de départ, dernière facture payée, montant perdu et raison de départ saisie manuellement. Analyse par BU et période avec rapports PDF/Excel.

### Champs par événement
Client, identifiant unique, BU réellement perdue, modules concernés, date effective de départ, date de résiliation si différente, dernière facture payée (référence, date de paiement, montant et période si disponibles), montant perdu, raison, commentaire, statut de validation, justificatif, auteur et historique.

### Raisons proposées
Prix/budget, produit/fonctionnalités, service/support, concurrent, fin d'activité/changement client, non-renouvellement, relation commerciale, autre, inconnue/à confirmer. « Autre » nécessite un commentaire. La raison doit être manuellement renseignée et validée avant de finaliser le dossier ; « inconnue » reste visible comme dossier incomplet.

### KPI
- Nombre de clients perdus uniques sur la période
- Revenu mensuel perdu
- Revenu annualisé perdu (si retenu, revenu mensuel × 12)
- Revenu moyen perdu par client
- Taux d'attrition clients, si la base des clients actifs au début de période est définie
- Taux d'attrition revenu, si la base de revenu récurrent est fiable
- Raisons manquantes
- Top causes par nombre de clients et par revenu perdu

### Précautions
- Ne pas prendre la dernière facture payée comme montant perdu par défaut.
- Ne pas assimiler une facture émise à une facture payée.
- Une perte de module n'est pas automatiquement une attrition totale du client.
- Dédupliquer les clients dans les totaux consolidés.
- Exports PDF et Excel par BU, année ou période personnalisée.
- Synchroniser les départs validés vers Clients, NEPI, Forecast et Dashboard sans effacer l'historique.

## 11. Workflows transverses

### Import de facture
1. Importer le fichier ou saisir manuellement.
2. Extraire/normaliser les champs.
3. Associer client, BU, contrat et période.
4. Détecter champs manquants, doublons et incohérences.
5. Laisser l'utilisateur confirmer les champs incertains.
6. Valider ou rejeter.
7. Recalculer le revenu reconnu et les vues concernées.
8. Mettre à jour les signaux NEPI et alertes.
9. Conserver le fichier source, l'auteur, l'heure et l'historique.

### Clôture mensuelle
1. Vérifier l'exhaustivité des factures.
2. Calculer revenu reconnu et restant à reconnaître.
3. Contrôler Excel vs RevPilot.
4. Vérifier anomalies et doublons.
5. Préparer le NEPI et classer les variations.
6. Rapprocher les totaux.
7. Produire les commentaires/rapports.
8. Valider et figer la version de clôture selon les droits.

## 12. Règles de consolidation et qualité

- Client unique : identifiant stable ; nom normalisé seul ne suffit pas pour une fusion automatique.
- Facture unique : clé composée à définir selon la source (émetteur, BU, numéro, date, montant ou identifiant source) et contrôle manuel des collisions.
- BU canonique : valeurs autorisées, alias séparés et mapping documenté.
- GED consolidé : agrégation des lignes détaillées ; jamais somme de vues déjà consolidées.
- Montants : conserver la devise source et préciser la règle de conversion si plusieurs devises sont admises.
- Périodes : dates explicites, conventions de début/fin documentées.
- Chaque donnée importée garde la référence de fichier et la date d'import.
- Toute correction manuelle est historisée.
- Les statuts historiques sont datés.
- Les données manquantes sont affichées comme manquantes, pas remplacées par zéro sans règle explicite.

## 13. Rôles et droits — proposition à valider

- Viewer : consulter les données autorisées.
- Analyst : importer, préparer, classifier et proposer des corrections.
- Approver / Finance Lead : valider factures, clôtures, NEPI et changements officiels du forecast.
- Admin : gérer les paramètres, les utilisateurs, les intégrations et les mappings.

Cette matrice est une proposition de sécurité, pas une règle métier déjà validée. Les droits existants du dépôt priment.

## 14. Livrables et exports

- Module 1 : tableaux et contrôles mensuels, Excel vs RevPilot, revenu restant à reconnaître.
- Forecast : tableaux, versions, scénarios, comparaison au budget et au réalisé.
- NEPI : classeur détaillé et PDF mensuel.
- Clients : vues de portefeuille et exports filtrés selon les besoins à préciser.
- Contrats & Factures : registre, documents sources, statuts et rapports de contrôle.
- Attrition : PDF/Excel par BU et année ou période.
- Dashboard principal : rapport exécutif PDF en un clic.

## 15. Non-objectifs et garde-fous

- Ne pas inventer des chiffres de production ni prétendre qu'une intégration est active si elle n'est pas configurée.
- Ne pas utiliser l'IA pour remplacer les calculs financiers déterministes.
- Ne pas transformer automatiquement des écarts non expliqués en effets prix.
- Ne pas faire évoluer un forecast officiel à partir d'une simulation non approuvée.
- Ne pas marquer une facture comme payée sans preuve ou confirmation.
- Ne pas fusionner des clients automatiquement sur simple similarité de nom.

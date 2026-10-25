# Prompt à copier dans l'agent IA GitHub — RevPilot

Tu travailles sur le dépôt RevPilot. Ta mission est de construire et faire évoluer une application SaaS de pilotage des revenus selon les spécifications présentes dans le dépôt.

**Avant de coder :**
1. Lis `AGENTS.md`, `docs/MASTER_SPEC.md`, `docs/DOMAIN_MODEL.md`, `docs/ARCHITECTURE.md`, `docs/IMPLEMENTATION_PLAN.md`, `docs/DECISIONS_TO_CONFIRM.md` et les documents source concernés.
2. Inspecte la stack et les fonctionnalités existantes. Ne remplace pas les choix déjà implémentés sans justification.
3. Présente un audit bref : ce qui existe, ce qui manque, les risques et l'incrément que tu proposes.
4. Si une règle métier importante est à confirmer, ne l'invente pas. Propose une question ou isole le paramètre.

**Objectif produit :**
Implémenter un cockpit financier relié à six modules :
- Monthly Revenue Overview
- Revenue Forecast
- NEPI
- Clients
- Contrats & Factures
- Attrition
et un dashboard principal comprenant Revenue Pulse, Revenue Gap, Revenue Growth Engine, Forecast Confidence, Revenue at Risk, Revenue Waterfall/NEPI, Revenue Trajectory 12 mois, Secure vs Unsecured, Morning Brief, Explain My Numbers, Ask RevPilot, alertes prioritaires, BU Performance League, What-if Simulator et One-Click Executive Report.

**Contraintes métier obligatoires :**
- Revenu reconnu selon la période de service, distinct du montant facturé et payé.
- Forecast total = sécurisé + non sécurisé ; ne pas compter deux fois.
- Forecast initial figé ; versions historisées ; simulations isolées du forecast officiel.
- NEPI prix uniquement à partir de mouvements explicitement classés et validés en `NEPI / Variation de prix`.
- Legal exclu du NEPI GED selon la spécification actuelle.
- Attrition : raison manuelle, montant perdu traçable, dernière facture payée uniquement si statut de paiement fiable.
- Imports contrôlés avant propagation aux KPI officiels.
- Une seule source de vérité et pas de double comptage en consolidation.
- L'IA explique les calculs existants, ne fabrique pas de chiffres ni de causes.

**Approche de développement :**
Travaille par incréments verticaux, en suivant `docs/IMPLEMENTATION_PLAN.md`. Pour chaque incrément :
- définis les critères d'acceptation ;
- implémente la logique métier hors des composants UI ;
- ajoute les tests nécessaires ;
- vérifie les états d'erreur, vides et données incomplètes ;
- documente les changements ;
- lance les tests disponibles ;
- résume les fichiers modifiés et les limites restantes.

Ne déclare pas une fonctionnalité terminée si elle utilise uniquement des données de démonstration, si elle n'a pas de tests pertinents ou si elle n'est pas reliée au modèle de données attendu. Si le dépôt est vide, propose d'abord une stack et un plan d'architecture adaptés avant de créer l'application.

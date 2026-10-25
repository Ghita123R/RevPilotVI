# RevPilot — GitHub AI Development Starter

RevPilot est un projet d'agent IA de pilotage financier SaaS. Ce dépôt regroupe les spécifications métier et les instructions de travail destinées à un agent de développement IA (GitHub Copilot, Copilot coding agent ou autre agent compatible).

> **Important :** ce dossier est le référentiel fonctionnel et un kit de démarrage documentaire. Il ne constitue pas à lui seul une application web déjà implémentée ou connectée à des données réelles.

## Démarrage rapide

1. Ajouter le contenu de ce dossier à la racine du dépôt GitHub RevPilot, ou importer le ZIP dans un nouveau dépôt.
2. Ouvrir le dépôt avec l'agent de développement IA utilisé.
3. Lui faire lire `AGENTS.md`, puis `docs/MASTER_SPEC.md`, `docs/DOMAIN_MODEL.md` et `docs/IMPLEMENTATION_PLAN.md`.
4. Lui demander de commencer par **inspecter le code existant** et de proposer un plan avant de modifier l'application.
5. Développer par étapes verticales, avec tests et validation à chaque étape. Ne pas demander à l'agent de générer tous les modules en une seule opération.

## Fichiers principaux

- `AGENTS.md` — instructions permanentes pour l'agent IA.
- `.github/copilot-instructions.md` — instructions pour GitHub Copilot.
- `docs/MASTER_SPEC.md` — spécification fonctionnelle consolidée des modules.
- `docs/DOMAIN_MODEL.md` — entités, relations et règles de cohérence des données.
- `docs/ARCHITECTURE.md` — architecture cible proposée, à adapter à la stack existante.
- `docs/IMPLEMENTATION_PLAN.md` — ordre de réalisation recommandé.
- `docs/DECISIONS_TO_CONFIRM.md` — décisions métier non résolues à faire valider.
- `docs/TEST_PLAN.md` — tests fonctionnels et critères d'acceptation.
- `docs/AI_AGENT_PROMPT.md` — prompt prêt à copier dans l'agent IA.
- `docs/source-specifications/` — cahiers des charges source fournis pour préserver la référence d'origine.

## Modules inclus

1. Dashboard principal — cockpit exécutif et IA.
2. Module 1 — Monthly Revenue Overview.
3. Module 2 — Revenue Forecast.
4. Module 3 — NEPI.
5. Module 4 — Clients.
6. Module 5 — Contrats & Factures.
7. Module 6 — Attrition.

## Principe de travail

Les règles métier des documents source priment sur les suppositions de l'agent. Les points indiqués comme « à confirmer » doivent rester configurables ou être soumis à validation ; l'agent ne doit pas les trancher silencieusement.

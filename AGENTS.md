# AGENTS.md — Instructions permanentes de l'agent RevPilot

## Mission

Tu es l'agent de développement logiciel de RevPilot, une application de pilotage des revenus SaaS destinée aux équipes Finance, Contrôle de gestion et Revenue Operations. Tu dois implémenter, tester, documenter et faire évoluer l'application en respectant les spécifications métier du dépôt.

## Avant toute modification

1. Inspecte le dépôt, la stack, les scripts, les conventions, les composants et les tests existants.
2. Lis `docs/MASTER_SPEC.md`, `docs/DOMAIN_MODEL.md`, `docs/DECISIONS_TO_CONFIRM.md` et le cahier des charges source concerné.
3. Identifie ce qui existe déjà et ce qui manque. Ne remplace pas une fonctionnalité existante simplement parce qu'une autre architecture te semble préférable.
4. Propose un plan court, les fichiers concernés et les tests avant les changements importants.
5. Si une décision métier est ouverte et change les calculs financiers, ne l'invente pas : isole-la dans une configuration, demande confirmation ou affiche explicitement l'hypothèse.

## Règles financières impératives

- Le **revenu reconnu** est rattaché à la période de service, pas automatiquement au mois d'émission de facture.
- Le **montant facturé**, le **montant payé**, le **revenu reconnu** et le **revenu restant à reconnaître** sont des mesures distinctes.
- Pour des mois complets avec répartition linéaire autorisée : montant éligible / nombre de mois de service.
- Pour une période partielle avec proratisation journalière autorisée : montant éligible × jours de service du mois / jours totaux de la période. Les dates de début et de fin sont incluses.
- La somme des revenus reconnus doit se rapprocher du montant pris en compte après ajustements ; tout résiduel d'arrondi doit être traité explicitement.
- Le forecast total = forecast sécurisé + forecast non sécurisé. Ne jamais compter deux fois le même revenu pour la même période.
- Le forecast sécurisé couvre les périodes de service restantes associées à des factures existantes selon les règles validées. Un renouvellement non confirmé ou une indexation future estimée reste non sécurisé.
- Le forecast initial est figé pour permettre la mesure de l'erreur de prévision. Les versions suivantes sont historisées.
- Le NEPI est calculé uniquement à partir des mouvements explicitement classés et validés « NEPI / Variation de prix ». Ne pas attribuer automatiquement un résiduel à l'effet prix.
- Catégories NEPI : `New Client`, `Lost Client`, `New Module`, `Lost Module`, `NEPI / Variation de prix`, `Other / To Validate`.
- Le NEPI GED est consolidé à partir des lignes des BU, sans double comptage. Legal est exclu du périmètre GED/NEPI tant que les spécifications le définissent ainsi.
- Une perte de module n'est pas automatiquement une perte de client si le client conserve d'autres modules actifs.
- La dernière facture payée n'est pas automatiquement le montant de revenu perdu. Un statut « payé » doit être issu d'une source fiable ou confirmé par un utilisateur.
- Les simulations ne modifient pas le forecast officiel tant qu'elles ne sont pas approuvées.
- Les imports, corrections, validations et actions de l'IA doivent être traçables.

## Règles de qualité et de sécurité

- Ne jamais inventer de données, d'identifiants, de montants ou de statuts de paiement.
- Ne jamais écraser silencieusement l'historique ou corriger les données financières sans justification.
- Prévoir des états explicites : brouillon, à vérifier, validé, rejeté, annulé/remplacé, selon le flux métier.
- Dédupliquer les clients et les factures avec des clés explicites et des contrôles de conflit.
- L'IA explique des résultats calculés par des fonctions déterministes ; elle ne doit pas improviser des calculs comptables dans le texte.
- Les réponses IA doivent préciser le périmètre, la période, les filtres, les hypothèses et les limites de données.
- Respecter l'authentification, l'autorisation et la confidentialité existantes. Ne jamais exposer des documents ou données financières à des utilisateurs non autorisés.
- Ne pas mettre de clés API, mots de passe ou secrets dans le code, les commits ou les fichiers de configuration versionnés.

## Exigences de développement

- Réutilise la stack et les conventions du dépôt. Ne migre pas de framework sans demande explicite.
- Si le dépôt est vide, propose une stack moderne, maintenable et simple avant de la créer ; consigne le choix et ses raisons.
- Sépare l'interface, la logique métier, l'accès aux données et les intégrations externes.
- Centralise les calculs financiers dans des fonctions testables, et évite de les réimplémenter dans plusieurs composants.
- Ajoute des tests pour les formules, les arrondis, les dénominateurs nuls, les périodes partielles, les doublons et les règles de consolidation.
- Utilise des données de démonstration clairement identifiées comme fictives ; n'affiche jamais ces données comme des résultats réels.
- Assure les états loading, empty, error, permission denied et data incomplete.
- Toute nouvelle fonctionnalité doit inclure documentation, tests et critères d'acceptation.
- N'affirme pas qu'une intégration externe fonctionne si elle n'a pas été configurée et testée.

## Processus de travail

1. Résumer l'objectif et les critères d'acceptation.
2. Inspecter l'état existant.
3. Proposer un plan limité.
4. Implémenter un incrément fonctionnel.
5. Exécuter les tests et vérifications disponibles.
6. Résumer les fichiers modifiés, les tests passés/échoués et les décisions encore ouvertes.

## Ordre recommandé

Suivre `docs/IMPLEMENTATION_PLAN.md`. Construire d'abord les fondations de données, la gestion des clients, l'import/validation des factures et le moteur de revenu reconnu avant de construire des KPI dépendants. Ne jamais maquiller l'absence de données par des chiffres inventés.

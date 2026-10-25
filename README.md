# RevPilot — Prototype GitHub Pages

Prototype interactif en français basé sur le classeur `SaaS_RevPilot_Donnees_Fictives_2026_2027.xlsx`. Toutes les données sont fictives.

## Déployer sur GitHub Pages
1. Dans le dépôt `Ghita123R/RevPilot`, téléversez `index.html`, le dossier `data/` et le classeur Excel à la racine du dépôt.
2. Committez les changements sur `main`.
3. Dans Settings → Pages, sélectionnez Deploy from a branch, `main`, `/(root)`.
4. Attendez le déploiement puis ouvrez `https://ghita123r.github.io/RevPilot/`.

## Contenu
- `index.html` : interface responsive, navigation entre dashboard et modules, filtres BU/année, graphiques et interactions locales.
- `data/revpilot-demo-data.json` : données structurées dérivées du classeur Excel et quelques événements d’attrition fictifs supplémentaires.
- `SaaS_RevPilot_Donnees_Fictives_2026_2027.xlsx` : source de démonstration.

## Limites explicites
Ceci est un prototype front-end sans base de données ni authentification. Les ajouts dans les formulaires ne sont pas persistants. Les fonctions IA sont simulées localement, les exports sont simplifiés et aucune facture PDF n'est réellement importée ou validée. Les montants de factures ne sont pas présentés comme revenu reconnu. Les règles financières doivent être validées avant utilisation réelle.

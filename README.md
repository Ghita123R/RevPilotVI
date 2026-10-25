# RevPilot — prototype alimenté par le classeur Excel SAAS GED

## Fichiers à publier
- `index.html`
- `data/revpilot-demo-data.json`
- `SAAS_GED_Gabarit_Base.xlsx`

Le fichier JSON doit se trouver à la racine du dépôt dans le dossier `data/`, au même niveau que `index.html`.

## Source des données
Les données proviennent des onglets `SAAS CSP`, `SAAS CSTLO` et `SAAS SYGED` du fichier Excel source. Les valeurs mensuelles utilisent les résultats des formules enregistrés dans le classeur. Les raisons NEPI et les raisons d'attrition absentes du classeur restent à compléter manuellement.

Les montants sont affichés en MAD. La partie forecast est une projection technique issue du planning du classeur, pas un budget ni un forecast officiel approuvé.

## Déploiement GitHub Pages
1. Décompressez l'archive.
2. Dans `Ghita123R/RevPilot`, téléversez `index.html`, le dossier `data/` et le classeur source.
3. Remplacez les anciens `index.html` et `data/revpilot-demo-data.json`.
4. Committez sur `main`.
5. Vérifiez Settings → Pages : `main` et `/ (root)`.
6. Testez `https://ghita123r.github.io/RevPilot/data/revpilot-demo-data.json`.
7. Rechargez le site avec `Ctrl+F5`.

## Limites
Prototype front-end, sans base de données, persistance des modifications ou IA réelle.

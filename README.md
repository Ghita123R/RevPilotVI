# RevPilot — Prototype complet basé sur le classeur Excel source

## Fichiers
- `index.html` : interface web du prototype.
- `data/revpilot-demo-data.json` : données préparées à partir du classeur source.
- `SAAS_GED_Gabarit_Base.xlsx` : fichier Excel source fourni.

## Source
Le prototype utilise les onglets `SAAS CSP`, `SAAS CSTLO` et `SAAS SYGED`. Les clients, contrats/lignes de facturation et revenus mensuels ont été extraits ou recalculés à partir des dates, montants et statuts présents dans le classeur. Les onglets `Synthèse` et `Contrôles` restent accessibles dans le fichier Excel.

Les raisons NEPI et les raisons d'attrition ne sont pas dans le classeur ; elles ne sont donc pas inventées. Le forecast issu des périodes de service est une projection technique, pas un budget ni un forecast officiel validé.

## Déploiement GitHub Pages
1. Décompressez l'archive.
2. Dans le dépôt `Ghita123R/RevPilot`, téléversez `index.html`, le dossier `data/` et le classeur Excel à la racine du dépôt.
3. Conservez exactement le chemin `data/revpilot-demo-data.json`.
4. Committez les changements sur `main`.
5. Vérifiez que Settings → Pages pointe vers la bonne branche et le dossier racine.
6. Testez `https://ghita123r.github.io/RevPilot/data/revpilot-demo-data.json`.
7. Rechargez le site avec `Ctrl+F5`.

## Limites du prototype
Pas de base de données, d'IA réelle ni de persistance des modifications. Les imports PDF/Excel et les règles finales de forecast/NEPI/attrition restent à implémenter et valider.

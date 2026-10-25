# RevPilot — Décisions métier à confirmer

L'agent ne doit pas masquer ou trancher ces points sans validation. Les suggestions sont des recommandations, pas des décisions acquises.

## Priorité bloquante

1. **Mapping BU** : quel est le mapping officiel entre `SAAS CSTLO` et `Constellio` ? S'agit-il d'un même périmètre ou de noms différents ?
2. **Périmètre GED** : confirmer si `Legal` reste exclu du total GED. Les documents NEPI l'excluent explicitement.
3. **Devise** : devise principale de reporting et règle de conversion si plusieurs devises sont présentes.
4. **Dates de service** : confirmer les conventions de proratisation, les règles d'arrondi et les traitements d'avoir.
5. **Forecast** : dates exactes de service pour l'exemple `INV-005248`, source officielle du budget et calendrier de gel du forecast initial.
6. **Sécurisé / non sécurisé** : valider les critères qui déterminent qu'un revenu est sécurisé.
7. **Attrition** : définir le revenu perdu officiel : revenu mensuel reconnu, revenu annualisé, ou les deux ; définir date effective de départ vs date de résiliation.
8. **Paiement** : identifier la source faisant autorité pour le statut « payé » et la date de paiement.

## Paramétrage important

9. Pondérations et seuils du Revenue Pulse.
10. Seuils d'alertes par montant, pourcentage, horizon et BU.
11. Méthode de calcul du Revenue at Risk et prévention du double comptage entre motifs de risque.
12. Raisons de départ définitives et rôle autorisé à valider l'attrition.
13. Règles du taux d'attrition clients et du taux d'attrition revenu.
14. Matrice de rôles : viewer, analyst, approver, admin (proposition).
15. Sources et format du fichier Excel existant, colonnes obligatoires et identifiants de facture.
16. Disponibilité et fournisseur de stockage connecté pour l'import automatique.
17. Fournisseur IA, politiques de données et limites d'accès aux données.
18. Format du rapport exécutif, identité visuelle et fréquence de génération.

## Politique par défaut tant que non confirmé

- Ne pas automatiser la règle contestée.
- Afficher « à confirmer » ou rendre le paramètre administrable.
- Conserver le résultat et la source utilisés pour chaque calcul.
- Ne pas afficher de probabilités de prévision sans modèle calibré.

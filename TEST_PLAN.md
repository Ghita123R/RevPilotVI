# RevPilot — Plan de tests fonctionnels

## Moteur de revenu reconnu
- Facture sur 12 mois complets : répartition correcte si la règle linéaire est activée.
- Période partielle : proratisation avec dates de début et fin incluses.
- Somme mensuelle égale au montant éligible après ajustements et arrondis.
- Avoir, facture annulée et facture remplacée traités sans double comptage.
- Champs dates ou montant manquants : blocage ou statut à vérifier, pas de substitution silencieuse.
- Comparaison Excel : différence = calcul RevPilot − montant de référence.
- Revenu restant à reconnaître cohérent avec le revenu total pris en compte moins le revenu reconnu à date.

## Forecast
- Forecast total = sécurisé + non sécurisé.
- Renouvellement non confirmé n'est pas sécurisé.
- Indexation manuelle appliquée au client et à l'année définis.
- Forecast initial immuable ; version nouvelle conserve son parent/historique.
- Une simulation n'altère pas le forecast officiel.
- Budget nul : pourcentage non calculable.
- Écart au réalisé et forecast accuracy cohérents avec les règles sélectionnées.

## NEPI
- Écart signé = revenu N − revenu N-1.
- Seules les lignes validées dans `NEPI / Variation de prix` contribuent au NEPI.
- `Other / To Validate` ne contribue pas au NEPI prix.
- Le total par BU se réconcilie avec les lignes détaillées.
- Le GED consolidé ne double-compte pas les sous-totaux et exclut Legal selon le périmètre source.
- Client ou module non apparié : alerte et revue manuelle.

## Clients et consolidation
- Un client associé à plusieurs BU ne compte qu'une fois dans le nombre de clients GED.
- Le statut historique à une date ne doit pas être remplacé par le statut actuel.
- Similarité de nom seule ne déclenche pas de fusion automatique.
- Les fiches client sont reliées aux factures, contrats, forecast, NEPI et attrition.

## Contrats & factures
- Réimport du même fichier : aucun doublon.
- Extraction PDF incertaine : confirmation manuelle requise.
- Facture validée : propagation correcte et auditée vers les modules concernés.
- Facture rejetée ou brouillon : n'alimente pas les KPI officiels.
- Avoir/remplacement : lien à la facture initiale et historique conservés.

## Attrition
- Date de départ et raison requises pour validation finale.
- Facture émise sans statut payé ne doit pas être présentée comme payée.
- Le montant perdu n'est pas automatiquement le montant de la dernière facture.
- Perte de module ne marque pas le client globalement perdu s'il reste actif ailleurs.
- PDF/Excel filtrés par BU et période reproduisent les données affichées.
- Consolidation évite les doublons d'un même client.

## Dashboard et IA
- Tous les KPI utilisent les mêmes filtres et périmètres que les vues sources.
- Les données provisoires sont étiquetées.
- L'IA cite période, périmètre et sources disponibles et indique les données insuffisantes.
- Aucune réponse IA ne fabrique des chiffres.
- Les alertes agrégées ne double-comptent pas le même montant exposé.
- Le rapport PDF reproduit les filtres, les KPI et la date de dernière mise à jour.

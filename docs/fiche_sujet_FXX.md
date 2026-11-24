# Fiche du sujet

## Intitulé
Prédiction du risque de défaut d'un demandeur de crédit (base « Crédit Allemagne »).

## Intérêt
Une banque prend un risque en accordant un crédit : refuser trop de dossiers fait perdre du chiffre d'affaires, en accepter trop de mauvais fait perdre du capital. Un modèle fondé sur les informations disponibles au moment de la demande aide le chargé de clientèle et le service risque / contrôle de gestion à décider d'accepter, de refuser ou de demander une garantie, et à prioriser les dossiers à examiner. Accorder un crédit à un mauvais payeur coûte plus cher que refuser un bon client : l'étude porte donc sur la détection de la classe « mauvais risque ».

## Problématique
À partir des 20 caractéristiques connues à la demande de crédit, peut-on identifier les clients à « mauvais risque » nettement mieux qu'une règle triviale, et quel algorithme y parvient le mieux ?

## Objectif mesurable (fixé avant l'évaluation)
- Métrique principale : F1-score de la classe « mauvais risque » (1).
- Métriques secondaires : rappel, précision, exactitude, AUC-ROC.
- Référence simple : « tout client est classé mauvais risque » (F1 ≈ 0,46 avec 30 % de mauvais risques).
- Jeu de test : 20 % des dossiers (200 clients), tirage stratifié, graine 42, jamais utilisé pour entraîner ni régler.
- Seuil de réussite : F1 (mauvais risque) ≥ 0,55 sur le test, soit au moins 9 points au-dessus de la référence.

## Nature du problème
Classification binaire déséquilibrée, supervisée : cible `mauvais_risque (0/1)` avec 700 bons risques (70 %) et 300 mauvais risques (30 %). Ni partitionnement, ni détection d'anomalies, ni réduction de dimension. Pas de dimension temporelle : découpage aléatoire stratifié.

## Données
- Fichier : `data/raw/base_credit_allemagne_1.xlsx` (feuilles `base_lisible`, `base_origine`, `dictionnaire`).
- 1 000 demandes de crédit de particuliers ; 7 variables numériques (durée en mois, montant en DM, âge en années…) et 13 catégorielles (compte courant, historique de crédit, objet du crédit, épargne, emploi…).
- Colonnes écartées : `n°` (identifiant) et `risque` (cible en texte, fuite de données).
- Limites : base ancienne, montants en Deutsche Mark, petit échantillon, période de collecte non précisée.

## Algorithmes étudiés
Régression logistique, KNN, Naive Bayes, arbre de décision, forêt aléatoire, SVM, Gradient Boosting, AdaBoost, XGBoost.

## Références
Voir [`references.md`](references.md).

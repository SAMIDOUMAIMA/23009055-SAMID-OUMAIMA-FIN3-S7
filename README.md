# Prédiction du risque de défaut sur la base « Crédit Allemagne »

Exploration d'algorithmes de Machine Learning supervisé (classification binaire déséquilibrée) sur 1 000 demandes de crédit.

**Auteur :** Samid Oumaima — ENCG Settat, 4e année, filière Finance
**Date :** novembre 2026

## Sujet et problématique

> À partir des 20 caractéristiques connues à la demande de crédit, peut-on identifier les clients à « mauvais risque » nettement mieux qu'une règle triviale, et quel algorithme y parvient le mieux ?

- **Cible :** `mauvais_risque (0/1)` — 1 = mauvais risque (300 clients), 0 = bon risque (700 clients).
- **Métrique principale :** F1-score de la classe « mauvais risque ».
- **Référence simple :** « tout client est classé mauvais risque » (F1 ≈ 0,46).
- **Seuil de réussite fixé avant l'évaluation :** F1 ≥ 0,55 sur un jeu de test de 20 % (tirage stratifié, graine 42).

La fiche détaillée du sujet est dans [`docs/fiche_sujet_FXX.md`](docs/fiche_sujet_FXX.md).

## Données

| | |
|---|---|
| Fichier | `data/raw/base_credit_allemagne_1.xlsx` (feuilles `base_lisible`, `base_origine`, `dictionnaire`) |
| Source | *Statlog (German Credit Data)*, UCI Machine Learning Repository (H. Hofmann) — version francisée fournie dans le cadre du devoir |
| Taille | 1 000 lignes, 20 variables explicatives (7 numériques, 13 catégorielles) + cible |
| Dates | aucune variable de date (coupe transversale) |

Les données brutes ne sont jamais modifiées : toute transformation est faite dans le notebook.

## Structure du dépôt

```
CodeApogee-Nom-Groupe/
├── README.md
├── requirements.txt
├── .gitignore
├── DECLARATION_IA.md
├── data/
│   ├── raw/                 données brutes (fichier Excel d'origine)
│   └── processed/           données préparées par le notebook (si produites)
├── docs/
│   ├── fiche_sujet_FXX.md   fiche du sujet
│   └── references.md        sources consultées, avec liens
├── notebooks/
│   └── projet_FXX.ipynb     notebook final, exécuté, avec ses sorties
├── src/                     scripts complémentaires (si utilisés)
├── figures/                 graphiques exportés pour le rapport et la présentation
├── rapport/
│   └── rapport.pdf          export PDF du notebook (compte rendu)
└── presentation/
    ├── presentation.pdf     les six diapositives
    └── presentation.pptx    fichier source
```

## Reproduire les résultats

**En local**

```bash
python -m venv .venv
source .venv/bin/activate          # Windows : .venv\Scripts\activate
pip install -r requirements.txt
jupyter notebook notebooks/projet_FXX.ipynb
```

Le notebook retrouve automatiquement `data/raw/base_credit_allemagne_1.xlsx` (chemin relatif depuis `notebooks/`). Puis **Exécuter toutes les cellules**.

**Sous Google Colab** : ouvrir le notebook, exécuter la cellule de chargement et téléverser `base_credit_allemagne_1.xlsx` quand le bouton apparaît.

Les résultats sont reproductibles : graine aléatoire unique (`GRAINE = 42`) et découpage stratifié.

**Export du compte rendu en PDF** (après exécution du notebook) :

```bash
jupyter nbconvert --to pdf notebooks/projet_FXX.ipynb --output rapport --output-dir rapport
```

## Contenu du notebook (compte rendu intégré)

1. Sujet et problématique · 2. Jeu de données · 3. Exploration (qualité, valeurs atypiques, trois graphiques interprétés) · 4. Préparation
5. Algorithme retenu (régression logistique) et huit algorithmes de comparaison : KNN, Naive Bayes, arbre de décision, forêt aléatoire, SVM, Gradient Boosting, AdaBoost, XGBoost
6. Protocole (validation croisée, variation d'hyperparamètres) · 7. Résultats (tableau, erreurs, seuil, coefficients)
8. Analyse critique · 9. Conclusion · 10. Références et reproduction · 11. Déclaration IA

## Résultats principaux

*À mettre à jour après l'exécution finale du notebook (avec XGBoost).*

Dans l'exécution de référence (`GRAINE = 42`, 8 algorithmes scikit-learn), au seuil de 0,5 :

| Algorithme | F1 (mauvais risque) | AUC-ROC |
|---|---|---|
| Régression logistique | 0,625 | 0,810 |
| Naive Bayes | 0,560 | 0,785 |
| Gradient Boosting | 0,547 | 0,786 |
| SVM | 0,545 | 0,810 |

L'objectif (F1 ≥ 0,55) est atteint de justesse par deux algorithmes ; les écarts entre les meilleurs ne sont pas significatifs (test de 200 clients, dont 60 mauvais risques). Voir la section 8 du notebook pour l'analyse critique et les limites.

## Limites

Petit échantillon et base ancienne (montants en Deutsche Mark), pas de dimension temporelle, hyperparamètres non optimisés, pas de matrice de coûts. Les variables sensibles (sexe/situation familiale, âge, travailleur étranger) ne devraient pas être utilisées dans un vrai scoring de crédit.

## Déclaration d'usage de l'IA

Voir [`DECLARATION_IA.md`](DECLARATION_IA.md).

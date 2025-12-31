# Projet d’Économétrie Appliquée — Analyse des Prix Immobiliers (Sorbonne Data Analytics)

Ce dépôt contient le **notebook Python** et le **rapport** du projet d’économétrie appliquée (Université Paris 1 Panthéon-Sorbonne / Sorbonne Data Analytics) portant sur l’analyse des déterminants des prix immobiliers, **du modèle linéaire aux méthodes de régularisation**.

## Auteurs
- **Edouard Lacroix**
- **Élise Prigent**

## Objectif du projet
À partir d’un échantillon de **150 transactions** immobilières (2015–2023), l’objectif est :
- d’identifier les facteurs structurels et contextuels du prix,
- de valider les hypothèses économétriques via des tests de diagnostic,
- de comparer MCO vs régularisation (Ridge/Lasso) en performance prédictive,
- et de produire une **prévision** (avec **intervalle de confiance à 95%**) pour un bien “représentatif”. 

## Données
Variables (principales) :
- `Surface_m2`, `Chambres`, `Annee_construction`, `Distance_centre_km`, `Etage`, `Ascenseur`
- `Annee_vente` (2015–2023)
- `Qualite_ecole` (1–10), `Revenu_median_quartier` (en k€)
- Variable dépendante : `Prix_milliers_euros` 

## Méthodologie (résumé)
Le travail suit une démarche progressive :
1. **Statistiques descriptives** + visualisations (histogrammes/boxplots)
2. **Corrélations** et discussion multicolinéarité
3. Estimations **MCO** :
   - modèle simple (prix ~ surface)
   - modèle multiple
   - comparaison des formes fonctionnelles (linéaire / semi-log / log-log)
4. **Diagnostics** : VIF, hétéroscédasticité (Breusch-Pagan + corrections), stabilité structurelle (Test de Chow)
5. **Endogénéité** : discussion + stratégie IV/2SLS (instrument : `Distance_universite`) + test DWH
6. **Régularisation** : Ridge et Lasso (standardisation + validation croisée)
7. **Prévision** : prédiction ponctuelle et **IC à 95%** (modèle semi-log MCO retenu pour produire l’intervalle).

## Pourquoi une prévision finale en MCO (et pas Ridge) ?
Même si Ridge peut améliorer la performance hors-échantillon, le livrable demande explicitement une **prévision + intervalle de confiance à 95%**. Or, les intervalles “classiques” des coefficients/predictions ne sont pas directement valides sous pénalisation (les estimateurs sont biaisés et l’inférence standard n’est pas celle des MCO).  
➡️ Pour respecter la consigne sur l’intervalle de confiance, la prévision finale est réalisée avec le **modèle semi-log enrichi estimé par MCO** (statsmodels `get_prediction()`), puis retranscrite en niveau via exponentielle.

## Structure du dépôt
- `Econometrie.ipynb` — Notebook principal (analyse + estimations + tests + prévision)
- `PROJET D’ÉCONOMÉTRIE APPLIQUÉE.pdf` — Rapport complet
- `donnees_immobilieres.xlsx` - Fichier de données

## Installation
### Prérequis
- Python 3.10+ recommandé
- Jupyter Notebook / JupyterLab

### Dépendances (typique)
```bash
pip install numpy pandas matplotlib seaborn statsmodels scikit-learn
```
## Exécution

Ouvrir le notebook :

- jupyter lab ou jupyter notebook
- Lancer Econometrie.ipynb et exécuter les cellules dans l’ordre.

## Résultats clés

- La spécification semi-log est retenue comme référence (interprétation en variations relatives + meilleure qualité d’ajustement).
- Les variables de quartier (Qualite_ecole, Revenu_median_quartier) améliorent significativement le modèle via test F emboîté.
- Diagnostics : pas d’hétéroscédasticité marquée (Breusch-Pagan non significatif dans le rapport), rupture structurelle associée à 2020 (Chow).
- En prédiction hors-échantillon, Ridge peut être performant, mais la prévision finale avec IC95% est produite en MCO semi-log enrichi. 

## Reproductibilité

Les résultats dépendent :

1 : du split train/test et de la seed (si utilisée),  
2 : des versions des packages,  
3 : de la disponibilité du fichier de données au bon chemin.

## Licence

Projet académique — usage pédagogique.

Université Paris 1 Panthéon-Sorbonne — Sorbonne Data Analytics

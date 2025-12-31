# Projet d’Économétrie Appliquée — Analyse des Prix Immobiliers (Sorbonne Data Analytics)

Ce dépôt contient le **notebook Python** et le **rapport** du projet d’économétrie appliquée (Université Paris 1 Panthéon-Sorbonne / Sorbonne Data Analytics) portant sur l’analyse des déterminants des prix immobiliers, **du modèle linéaire aux méthodes de régularisation**. :contentReference[oaicite:0]{index=0} :contentReference[oaicite:1]{index=1}

## Auteurs
- **Edouard Lacroix**
- **Élise Prigent** :contentReference[oaicite:2]{index=2}

## Objectif du projet
À partir d’un échantillon de **150 transactions** immobilières (2015–2023), l’objectif est :
- d’identifier les facteurs structurels et contextuels du prix,
- de valider les hypothèses économétriques via des tests de diagnostic,
- de comparer MCO vs régularisation (Ridge/Lasso) en performance prédictive,
- et de produire une **prévision** (avec **intervalle de confiance à 95%**) pour un bien “représentatif”. :contentReference[oaicite:3]{index=3} :contentReference[oaicite:4]{index=4}

## Données
Variables (principales) :
- `Surface_m2`, `Chambres`, `Annee_construction`, `Distance_centre_km`, `Etage`, `Ascenseur`
- `Annee_vente` (2015–2023)
- `Qualite_ecole` (1–10), `Revenu_median_quartier` (en k€)
- Variable dépendante : `Prix_milliers_euros` :contentReference[oaicite:5]{index=5}

> Le fichier de données (`donnees_immobilieres.xlsx`) n’est pas forcément versionné dans ce dépôt (selon consignes).  
> Si besoin, placez-le à la racine du projet ou adaptez le chemin dans le notebook.

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
7. **Prévision** : prédiction ponctuelle et **IC à 95%** (modèle semi-log MCO retenu pour produire l’intervalle). :contentReference[oaicite:6]{index=6} :contentReference[oaicite:7]{index=7}

## Pourquoi une prévision finale en MCO (et pas Ridge) ?
Même si Ridge peut améliorer la performance hors-échantillon, le livrable demande explicitement une **prévision + intervalle de confiance à 95%**. Or, les intervalles “classiques” des coefficients/predictions ne sont pas directement valides sous pénalisation (les estimateurs sont biaisés et l’inférence standard n’est pas celle des MCO).  
➡️ Pour respecter la consigne “à la lettre” sur l’intervalle de confiance, la prévision finale est réalisée avec le **modèle semi-log enrichi estimé par MCO** (statsmodels `get_prediction()`), puis retranscrite en niveau via exponentielle. :contentReference[oaicite:8]{index=8}

## Structure du dépôt
- `Econometrie.ipynb` — Notebook principal (analyse + estimations + tests + prévision)
- `PROJET D’ÉCONOMÉTRIE APPLIQUÉE.pdf` — Rapport complet (rendu final)
- `Statistics_project_2025_2026-1.pdf` — Énoncé / consignes du projet :contentReference[oaicite:9]{index=9} :contentReference[oaicite:10]{index=10}

## Installation
### Prérequis
- Python 3.10+ recommandé
- Jupyter Notebook / JupyterLab

### Dépendances (typique)
```bash
pip install numpy pandas matplotlib seaborn statsmodels scikit-learn
```
Exécution

Ouvrir le notebook :

jupyter lab
# ou
jupyter notebook

Lancer Econometrie.ipynb et exécuter les cellules dans l’ordre.

Résultats clés (très bref)

La spécification semi-log est retenue comme référence (interprétation en variations relatives + meilleure qualité d’ajustement).

Les variables de quartier (Qualite_ecole, Revenu_median_quartier) améliorent significativement le modèle via test F emboîté.

Diagnostics : pas d’hétéroscédasticité marquée (Breusch-Pagan non significatif dans le rapport), rupture structurelle associée à 2020 (Chow).

En prédiction hors-échantillon, Ridge peut être performant, mais la prévision finale avec IC95% est produite en MCO semi-log enrichi. 

PROJET D’ÉCONOMÉTRIE APPLIQUE…

Reproductibilité

Les résultats dépendent :

du split train/test et de la seed (si utilisée),

des versions des packages,

de la disponibilité du fichier de données au bon chemin.

Licence

Projet académique — usage pédagogique.

Université Paris 1 Panthéon-Sorbonne — Sorbonne Data Analytics

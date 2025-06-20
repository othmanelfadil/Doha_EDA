# Analyse Exploratoire et Modélisation Prédictive du Chiffre d'Affaires (CA)

Ce document présente une synthèse de l'analyse exploratoire des données (EDA) et de la modélisation prédictive réalisées sur un ensemble de données immobilières dans le but de prédire le chiffre d'affaires (CA).

## 1. Introduction

L'objectif de cette étude est de comprendre les facteurs qui influencent le chiffre d'affaires des ventes immobilières et de construire un modèle capable de le prédire. Le notebook **`Doha(EDA).ipynb`** contient l'ensemble du processus, du nettoyage des données à l'évaluation du modèle.

## 2. Description des Données

Les données initiales se composent de **11 744 observations** et **23 variables**, incluant des informations sur :

- La société et la business unit (BU)
- La localisation (Ville, Région)
- Les caractéristiques du bien (Programme, Nature, Superficie Totale)
- Les dates clés (Réservation, Concrétisation, TF)
- Des informations financières (CA, Marge, Reliquat)
- Des détails sur la vente (Vendu Par, Type de Contrat, Statut)

## 3. Nettoyage et Prétraitement des Données

Plusieurs étapes ont été nécessaires pour préparer les données à l'analyse et à la modélisation :

- **Suppression de colonnes inutiles** : `Unnamed: 22` et `Column1`, entièrement vides, ont été supprimées.
- **Conversion de types de données** :
  - Les colonnes `CA` et `MARGE` ont été converties en format numérique après suppression des virgules et traitement des valeurs non numériques.
  - Les colonnes de dates (`Date_TF`, `Date_Reservation`, `Date_Concretisation`, `Date_Verif`) ont été converties au format `datetime`.
- **Gestion des valeurs manquantes** :
  - `Reliquat` remplacé par `0`, `Vendu_Par` remplacé par `'UNKNOWN'`.
  - Imputation par la médiane ou propagation directe (`ffill`) pour certaines dates et la marge.
- **Traitement des zéros** :
  - **6 748** entrées de `Sup_Totale` étaient à zéro, pouvant indiquer des problèmes de saisie ou des biens non mesurés.
- **Transformation de la variable cible** :
  - Application de `log1p` à `CA` pour normaliser sa distribution fortement asymétrique.

## 4. Analyse Exploratoire des Données (EDA)

L'analyse a révélé plusieurs points importants :

- **Distribution du CA** : Étendue à droite, présence de valeurs extrêmes → transformation logarithmique justifiée.
- **Corrélation** :
  - Corrélation faible entre les variables numériques.
  - `Sup_Totale` et `MARGE` sont les plus corrélées avec `CA`.
- **Variables catégorielles** :
  - La majorité des biens vendus sont des **appartements**.
  - Les ventes sont concentrées dans la région **Casablanca-Settat**.

## 5. Modélisation Prédictive

### 5.1. Régression Linéaire (Modèle de référence)

Un modèle de régression linéaire simple a été mis en place comme base de comparaison.

- **Performance** :
  - MSE : `9.622 × 10¹⁵`
  - R² : `-0.0003`

> Un R² négatif indique que le modèle est moins performant que la moyenne → les relations dans les données ne sont pas linéaires.

### 5.2. XGBoost Regressor

Un modèle plus complexe, **XGBoost**, a été entraîné avec des hyperparamètres ajustés.

- **Performance** :
  - RMSE : `98,474,599.13`
  - R² : `-0.0081`

> Le score R² reste négatif malgré la complexité du modèle, suggérant une grande difficulté à prédire le CA avec les variables actuelles.

- **Importance des variables** :
  - Les plus significatives : `Sup_Totale`, `MARGE`, `Reliquat`.

## 6. Conclusions et Remarques

- **Qualité des données** :
  - Problèmes importants de qualité, notamment de nombreuses valeurs nulles ou à zéro pour des variables clés.
- **Complexité de la prédiction** :
  - Faibles scores R² → les variables disponibles n’expliquent qu’une faible partie de la variance du CA.
- **Non-linéarité** :
  - Confirmée par la mauvaise performance de la régression linéaire.
- **Modèle XGBoost** :
  - R² négatif inattendu pour ce type de modèle.
  - Hypothèses possibles :
    - Feature engineering insuffisant.
    - Variables explicatives absentes (ex : conditions du marché).
    - Risque de **fuite de données** (ex : `MARGE` calculée à partir de `CA` ?)

## 7. Recommandations

- **Amélioration de la collecte de données** :
  - Assurer la complétude et l'exactitude des données, notamment `Sup_Totale`.
- **Feature Engineering** :
  - Créer des variables pertinentes comme :
    - Durée entre réservation et concrétisation.
    - Encodage des variables `Programme`, `Ville`.
- **Explorer d'autres modèles** :
  - Tester des modèles comme **Random Forest**, etc.
- **Analyse approfondie** :
  - Étudier pourquoi `Sup_Totale` vaut zéro pour autant de biens.
  - Cela pourrait révéler des catégories spécifiques à modéliser séparément.

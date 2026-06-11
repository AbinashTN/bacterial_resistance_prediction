> **Note:** An English version follows the French description.

# Prédiction de la Résistance Bactérienne

## Objectif
L'objectif principal est de prédire la résistance aux antibiotiques d'échantillons bactériens (classification binaire) à partir de caractéristiques génomiques et de métadonnées.

## Contenu du Dépôt
*   `main.ipynb` : Le notebook Jupyter contenant l'intégralité du pipeline : exploration des données, prétraitement, entraînement des modèles et génération des prédictions.

## Méthodologie

### 1. Exploration et Prétraitement des Données
*   **Réduction de dimensionnalité** : Application de TruncatedSVD (100 à 200 composantes) sur les caractéristiques génomiques pour réduire la haute dimensionnalité des données.
*   **Normalisation** : StandardScaler appliqué aux composantes SVD.
*   **Ingénierie des métadonnées** :
    *   Normalisation du texte et imputation des valeurs manquantes (mode).
    *   Regroupement des catégories rares (< 20 échantillons) sous une étiquette commune.
    *   Encodage one-hot des variables catégorielles.
*   **Rééchantillonnage** : SMOTE pour corriger le déséquilibre des classes, avec variation du paramètre `k_neighbors` (3, 7, 20, 50).

### 2. Modèles Évalués
Plusieurs approches de classification ont été implémentées et comparées via une **validation croisée à 5 plis** :
*   ExtraTreesClassifier (critère gini, avec métadonnées)
*   ExtraTreesClassifier (critère entropy, avec métadonnées)
*   ExtraTreesClassifier (critère gini, sans métadonnées)
*   MLPClassifier (avec métadonnées)
*   MLPClassifier (sans métadonnées)
*   XGBClassifier (avec métadonnées)
*   XGBClassifier (sans métadonnées)
*   SVC à noyau polynomial (avec métadonnées)
*   SVC à noyau polynomial (sans métadonnées)

### 3. Modèle Final Sélectionné
La solution retenue est un **ensemble par vote majoritaire** combinant les 9 configurations de modèles. Cette approche tire parti de la diversité des architectures (arbres de décision, réseau de neurones, SVM, boosting) et des configurations de prétraitement variées afin de réduire la variance et d'améliorer la généralisation. La prédiction finale est obtenue en faisant la moyenne des probabilités prédites par chaque modèle, avec un seuil de décision à 0.5.

## Résultats
*   **Validation croisée** : ~90.6 % de précision, ~0.81 de F1 macro (moyenne sur 5 plis).
*   **Classement final** : **8e / 40** équipes, avec un **F1 macro de 0.728** sur le jeu de test privé.

---
*Réalisé par : Carl-Éric Lafleur, Antoine Lafrance, Abinash Tharmaseelan.*

---

# Bacterial Resistance Prediction

## Objective
The main objective is to predict antibiotic resistance in bacterial samples (binary classification) using genomic features and metadata.

## Repository Content
*   `main.ipynb`: The Jupyter notebook containing the entire pipeline: data exploration, preprocessing, model training, and prediction generation.

## Methodology

### 1. Data Exploration and Preprocessing
*   **Dimensionality Reduction**: TruncatedSVD (100–200 components) applied to genomic features to reduce their high dimensionality.
*   **Normalization**: StandardScaler applied to the SVD components.
*   **Metadata Engineering**:
    *   Text normalization and missing value imputation (mode).
    *   Rare category grouping (< 20 samples) under a common label.
    *   One-hot encoding of categorical variables.
*   **Resampling**: SMOTE oversampling to address class imbalance, with varying `k_neighbors` parameter (3, 7, 20, 50).

### 2. Evaluated Models
Several classification approaches were implemented and compared via **5-fold cross-validation**:
*   ExtraTreesClassifier (gini criterion, with metadata)
*   ExtraTreesClassifier (entropy criterion, with metadata)
*   ExtraTreesClassifier (gini criterion, without metadata)
*   MLPClassifier (with metadata)
*   MLPClassifier (without metadata)
*   XGBClassifier (with metadata)
*   XGBClassifier (without metadata)
*   SVC with polynomial kernel (with metadata)
*   SVC with polynomial kernel (without metadata)

### 3. Selected Final Model
The chosen solution is a **majority voting ensemble** combining all 9 model configurations. This approach leverages the diversity of architectures (tree-based, neural network, SVM, boosting) and varied preprocessing configurations to reduce variance and improve generalization. The final prediction is obtained by averaging the predicted probabilities across all models, with a decision threshold of 0.5.

## Results
*   **Cross-validation**: ~90.6% accuracy, ~0.81 macro F1 (averaged across 5 folds).
*   **Final leaderboard**: **8th / 40** teams, with a **macro F1 of 0.728** on the private test set.

---
*Realized by: Carl-Éric Lafleur, Antoine Lafrance, Abinash Tharmaseelan.*

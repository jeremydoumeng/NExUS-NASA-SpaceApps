# NExUS - Neural EXoplanet Understanding System

Un système web interactif de prédiction d'exoplanètes utilisant l'apprentissage automatique et la visualisation 3D.

![Exoplanet Explorer](https://img.shields.io/badge/Status-Active-brightgreen)
![Machine Learning](https://img.shields.io/badge/ML-AutoGluon-blue)
![WebGL](https://img.shields.io/badge/Visualization-WebGL-orange)
![F1 Score](https://img.shields.io/badge/F1%20Score-92.6%25-green)

## Table des matières

- [Vue d'ensemble](#-vue-densemble)
- [Fonctionnalités](#-fonctionnalités)
- [Installation](#-installation)
- [Utilisation](#-utilisation)
- [Architecture](#-architecture)
- [API](#-api)
- [Modèle ML](#-modèle-ml)
- [Structure du projet](#-structure-du-projet)
- [Contribution](#-contribution)

## Vue d'ensemble

NExUS est une application web sophistiquée qui combine l'apprentissage automatique et la visualisation 3D pour prédire et visualiser des exoplanètes. Le système utilise un modèle AutoGluon entraîné avec un score F1 de 92.6% pour classer les candidats exoplanètes en temps réel.

### Objectif

Démocratiser la recherche d'exoplanètes en permettant aux utilisateurs de :
- Entrer des paramètres d'observation astronomique
- Obtenir une prédiction instantanée via un modèle ML
- Visualiser le système planétaire en 3D
- Explorer une base de données d'exoplanètes confirmées

## Fonctionnalités

### Prédiction ML
- **Modèle AutoGluon** avec ensemble de 8 algorithmes (CatBoost, XGBoost, LightGBM, etc.)
- **Score F1 de 92.6%** sur la validation
- **Seuil de décision ajustable** (par défaut 0.436)
- **Validation en temps réel** des paramètres d'entrée

### Visualisation 3D
- **WebGL interactif** pour la visualisation des systèmes planétaires
- **Animation orbitale** en temps réel
- **Gradients de température** pour étoiles et planètes
- **Vue orbitale** avec contrôles caméra


### 🗄️ Base de données
- **12,656 exoplanètes** confirmées chargées depuis CSV
- **Changement automatique** toutes les 30 secondes
- **Informations détaillées** sur chaque système planétaire

## Installation

### Prérequis
- Python 3.8+
- Navigateur web moderne avec support WebGL
- Node.js (optionnel, pour le développement)

### 1. Cloner le projet
```bash
git clone <votre-repo>
cd spaceappshtml3
```

### 2. Installer les dépendances Python
```bash
pip install -r requirements.txt
```

### 3. Lancer l'API de prédiction
```bash
cd folder
python api.py
```

L'API sera accessible sur `http://localhost:5000`

### 4. Ouvrir l'application web
Ouvrez `index.html` dans votre navigateur ou servez les fichiers statiques :

```bash
# Option 1: Serveur Python simple
python -m http.server 8000

# Option 2: Serveur Node.js (si installé)
npx serve .

# Option 3: Ouvrir directement
open index.html  # macOS
```

## Utilisation

### Page principale (`index.html`)
- **Visualisation automatique** d'exoplanètes confirmées
- **Changement automatique** toutes les 30 secondes
- **Informations détaillées** sur chaque système
- **Bouton "Predict"** pour accéder aux prédictions

### Page de prédiction (`predict.html`)
1. **Remplir les paramètres** d'observation :
   - Mission spatiale (Kepler, K2, TESS)
   - Période orbitale (jours)
   - Profondeur du transit (ppm)
   - Paramètres stellaires et planétaires

2. **Utiliser les exemples** prédéfinis :
   - Jupiter-like
   - Petite planète terrestre
   - Faux positif

3. **Ajuster le seuil** de décision (0-1)

4. **Cliquer "Prédire"** pour obtenir le résultat

5. **Visualiser** le système 3D si une planète est détectée

## Architecture

```
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│   Frontend      │    │   Flask API     │    │   ML Model      │
│   (HTML/CSS/JS) │◄──►│   (Python)      │◄──►│   (AutoGluon)   │
└─────────────────┘    └─────────────────┘    └─────────────────┘
        │                       │                       │
        ▼                       ▼                       ▼
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│   WebGL 3D      │    │   Validation    │    │   Ensemble      │
│   Visualization │    │   & Processing  │    │   Predictors    │
└─────────────────┘    └─────────────────┘    └─────────────────┘
```

### Composants principaux

- **Frontend** : Interface utilisateur avec WebGL pour la 3D
- **API Flask** : Serveur de prédiction avec validation
- **Modèle AutoGluon** : Ensemble de 8 algorithmes ML
- **Base de données** : CSV avec exoplanètes confirmées

## API

### Endpoints disponibles

#### `POST /api/predict`
Prédiction d'exoplanète

**Paramètres :**
```json
{
  "mission": "kepler",
  "period_days": 3.52,
  "depth_ppm": 15000,
  "t_mag": 12.1,
  "threshold": 0.436
}
```

**Réponse :**
```json
{
  "success": true,
  "prediction": {
    "decision": "Planète/PC"
  },
  "input_data": { ... }
}
```

#### `GET /api/health`
Vérification de l'état de l'API

#### `GET /api/fields`
Liste des champs attendus

## 🤖 Modèle ML

### Architecture AutoGluon
- **8 algorithmes** en ensemble :
  - CatBoost
  - XGBoost
  - LightGBM
  - ExtraTrees
  - RandomForest
  - KNeighbors
  - NeuralNetTorch
  - LinearModel
- **WeightedEnsemble_L2** pour la combinaison finale

### Performance
- **F1 Score : 92.6%**
- **Seuil optimal : 0.436**
- **Données d'entraînement :** 12,656 exoplanètes confirmées

### Features utilisées
- Paramètres orbitaux (période, durée, profondeur)
- Caractéristiques stellaires (température, rayon, gravité)
- Paramètres planétaires (rayon, température d'équilibre)
- Coordonnées astronomiques (RA, DEC)

## 📁 Structure du projet

```
spaceappshtml3/
├── 📄 index.html              # Page principale avec visualisation
├── 📄 predict.html            # Page de prédiction
├── 📄 styles.css              # Styles pour index.html
├── 📄 styles2.css             # Styles pour predict.html
├── 📄 script.js               # Logique principale
├── 📄 webgl3d.js              # Moteur 3D WebGL
├── 📄 requirements.txt        # Dépendances Python
├── 📄 README.md               # Documentation
├── 📁 folder/                 # Backend ML
│   ├── 📄 api.py              # API Flask
│   ├── 📄 predict.py          # Module de prédiction
│   └── 📁 autogluon_exoplanets/  # Modèle entraîné
│       ├── 📄 predictor.pkl   # Prédicteur principal
│       ├── 📄 learner.pkl     # Apprenant
│       └── 📁 models/         # Modèles individuels
├── 📁 src/
│   └── 📄 exoplanets_unified.csv  # Base de données
└── 📄 galaxie.jpeg            # Image de fond
```

## Développement

### Ajouter de nouveaux exemples
Modifiez la section `examples` dans `predict.html` :

```javascript
const examples = {
    nouveau_exemple: {
        mission: "tess",
        period_days: 10.5,
        // ... autres paramètres
    }
};
```

### Modifier le seuil par défaut
Dans `folder/predict.py` :
```python
DEFAULT_THRESHOLD = 0.436  # Votre nouveau seuil
```

### Personnaliser les styles
- `styles.css` : Page principale
- `styles2.css` : Page de prédiction

## Déploiement

### Production
1. **Serveur web** (nginx, Apache) pour les fichiers statiques
2. **WSGI** (gunicorn) pour l'API Flask
3. **Base de données** PostgreSQL/MySQL pour les logs

### Docker (optionnel)
```dockerfile
FROM python:3.9-slim
COPY . /app
WORKDIR /app
RUN pip install -r requirements.txt
EXPOSE 5000
CMD ["python", "folder/api.py"]
```

## Contribution

1. **Fork** le projet
2. **Créer** une branche feature (`git checkout -b feature/nouvelle-fonctionnalite`)
3. **Commit** vos changements (`git commit -m 'Ajouter nouvelle fonctionnalité'`)
4. **Push** vers la branche (`git push origin feature/nouvelle-fonctionnalite`)
5. **Ouvrir** une Pull Request

## Licence

Ce projet est sous licence MIT. Voir le fichier `LICENSE` pour plus de détails.

## Remerciements

- **NASA** pour les données d'exoplanètes
- **AutoGluon** pour le framework ML
- **Three.js** pour l'inspiration WebGL
- **Communauté astronomique** pour les données ouvertes

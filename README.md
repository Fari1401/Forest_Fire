# GMQ710 – Estimation de la surface brûlée à partir d’imagerie satellitaire

## Objectifs
Ce projet a pour objectif d’estimer les surfaces brûlées causées par les incendies de forêt en utilisant des images satellitaires open source.  
L’analyse repose principalement sur les images **Landsat 8 et Landsat 9**, complétées par des données externes telles que **FIRMS** (points chauds) et **Wildfire Canada 2023** (zones brûlées).  
Le but est de développer une méthodologie fiable pour identifier, cartographier et quantifier les zones brûlées à l’aide d’indices spectraux (NBR), du seuillage et de comparaisons temporelles (avant/après).

---

## Données utilisées

| Source                                      | Type              | Format             | Utilité                                                                 |
|---------------------------------------------|-------------------|--------------------|-------------------------------------------------------------------------|
| **Landsat 8/9 (USGS)**                       | Raster (30 m)     | GeoTIFF            | Images avant/après pour le calcul du NBR et l’extraction des zones brûlées |
| **FIRMS (NASA – Fire Information for Resource Management System)** | Vecteur | FeatureCollection | Points chauds (confidence, date, intensité) pour valider les incendies |
| **Wildfire Canada 2023 (TIIC / PCA)**        | Raster (30 m)     | GeoTIFF / Image GEE| Référence externe pour valider les zones brûlées |
| **Zone d’étude – Québec (Données Québec)**   | Vecteur           | Shapefile / GeoJSON | Délimitation spatiale de l’analyse |

---

## Approche / Méthodologie envisagée

### 1. Définition de la zone d’étude
- Importation d’une zone du Québec affectée par les feux de 2023  
- Utilisation du shapefile comme masque pour restreindre l’analyse

### 2. Importation et prétraitement des images Landsat
- Sélection d’images cloud-free avant et après l’incendie  
- Calibration radiométrique et inspection des métadonnées  
- Application d’un masque de nuages (QA_PIXEL)

### 3. Calcul du NBR et du dNBR
- **NBR_before** et **NBR_after** à partir des bandes NIR et SWIR2  
- **dNBR = NBR_before – NBR_after**  
- Extraction des zones brûlées par analyse temporelle

---

## Approche 1 : Analyse dNBR (Avant/Après)

- Calcul du dNBR  
- Classification des valeurs (faible brûlure, modérée, sévère)  
- Extraction des pixels touchés  
- **Estimation de surface : Surface = Nb_pixels × 900 m²**

---

## Approche 2 : Seuillage sur l’image post-incendie

- Utilisation uniquement de l’image après feu  
- Méthodes de détection :
  - Photo-interprétation + histogramme  
  
- Production d’un raster binaire : **Feu / Non Feu**  
- Estimation de surface par comptage des pixels classifiés Feu

---

## Validation et comparaison

### Données FIRMS
- Validation de la présence de points chauds  
- Analyse de la confiance (0–100 %)  
- Vérification temporelle des détections

### Données Wildfire Canada 2023
- Comparaison avec les zones brûlées officielles  
- Vérification de la cohérence spatiale

---

## Outils et langages prévus

- **Langage(s)**  
  - Python

- **Bibliothèques / logiciels**  
  - Rasterio  
  - NumPy  
  - Pandas  
  - Folium / Geemap  
  - GEE API
    

---

## Répartition des tâches dans l’équipe

- **Bouzid Mohammed Amine**  
  Prétraitement des données

- **Farsi Ibrahim Aymen**  
  Estimation de surface brûlée – Approche dNBR (avant/après)

- **Benchiha Akrem**  
  Estimation de surface brûlée – Approche par seuillage

---

## Questions non résolues

- Quels seuils dNBR adopter pour une classification optimale ?  


# Python_DS_Project

## Sujet : Variations météorologiques et consommation d'énergie électrique

Ce projet a été réalisé dans le cadre d'un cours enseigné en deuxième année du cycle ingénieur à l'Ecole Nationale de la Statistique et de l'Administration Economique (ENSAE), à savoir __Python pour la data Science__.

## Participants

L'équipe de travail est constituée de trois membres :  
    - M. Richard AKOTONOU VIGNON  
    - Mme Ange Yelva PETNGA NJEUMEN  
    - M. Appolini WODJE  

## Objectif

L'objectif du travail réalisé est d'effectuer la prévision de la consommation d'énergie électrique en fonctions des grandeurs météorologiques.

## Bases utilisées

Les bases de données utilisées sont : 
- **Base de consommation d'électricité** obtenue sur l'OpenData Réseaux Energies, qui présente la consommation quotidienne d'électricité par région en France métropolitaine hors Corse
- **Base sur les données météorologiques** : les données d'observations issues des messages internationaux d’observation en surface (SYNOP) circulant sur le système mondial de télécommunication (SMT) de l’Organisation Météorologique Mondiale (OMM). Paramètres atmosphériques mesurés (température, humidité, direction et force du vent, pression atmosphérique, hauteur de précipitations) ou observés (temps sensible, description des nuages, visibilité) depuis la surface terrestre.
- **Base du statut des journées en France** (jours fériés, ouvrés, Week end)
- **Données géographiques de la France** : obtenue à partir de Cartifllete, qui est un projet qui faculite l'association des sources géographiques.


## Couverture géographique

Le travail a été réalisé à partir des données de la France métropolitaire hors Corse. Toutefois, la prévision de la consommation d'énergie électrique n'a été implémentée que pour la région Ile-de-France.

## Démarche adoptée

Afin de vous donner une idée du contenu du travail réalisée, nous présentons ci-dessous le plan détaillé:  

INTRODUCTION  
I. IMPORTATION DES BASES DE DONNEES  
      I.1 Importation des bases de données via API de l'OpenDataSoft  
      I.2 Importation de la base de données complémentaires des communes  
      I.3 Importation de la Geodataframe des départements et des bases de données préalablement requetées  
II. TRAITEMENT DES BASES DE DONNEES  
    II.1 Suppression des éventuelles données répétées  
    II.2 Choix de l'année d'étude  
    II.3 Traitement de la base de données de consommation d'électricité  
    II.4 Traitement de la base de données des variables météorolgiques  
        II.4.1 Calcul des superficies par département  
        II.4.2 Choix des noms des variables  
        II.4.3 Identification des variables météorologiques à retenir  
        II.4.4 Vérification et gestion des valeurs manquantes pour les variables d'agrégation (département et région)  
        II.4.5 Agrégation des variables météorologiques  
            II.4.5.1 Agrégation des variables à l'échelle départementale  
            II.4.5.2 Agrégation des variables à l'échelle régionale  
            II.4.5.3 Agrégation des autres variables (températures minimale et maximale)  
    II.5 Création de la variable indiquant le statut d'une journée  
    II.6 Gestion des valeurs manquantes  
        II.6.1 Gestion des valeurs extrêmes  
        II.6.2 Préparation de la base de données pour l'imputation  
        II.6.2 Choix des méthodes d'imputation pour chaque variable  
    II.7 Constitution de la base de données comprenant toutes les variables  
III. ANALYSE EXPLORATOIRE DES GRANDEURS CONSIDEREES   
    III.1 Analyse de l'évolution des grandeurs au cours de l'année 2019   
        III.1.1 Evolution de la consommation totale d'électricité au cours de l'année 2019  
        III.1.2 Evolution de la température moyenne au cours de l'année 2019  
        III.1.3 Evolution de la température minimale au cours de l'année 2019  
        III.1.4 Evolution de la température maximale au cours de l'année 2019  
        III.1.5 Evolution de la vitesse moyenne du vent au cours de l'année 2019  
        III.1.6 Evolution de la visibilité horizontale moyenne au cours de l'année 2019  
    III.2 Analyse des potentielles liaisons entre la consommation d'électricité et autres grandeurs  
        III.2.1 Consommation d'électricité et température moyenne  
        III.2.2 Consommation d'électricité et température minimale  
        III.2.3 Consommation d'électricité et température maximale  
        III.2.4 Consommation d'électricité et vitesse du vent  
        III.2.5 Consommation d'électricité et visibilité horizontale  
        III.2.6 Consommation d'électricité et statut du jour  
    III.3 Gestion des valeurs extrêmes pour la consommation d'électricité, les températures maximale et minimale  
    III.4 Matrices de corrélation par région  
IV. ANALYSE CARTOGRAPHIQUE  
V. MODELISATION DE LA CONSOMATION D'ELECTRICITE EN ILE-DE-FRANCE  
    V.1 Première regression naïve sur les variables temporelles et la température  
    V.2 Standardisation  
    V.3 Desaisonalisation  
    V.4 Recherche des meilleurs hyperparamètres pour le Random Forest (avec GridSeachCV)  
    V.5 Modèle retenu  
CONCLUSION  



## Packages à installer 
```python
! pip install git+https://github.com/inseefrlab/cartiflette@80b8a5a28371feb6df31d55bcc2617948a5f9b1a

```
Parmi les packages suivants, certains pourraient être à téléchargés si vous n'êtes pas sur le Datalab

## Packages utilisés
```python
import pandas as pd
import numpy as np
import requests
import cartiflette.s3 as s3
import geopandas as gpd
import seaborn as sns
import matplotlib.pyplot as plt

from shapely.geometry import shape
from io import StringIO
from scipy.stats import iqr
from datetime import datetime
from sklearn.impute import SimpleImputer, KNNImputer
from sklearn.linear_model import LinearRegression, Lasso
from sklearn.model_selection import GridSearchCV
from sklearn.neighbors import KNeighborsRegressor
from sklearn.ensemble import RandomForestRegressor
from sklearn.preprocessing import StandardScaler, OneHotEncoder
from sklearn.metrics import mean_squared_error, make_scorer
from statsmodels.tsa.seasonal import seasonal_decompose
from statsmodels.tsa.holtwinters import ExponentialSmoothing
from cartiflette.s3 import download_vectorfile_url_all
```



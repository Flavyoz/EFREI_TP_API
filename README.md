# EcoTrack – TEXIER Flavio - README

## Présentation du projet
EcoTrack est une application qui récupère des données environnementales depuis deux API externes, les transforme, puis les stocke dans une base SQLite.  
Une API FastAPI permet ensuite de consulter ces données, et un petit front-end affiche les résultats.

---

## Sources de données

### 1. Open-Meteo – Air Quality API
Utilisée pour obtenir des données horaires sur la qualité de l’air.  
Indicateurs récupérés :
- PM10  
- PM2.5  
- Monoxyde de carbone

Ces données sont ingérées pour deux zones : **Paris** et **Albuquerque**.
L'intérêt est donc de comparer la qualité de l'air dans ces deuc villes.

### 2. OpenAQ – Latest API
Utilisée comme deuxième source de mesures.  
Cependant, je n’ai pas réussi à obtenir des résultats réellement différents entre Paris et Albuquerque.  
Les deux zones se retrouvent donc avec un volume comparable d’indicateurs.
J'ai rencontré des difficultés avec cette API, notamment la documentation, qui n'était pas toujours claire. J'ai eu du mal à comprendre comment récupérer des données pour une ville en particulier. J'ai réussi à récupérer des données pour un pays, pour une localisation, mais pas pour une ville précisément.

---

## Fonctionnement du script d’ingestion

Le fichier `ingestion.py` gère trois grandes étapes :

### 1. Appels API
Le script interroge :
- Open-Meteo pour les données horaires,
- OpenAQ pour les mesures récentes.

### 2. Transformation
- Parsing des dates ISO,
- Mapping des variables (ex : PM10, PM2_5),
- Préparation des objets `Indicator`.

### 3. Stockage
Les données sont enregistrées dans les tables :
- `zones`
- `sources`
- `indicators`

L’orchestrateur `ingest_all()` lance l’ensemble du processus.

### Initialisation
Le script `init_db.py` :
- crée les tables SQLite,
- lance l’ingestion complète,
- remplit la base.

La première étape est donc de lancer le script `init_db.py`.
---

## API & Front-end

- Une API FastAPI expose des routes pour accéder aux zones, sources et indicateurs.
- Un petit front-end statique, lancé via `python -m http.server`, permet d’afficher les données.

  Il faut donc lancer l'API avec la commande ```uvicorn app.main:app --reload```.
  Puis, dans un autre terminal, se placer dans le répertoire `\frontend` avec la commande ```cd frontend```. On lance ensuite la commande ```py -m http.server 8001```



# TP4 — API Iris et Base de Données Redis

## 1. Présentation du projet

Ce projet consiste à développer et déployer une API REST de classification des fleurs Iris avec **FastAPI**, puis à lui ajouter une mémoire grâce à **Redis**.

L'API utilise des modèles de Machine Learning pour prédire l'espèce d'une fleur Iris à partir de ses caractéristiques. Redis permet de stocker un compteur du nombre de prédictions effectuées.

L'application est conteneurisée avec **Docker** et orchestrée avec **Docker Compose**.

## 2. Objectifs du TP

- Reprendre une API de classification Iris existante.
- Intégrer Redis pour compter les prédictions.
- Configurer les services avec Docker Compose.
- Tester les endpoints de l'API.
- Vérifier la persistance des données après redémarrage.
- Observer l'effet de la suppression des volumes Docker.

## 3. Technologies utilisées

| Technologie | Utilisation |
|---|---|
| Python | Développement de l'application |
| FastAPI | Création de l'API REST |
| Scikit-learn | Modèle Random Forest |
| XGBoost | Modèle de classification |
| Redis | Stockage du compteur |
| Docker | Conteneurisation |
| Docker Compose | Orchestration des services |

## 4. Architecture de l'application

```text
Client HTTP
    |
    v
API FastAPI (Port 8000)
    |
    +----> Classification Iris
    |       Random Forest / XGBoost
    |
    +----> Redis (Port 6379)
               |
               v
       Compteur de prédictions
               |
               v
         Volume Docker
          redis-data
```

## 5. Installation et exécution

### Prérequis

- Docker Desktop
- Docker Compose
- Git

### Cloner le projet

```bash
git clone https://github.com/yassmine-soufary/L-API-iris-et-une-base-de-donn-es.git
cd L-API-iris-et-une-base-de-donn-es
```

### Vérifier la configuration

```bash
docker compose config
```

### Construire et démarrer les services

```bash
docker compose up --build -d
```

### Vérifier les conteneurs

```bash
docker compose ps
```

## 6. Tester l'API

L'API est accessible à l'adresse :

http://localhost:8000

Documentation interactive FastAPI :

http://localhost:8000/docs

### Exemple de prédiction

Endpoint prévu :

```http
POST /predict/random_forest
```

Exemple de données :

```json
{
  "sepal_length": 5.1,
  "sepal_width": 3.5,
  "petal_length": 1.4,
  "petal_width": 0.2
}
```

### Consultation des statistiques

Endpoint prévu :

```http
GET /stats
```

Redis conserve le nombre total de prédictions réalisées.

**Remarque :** les endpoints doivent être vérifiés dans `/docs` ou `/openapi.json` si l'API retourne une erreur `404 Not Found`.

## 7. Test de persistance Redis

### Premier test : arrêt sans suppression des volumes

```bash
docker compose down
docker compose up -d
```

**Résultat attendu :** le compteur conserve sa valeur grâce au volume Docker Redis.

### Deuxième test : suppression des volumes

```bash
docker compose down -v
docker compose up --build -d
```

**Résultat attendu :** le compteur est réinitialisé à zéro, car les données du volume Redis sont supprimées.

## 8. Résultats et observations

Les tests permettent d'étudier :

- La communication entre FastAPI et Redis.
- Le fonctionnement du compteur de prédictions.
- La gestion des services avec Docker Compose.
- La différence entre `docker compose down` et `docker compose down -v`.

Les résultats effectifs doivent être confirmés à partir des réponses de l'API et des captures d'écran réalisées.

## 9. Conclusion

Ce TP illustre l'intégration d'une base de données Redis dans une API de Machine Learning.

Docker Compose facilite le déploiement des différents services, tandis que les volumes Docker permettent de conserver les données indépendamment du cycle de vie des conteneurs.

Le projet met ainsi en pratique les notions d'API REST, de conteneurisation, de communication entre services et de persistance des données.

---

**Réalisé par :** Yassmine Soufary

**Projet :** TP4 — L'API Iris et une base de données
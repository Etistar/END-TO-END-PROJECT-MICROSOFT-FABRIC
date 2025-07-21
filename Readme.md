# Analyse Sismique via Microsoft Fabric

<p align="center"><strong>Réalisé par : ZONON Etienne


**Profil de l'auteur** : Etienne ZONON est un passionné de la data, spécialisé en BI, data engineering et cloud computing. Il conçoit des solutions modernes centrées sur Microsoft Fabric, Azure et Power BI, pour la valorisation des données à fort impact métier.
</strong></p>

---

## Agenda

1. [Prérequis](#prérequis)
2. [Objectif du projet](#objectif-du-projet)
3. [Architecture du projet](#architecture-du-projet)
4. [Pipeline d'automatisation](#pipeline-dautomatisation)
5. [Visualisation Power BI](#visualisation-power-bi)
6. [Avantages du projet](#avantages-du-projet)
7. [Prochaines étapes](#prochaines-étapes)

---

## Prérequis

* Connaissances en Python, SQL, PySpark et Power BI
* Compréhension des architectures Lakehouse
* Accès à un espace de travail Microsoft Fabric
* Droits administrateur sur Fabric

---

## Objectif du projet

Ce projet a pour but de collecter, transformer, enrichir et visualiser des données sismiques en provenance de l'API publique de l'USGS. L'architecture de type Data Lake (Bronze / Silver / Gold) permet une analyse dynamique dans Power BI, avec un pipeline de rafraîchissement automatisé. Il vise à surveiller l'activité sismique à l'échelle mondiale et à identifier les zones à risque.

Inspiré de : [Analyse sismique avec Fabric (vidéo)](https://www.youtube.com/watch?v=Av44Nrhl05s&pp=0gcJCdgAo7VqN5tD)



---

## Architecture du projet

### 1. Bronze – Ingestion brute

* **Notebook :** `Bronze`
* **Source :** API USGS Earthquake API
* **Données :** JSON bruts enregistrés dans `Files/{start_date}_earthquake_data.json`
* **Objectif :** Collecte sans transformation
* **Lien API :** [USGS Earthquake API](https://earthquake.usgs.gov/fdsnws/event/1/)
* **Exemple d'appel :**

  ```
  https://earthquake.usgs.gov/fdsnws/event/1/query?format=geojson&starttime=2014-01-01&endtime=2014-01-02
  ```

### 2. Silver – Structuration

* **Notebook :** `Silver`
* **Objectif :** Nettoyage et structuration
* **Actions :**

  * Lecture JSON multi-lignes
  * Extraction de colonnes utiles (id, magnitude, localisation, description, etc.)

### 3. Gold – Données prêtes pour analyse

* **Notebook :** `Gold`
* **Objectif :** Transformation avancée pour analyse
* **Actions :**

  * Enrichissement, calculs, filtrage
  * Création d'un dataset Gold prêt pour Power BI
  * Ajout de traçabilité (Data Lineage)
  * Géocodage inversé via `reverse_geocoder`
  * Classification de l'intensité :

    * `Low` : magnitude < 100
    * `Moderate` : 100 ≤ magnitude ≤ 500
    * `High` : magnitude > 500

---

## Pipeline d'automatisation

* **Objectif :** Rafraîchissement régulier des données via le notebook Bronze
* **Fonctionnalités :**

  * Dates paramétrables dynamiquement
  * Orchestration via Microsoft Fabric

  ![Mon schema](Pipeline.PNG)

---

## Visualisation Power BI

* **Rapport :** Power BI connecté au Gold Layer dans le Lakehouse `earth_lakehouse`
* **Visualisations :**

  * Carte sismique
  * Magnitude par région
  * Évolution temporelle des séismes

![Mon schema](PowerBI.PNG)
---

## Avantages du projet

* Architecture modulaire et maintenable (Bronze → Silver → Gold)
* Séparation claire des étapes d’ingestion, traitement et valorisation
* Intégration native avec Microsoft Fabric et Power BI
* Automatisation possible de bout en bout

![Mon schema](DataLineage.PNG)

---

## Prochaines étapes

* Mise en place d’un pipeline CI/CD
* Intégration d’une génération de réponses via RAG (Retrieval-Augmented Generation)
* Amélioration de la qualité des métadonnées (pays, libellés)

---

Projet réalisé avec Microsoft Fabric pour une valorisation moderne des données sismiques.


# Cahier des charges — Pipeline Big Data d'analyse de sentiment Twitter

| Champ | Valeur |
|---|---|
| **Auteur** | Aymane |
| **Filière** | Data and Software Sciences (DSS) — 2A |
| **École** | ENSIAS — Université Mohammed V de Rabat |
| **Module** | Fondamentaux Big Data |
| **Enseignante** | Pr. Widad ELOUATAOUI |
| **Année universitaire** | 2025–2026 |
| **Date de rédaction** | Mai 2026 |
| **Durée du projet** | 2 semaines |
| **Repository** | https://github.com/Aymane7236/bigdata-twitter-sentiment-pipeline |

---

## 1. Contexte et motivation

Avec l'explosion des réseaux sociaux, Twitter (désormais X) génère **chaque jour plusieurs centaines de millions de tweets** exprimant des opinions, sentiments et réactions à l'actualité. Ces données représentent une mine d'information stratégique pour les entreprises, mais leur **volume**, leur **vélocité** et leur **variété** dépassent largement les capacités des systèmes traditionnels.

Les outils du Big Data (Hadoop, Spark, Kafka, Hive) permettent de relever ce défi en offrant un traitement distribué, scalable et tolérant aux pannes. Ce projet vise à mettre en œuvre, de bout en bout, **l'ensemble des étapes de la Big Data Value Chain** sur un cas d'usage réel : l'analyse de sentiment de tweets à grande échelle.

---

## 2. Problématique métier

> **Comment construire un pipeline Big Data capable d'ingérer, traiter et analyser en quasi-temps réel des millions de tweets pour fournir une analyse de sentiment automatisée et identifier les tendances, afin d'aider les entreprises à piloter leur e-réputation et leurs décisions marketing ?**

### 2.1 Public cible

- Équipes marketing (suivi de marque et campagnes)
- Équipes support client (détection d'insatisfaction)
- Analystes e-réputation
- Décideurs en communication de crise

### 2.2 Cas d'usage

- Détecter en quasi-temps réel les pics de sentiments négatifs
- Identifier les sujets émergents (trending topics)
- Prédire le sentiment de nouveaux tweets pour automatiser la modération
- Cartographier le comportement des utilisateurs

---

## 3. Objectifs

### 3.1 Objectif principal

Concevoir et implémenter un pipeline Big Data complet couvrant les **6 étapes de la Big Data Value Chain** appliqué à un dataset de 1,6 million de tweets.

### 3.2 Objectifs spécifiques

1. Ingérer un flux de tweets via Apache Kafka (simulation streaming)
2. Nettoyer et enrichir les données textuelles avec PySpark (NLP)
3. Stocker dans un système distribué interrogeable (HDFS + Apache Hive)
4. Analyser les patterns temporels et thématiques avec Spark
5. Prédire le sentiment de nouveaux tweets via Spark MLlib (> 75% accuracy)
6. Visualiser les KPIs dans un dashboard interactif

---

## 4. Périmètre du projet

### 4.1 Inclus

- ✅ Pipeline complet (ingestion → visualisation)
- ✅ Infrastructure conteneurisée (Docker Compose)
- ✅ Modèle ML supervisé de classification de sentiment
- ✅ Dashboard interactif avec au moins 4 visualisations
- ✅ Documentation technique et rapport académique
- ✅ Orchestration via Apache Airflow (bonus)

### 4.2 Exclu (hors scope)

- ❌ Déploiement en production sur cloud (AWS/GCP/Azure)
- ❌ Streaming temps réel via l'API Twitter (simulé via Kafka)
- ❌ Modèles deep learning (BERT, transformers) — restera sur des modèles classiques pour rester dans le cadre Spark MLlib
- ❌ Multi-langue (focus sur l'anglais, langue du dataset)

---

## 5. Dataset

| Caractéristique | Valeur |
|---|---|
| **Nom** | Sentiment140 |
| **Source** | [Kaggle / Stanford University](https://www.kaggle.com/datasets/kazanova/sentiment140) |
| **Volume** | 1 600 000 tweets |
| **Taille** | ~238 MB (CSV brut) |
| **Période** | Avril – Juin 2009 |
| **Langue** | Anglais |
| **Format** | CSV |
| **Colonnes** | `target`, `ids`, `date`, `flag`, `user`, `text` |
| **Types de données** | Numérique, date, texte, catégoriel |
| **Labels** | 0 (négatif), 4 (positif) — labellisation automatique via emoticons |

### 5.1 Respect des critères du sujet

| Exigence | Statut |
|---|---|
| ≥ 500 MB OU ≥ 1M lignes | ✅ 1,6M lignes |
| ≥ 5 colonnes | ✅ 6 colonnes |
| ≥ 2 types de données | ✅ 4 types |
| Données réelles | ✅ Tweets authentiques |

---

## 6. Architecture technique

### 6.1 Stack technologique

| Phase | Outil | Justification |
|---|---|---|
| **Data Ingestion** | Apache Kafka | Standard du streaming, simule un flux temps réel |
| **Data Storage (Bronze)** | HDFS | Stockage distribué tolérant aux pannes |
| **Data Preprocessing** | Apache Spark (PySpark) | Traitement distribué scalable |
| **Data Storage (Silver/Gold)** | Apache Hive + Parquet | Tables partitionnées interrogeables en SQL |
| **Data Processing** | Apache Spark | Agrégations et calculs distribués |
| **Machine Learning** | Spark MLlib | Pipeline ML scalable et intégré |
| **Data Visualization** | Apache Superset / Streamlit | Dashboard interactif moderne |
| **Orchestration (bonus)** | Apache Airflow | DAG automatisé reproductible |
| **Infrastructure** | Docker Compose | Reproductibilité et portabilité |

### 6.2 Architecture medallion

Le pipeline suit l'architecture **medallion** (bronze / silver / gold) :

- **Bronze** : données brutes ingérées (Parquet sur HDFS)
- **Silver** : données nettoyées et enrichies (Parquet partitionné)
- **Gold** : agrégations métier prêtes pour la visualisation (Tables Hive)

### 6.3 Schéma global

```
Source CSV → Kafka Producer → Kafka Topic → Spark Streaming
                                                  ↓
                                          HDFS (Bronze)
                                                  ↓
                                       Spark Preprocessing
                                                  ↓
                                          HDFS (Silver)
                                                  ↓
                            ┌────────────────────┼────────────────────┐
                            ↓                    ↓                    ↓
                     Hive Tables         Spark Aggregations    Spark MLlib
                                                  ↓                    ↓
                                          HDFS (Gold)         Model Storage
                                                  ↓
                                       Superset / Streamlit
```

---

## 7. KPIs et questions analytiques

| # | Question | Type d'analyse | Livrable |
|---|---|---|---|
| Q1 | Quelle est la répartition globale des sentiments ? | Descriptive | Pie chart |
| Q2 | Comment évolue le sentiment dans le temps ? | Temporelle | Line chart |
| Q3 | Quels sont les mots/hashtags les plus fréquents par sentiment ? | Textuelle | Word cloud / Bar chart |
| Q4 | Qui sont les utilisateurs les plus actifs et leur biais sentimental ? | Comportementale | Table + Bar chart |
| Q5 | Peut-on prédire le sentiment d'un nouveau tweet (> 75% accuracy) ? | Prédictive | Modèle ML + matrice de confusion |

### 7.1 Hypothèses à vérifier

- **H1** : Le sentiment varie significativement selon l'heure de la journée
- **H2** : Certains utilisateurs ont un biais sentimental marqué
- **H3** : La longueur du tweet est corrélée à son sentiment
- **H4** : Un modèle TF-IDF + classification linéaire atteint > 75% d'accuracy

---

## 8. Planning prévisionnel (2 semaines)

### Semaine 1 — Infrastructure et données

| Jour | Tâches | Livrables |
|---|---|---|
| **J1** | Cadrage, problématique, setup repo | Cahier des charges, repo GitHub |
| **J2** | Setup Docker stack (Kafka, HDFS, Hive, Spark) | docker-compose.yml fonctionnel |
| **J3** | Data Ingestion (Kafka → HDFS) | Données brutes dans HDFS |
| **J4** | Data Preprocessing (PySpark NLP) | Données nettoyées (Silver) |
| **J5** | Storage Hive (partitions, schémas) | Tables Hive opérationnelles |
| **J6-7** | Buffer / Documentation intermédiaire | — |

### Semaine 2 — Analyse et restitution

| Jour | Tâches | Livrables |
|---|---|---|
| **J8** | Data Processing (agrégations Spark) | Tables Gold avec KPIs |
| **J9-10** | Machine Learning (Spark MLlib) | Modèle entraîné + métriques |
| **J11** | Data Visualization (Superset/Streamlit) | Dashboard interactif |
| **J12** | Orchestration Airflow (bonus) | DAG fonctionnel |
| **J13** | Rapport LaTeX | Rapport ~15-20 pages |
| **J14** | Présentation + finalisation | PPT + démo |

---

## 9. Livrables finaux

1. **Code source** complet sur GitHub (public)
2. **docker-compose.yml** reproductible (lancement en une commande)
3. **Rapport LaTeX** (~15-20 pages) incluant :
   - Introduction et contexte
   - Architecture détaillée
   - Choix techniques justifiés
   - Captures d'écran du dashboard
   - Résultats du modèle ML
   - Conclusion et perspectives
4. **Présentation PowerPoint** (12-15 slides)
5. **Dashboard fonctionnel** (Superset ou Streamlit)
6. **Modèle ML** sauvegardé avec ses métriques
7. **README.md** clair avec instructions de lancement
8. **Démo vidéo** (optionnel, bonus)

---

## 10. Critères de succès

### 10.1 Critères techniques

- [ ] Le pipeline s'exécute de bout en bout sans erreur
- [ ] Toutes les 6 étapes de la Big Data Value Chain sont couvertes
- [ ] Au moins un outil distribué (Spark) est utilisé pour chaque étape pertinente
- [ ] Le code est versionné, documenté et reproductible
- [ ] Le modèle ML atteint au moins 75% d'accuracy

### 10.2 Critères académiques

- [ ] Cahier des charges clair et structuré
- [ ] Justification technique pour chaque choix d'outil
- [ ] Rapport complet et bien rédigé
- [ ] Présentation orale convaincante

---

## 11. Risques identifiés et mitigation

| Risque | Probabilité | Impact | Mitigation |
|---|---|---|---|
| Problèmes Docker / RAM insuffisante | Moyenne | Élevé | Limiter les services lancés en parallèle, ajuster les ressources |
| Performance Spark sur petite machine | Moyenne | Moyen | Travailler sur un échantillon en dev, full dataset en final |
| Temps de preprocessing trop long | Faible | Moyen | Paralléliser, utiliser Parquet, partitionner |
| Difficulté avec Hive | Moyenne | Moyen | Documentation officielle + cours suivi |
| Retard sur le planning | Moyenne | Élevé | Buffers J6-J7 prévus |

---

## 12. Environnement de développement

- **OS** : Windows 11 (machine principale)
- **Containerisation** : Docker Desktop + Docker Compose
- **IDE** : Visual Studio Code
- **Langage principal** : Python 3.11
- **Versioning** : Git + GitHub
- **Documentation** : Markdown + LaTeX

---

## 13. Références

1. **Cours** : *Fondamentaux Big Data*, Pr. Widad ELOUATAOUI, ENSIAS, 2025–2026
2. **Dataset** : Sentiment140 — Stanford / Kaggle — https://www.kaggle.com/datasets/kazanova/sentiment140
3. **Apache Hadoop** : https://hadoop.apache.org/
4. **Apache Spark** : https://spark.apache.org/
5. **Apache Kafka** : https://kafka.apache.org/
6. **Apache Hive** : https://hive.apache.org/

---

**Document évolutif** — Sera mis à jour au fur et à mesure de l'avancement du projet.
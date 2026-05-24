# 🐦 Big Data Twitter Sentiment Analysis Pipeline

End-to-end Big Data pipeline for real-time Twitter sentiment analysis, built with the Hadoop ecosystem and modern data engineering practices.

## 📋 Project Overview

This project implements a complete **Big Data Value Chain** to analyze 1.6M+ tweets, predict sentiment, and visualize trends through an interactive dashboard.

- **Author:** Aymane (ENSIAS - DSS 2nd year)
- **Course:** Fondamentaux Big Data
- **Instructor:** Pr. Widad ELOUATAOUI
- **Academic Year:** 2025-2026

## 🎯 Business Problem

How to build a Big Data pipeline capable of ingesting, processing and analyzing millions of tweets in near real-time to provide automated sentiment analysis and trend detection, in order to help companies monitor their e-reputation and marketing decisions?

## 🛠️ Tech Stack

| Phase | Tool |
|---|---|
| Ingestion | Apache Kafka |
| Storage (Raw) | HDFS |
| Storage (Analytical) | Apache Hive |
| Processing | Apache Spark (PySpark) |
| Machine Learning | Spark MLlib |
| Visualization | Apache Superset / Streamlit |
| Orchestration (bonus) | Apache Airflow |
| Infrastructure | Docker Compose |

## 📊 Dataset

- **Source:** [Sentiment140 (Kaggle)](https://www.kaggle.com/datasets/kazanova/sentiment140)
- **Size:** 1.6M tweets (~238 MB)
- **Features:** target, ids, date, flag, user, text

## 🏗️ Architecture

> Architecture diagram coming soon — see `docs/architecture.png`

## 📁 Project Structure

```
bigdata-twitter-sentiment-pipeline/
├── docker/              # Docker Compose configuration
├── ingestion/           # Phase 1 - Kafka producer & Spark consumer
├── preprocessing/       # Phase 2 - Data cleaning with PySpark
├── storage/             # Phase 3 - Hive schemas
├── processing/          # Phase 4 - Spark aggregations
├── ml/                  # Phase 5 - Spark MLlib pipeline
├── visualization/       # Phase 6 - Superset / Streamlit
├── airflow/             # Bonus - Pipeline orchestration
├── notebooks/           # EDA & experiments
├── docs/                # Reports & presentations
└── scripts/             # Utility scripts
```

## 🚀 Getting Started

> Setup instructions coming in the next phase.

## 📜 License

MIT — see [LICENSE](LICENSE) file.End-to-end Big Data pipeline for Twitter sentiment analysis using Hadoop ecosystem (Kafka, HDFS, Spark, Hive) + ML and visualization

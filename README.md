# 🔥 Yelp Sentiment Analysis — End-to-End Big Data Pipeline

Distributed sentiment analysis pipeline processing 6.96M+ Yelp reviews 
using Hadoop, Spark and MongoDB in a fully containerized environment

---

## 📌 Project Overview

This project builds a complete Big Data pipeline to classify Yelp reviews 
as positive, neutral or negative, using distributed storage (HDFS), 
distributed computing (PySpark) and NoSQL storage (MongoDB).

**Dataset :** Yelp Academic Dataset  
**Volume :** 3.45 GB · 6.96M reviews  
**Training sample :** 250,000 reviews  
**Infrastructure :** 7 Docker containers

---

## 🏆 Results

| Model | Accuracy | F1-Score |
|-------|----------|----------|
| Naïve Bayes | 74.9% | 0.77 |
| Random Forest | 70.3% | 0.58 |
| **Logistic Regression** | **83.2%** | **0.82** |

✅ Best model : **Logistic Regression** (83.2% accuracy)

---

## 🏗️ Infrastructure — Docker Containers

| Container | Role |
|-----------|------|
| Namenode | HDFS master — manages block locations |
| Datanode 1 | HDFS storage node |
| Datanode 2 | HDFS storage node |
| Datanode 3 | HDFS storage node |
| Spark Master | Distributes computing tasks |
| Spark Worker | Executes parallel computations |
| Jupyter Lab | Interactive development (port 8888) |
| MongoDB 6.0 | Stores predictions and metrics |

---

## 📦 HDFS Storage

| Parameter | Value |
|-----------|-------|
| File size | 3.45 GB |
| Block size | 128 MB |
| Total blocks | 26 blocks |
| Replication factor | 3 DataNodes |
| Health status | HEALTHY — 0 corrupt, 0 missing blocks |

```bash
# HDFS directory structure
/data/yelp/raw        # Raw JSON dataset
/data/yelp/processed  # Cleaned data
/data/yelp/features   # TF-IDF feature vectors
/models               # Saved ML models
```

---

## ⚙️ Pipeline Steps

### 1. Ingestion — HDFS
```bash
hdfs dfs -mkdir -p /data/yelp/raw
hdfs dfs -put /data/yelp/raw/yelp_academic_dataset_review.json /data/yelp/raw/
```

### 2. Spark Processing — PySpark
```python
spark = SparkSession.builder.appName("YelpSentiment").getOrCreate()
df = spark.read.json("hdfs://namenode:9000/data/yelp/raw/*.json")
```

### 3. NLP Pipeline — TF-IDF
- Tokenizer : splits each review into individual words
- StopWordsRemover : removes common words (the, a, is...)
- HashingTF : converts words into frequency vectors
- IDF : weights rare words more than frequent ones

### 4. ML Training — Spark MLlib
- 3 models trained on 80% data (199,815 reviews)
- Tested on 20% (50,185 reviews)
- Parallel computation on Spark Master + Worker

### 5. Storage — MongoDB
- 5,000 predictions stored in `reviews_predictions`
- 3 model metrics stored in `model_metrics`
- Database : `yelp_sentiment`

---

## 🛠️ Tech Stack

| Category | Technology |
|----------|-----------|
| Distributed Storage | Apache Hadoop HDFS 3.x |
| Distributed Computing | Apache Spark · PySpark |
| Machine Learning | Spark MLlib |
| NoSQL Database | MongoDB 6.0 |
| Containerization | Docker · Docker Compose |
| Development | Jupyter Lab · Python 3.x |
| Libraries | pandas · scikit-learn · numpy · pymongo · matplotlib |

---


[README.md](https://github.com/Prateek07-Kumar/GCP-End-to-End-Healthcare-Data-Engineer-Project/edit/main/README.md)

# Real-Time Healthcare Data Pipeline (GCP | Apache Beam | Medallion Architecture)

## Project Overview
This project implements a **real-time healthcare data processing pipeline** using **Google Cloud Platform (GCP)** and **Apache Beam (Dataflow)**. It simulates patient vitals streaming data, processes it through a **Medallion Architecture (Bronze → Silver → Gold)**, and delivers **aggregated insights for monitoring patient health risks**.

The system is designed to handle **streaming healthcare data**, ensuring **data quality, validation, enrichment, and analytics-ready outputs**.

---

## Objectives
- Build a **real-time streaming pipeline** for healthcare vitals data
- Apply **data validation and cleansing**
- Perform **risk scoring and classification**
- Implement **Medallion Architecture (Bronze, Silver, Gold layers)**
- Store processed data for **analytics and reporting (BigQuery)**
  
---

## Architecture Overview

```
Patient Simulator → Pub/Sub → Dataflow (Apache Beam)
        ↓
   Bronze Layer (Raw Data - GCS)
        ↓
   Silver Layer (Cleaned + Enriched - GCS)
        ↓
   Gold Layer (Aggregated - BigQuery)
```

---

## Tech Stack
- **Cloud Platform:** Google Cloud Platform (GCP)
- **Streaming:** Pub/Sub
- **Processing Engine:** Apache Beam (Dataflow)
- **Storage:**
  - Bronze & Silver → Google Cloud Storage (GCS)
  - Gold → BigQuery
- **Programming Language:** Python
- **Environment Management:** dotenv

---

## Data Pipeline Flow

### Data Generation (Simulator)
A custom simulator generates real-time patient vitals and streams them into Pub/Sub.

#### Features:
- Simulates multiple patients
- Generates realistic vitals:
  - Heart Rate
  - SpO2
  - Temperature
  - Blood Pressure
- Injects **intentional data errors**:
  - Missing fields
  - Invalid values
  - Out-of-range values

👉 This ensures the pipeline can handle **dirty real-world data**.

---

### 2️⃣ Streaming Pipeline (Apache Beam)
The pipeline runs in **streaming mode** using Apache Beam and processes data in real-time.

---

## 🥉 Bronze Layer (Raw Data)

### Purpose:
Store raw incoming data **without transformation**.

### Process:
- Read streaming messages from Pub/Sub
- Decode messages into strings
- Apply **1-minute windowing**
- Store raw JSON data in GCS

### Output:
- Raw, unprocessed data
- Useful for auditing and reprocessing

---

## 🥈 Silver Layer (Cleaned & Enriched Data)

### Purpose:
Improve data quality and add meaningful insights.

### Steps:

#### ✅ Data Parsing
- Convert JSON strings into structured records

#### ✅ Data Validation
Records are filtered based on:
- Required fields presence
- Valid ranges:
  - SpO2: 0–100
  - Heart Rate: 0–200
  - Temperature: 30–45°C

Invalid records are **discarded**

#### ✅ Data Enrichment (Risk Scoring)

Risk Score =
    (Heart Rate / 200) * 0.4 +
    (Temperature / 40) * 0.3 +
    (1 - SpO2 / 100) * 0.3

**Risk Levels:**
- Low (< 0.3)
- Moderate (0.3 – 0.6)
- High (> 0.6)

### Output:
- Cleaned and enriched data stored in GCS

---

## 🥇 Gold Layer (Aggregated Analytics)

### Purpose:
Provide **analytics-ready data for dashboards and monitoring systems**

### Transformations:
- Group data by `patient_id`
- Compute:
  - Average Heart Rate
  - Average SpO2
  - Average Temperature
- Determine **maximum risk level per patient**

### Output:
- Stored in **BigQuery**
- Ready for:
  - BI tools (Power BI, Looker)
  - Real-time monitoring dashboards

---

## 📊 Key Features

### 🔹 Real-Time Processing
- Continuous streaming using Pub/Sub + Dataflow

### 🔹 Data Quality Handling
- Filters invalid healthcare records
- Handles missing and corrupted data

### 🔹 Risk-Based Monitoring
- Assigns dynamic risk levels per patient
- Enables early detection of critical conditions

### 🔹 Scalable Architecture
- Serverless processing with Dataflow
- Handles increasing patient data seamlessly

### 🔹 Medallion Architecture
- Clear separation of:
  - Raw data (Bronze)
  - Clean data (Silver)
  - Aggregated insights (Gold)

---

## 📁 Project Structure

```
├── streaming_medallion_pipeline.py
├── patient_vitals_simulator.py
├── .env
├── README.md
```

---

## 🔐 Environment Configuration

The pipeline uses environment variables for flexibility:

- `GCP_PROJECT`
- `PUBSUB_TOPIC / SUBSCRIPTION`
- `BRONZE_PATH`
- `SILVER_PATH`
- `BIGQUERY_TABLE`
- `TEMP_LOCATION`
- `STAGING_LOCATION`
- `REGION`
  
---

## 🚀 Use Cases

- 🏥 Hospital patient monitoring systems  
- 🚑 Emergency alert systems  
- 📈 Healthcare analytics platforms  
- 🧠 Predictive health risk modeling  
- 🏠 Remote patient monitoring (IoT devices)

---

## 📌 Key Learnings

- Designing **end-to-end streaming pipelines**
- Implementing **data validation at scale**
- Applying **Medallion Architecture in real-time systems**
- Building **fault-tolerant pipelines**
- Integrating multiple GCP services effectively

---

## 🔮 Future Enhancements

- Add **real-time alerting system (Cloud Functions / Pub/Sub triggers)**
- Integrate **ML models for predictive risk scoring**
- Build **dashboard using Looker / Power BI**
- Store historical trends for **time-series analysis**
- Add **patient anomaly detection**

---

## 🏁 Conclusion

This project demonstrates how to build a **scalable, real-time healthcare analytics pipeline** using modern data engineering practices. It ensures **data reliability, real-time insights, and actionable intelligence**, making it highly relevant for **health-tech and IoT-based healthcare systems**.

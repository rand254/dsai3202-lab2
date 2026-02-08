# Amazon Electronics Data Ingestion Pipeline (Azure)

## Course
DSAI3202 – Data Ingestion and Preprocessing in the Cloud  
University of Doha for Science and Technology

---

## Objective
The goal of this lab is to build an end-to-end **automated data ingestion pipeline** using **Azure cloud services**.  
We ingest a large Amazon Electronics reviews dataset, store it in a data lake, clean and transform it, and write the processed data in **Parquet format**, partitioned by review year.

---

## Architecture Overview
- **Azure Blob Storage / ADLS Gen2** – Raw and processed data storage  
- **Azure Machine Learning** – Compute and terminal access  
- **Azure Data Factory (ADF)** – Data ingestion, transformation, and automation  

---

## Dataset
- **Dataset**: Amazon Electronics Reviews
- **Format**: JSON (line-delimited after fixing)
- **Size**: > 1 GB
- **Files used**:
  - `reviews_Electronics_5.json`
  - `meta_Electronics_fixed.json`

---

## Storage Structure
```text
raw/
 ├── reviews_Electronics_5.json
 ├── meta_Electronics.json
 ├── meta_Electronics_fixed.json

processed/
 └── reviews/
     ├── review_year=1999/
     ├── review_year=2000/
     ├── review_year=2001/
     └── ...

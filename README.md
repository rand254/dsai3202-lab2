

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

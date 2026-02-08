

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

Step 1: Upload Data to Azure Blob Storage

Created an Azure Storage Account (ADLS Gen2 enabled)

Created a container named raw

Uploaded:

reviews_Electronics_5.json

meta_Electronics.json

Verified files using Azure Portal

Step 2: Fix Metadata File

The metadata file was not valid JSON.

Actions:

Downloaded meta_Electronics.json.gz

Decompressed the file

Converted it into valid line-delimited JSON using Python

Uploaded the corrected file as:

meta_Electronics_fixed.json

Step 3: Create Azure Data Factory

Created an Azure Data Factory (V2) instance

Launched ADF Studio

Step 4: Create Linked Service

Created a Linked Service connecting ADF to the Storage Account

Authentication method: Account key

Tested and confirmed successful connection

Step 5: Create Datasets
Source Dataset (Raw JSON)

Name: ds_reviews_raw_json

Type: ADLS Gen2

Format: JSON

Path:

Container: raw

File: reviews_Electronics_5.json

Sink Dataset (Processed Parquet)

Name: ds_reviews_processed_parquet

Type: ADLS Gen2

Format: Parquet

Path:

Container: processed

Directory: reviews/

Schema: None (schema drift allowed)

Step 6: Create Mapping Data Flow

Name: df_reviews_json_to_parquet_partitioned

Transformations:

Source

Reads raw JSON reviews

Schema drift enabled

Derived Column

Transformation name: deriveReviewYear

New column:

review_year = year(toTimestamp(unixReviewTime))


Sink

Output format: Parquet

Partitioned by: review_year

Output path: processed/reviews/

Step 7: Create and Run Pipeline

Pipeline name: pl_reviews_ingestion_parquet_partitioned

Added Data Flow activity

Selected data flow: df_reviews_json_to_parquet_partitioned

Ran Debug

Pipeline completed successfully

Step 8: Verification

Verified processed data in Azure Portal

Confirmed folders created by year:

review_year=1999
review_year=2000
review_year=2001
...


Data stored in Parquet format

Step 9: Add Trigger (Automation)

Created a Schedule Trigger

Recurrence: Daily

Trigger attached to pipeline

Published all changes

Note: The dataset is static. The trigger demonstrates pipeline automation, not live data arrival.

Conclusion

This lab demonstrates how to:

Ingest large-scale JSON data into Azure

Clean and validate raw datasets

Transform data using Azure Data Factory

Store analytics-ready Parquet files

Automate ingestion using scheduled triggers

The pipeline is scalable, automated, and suitable for real-world data lake architectures.



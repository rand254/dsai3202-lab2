
## Dataset
- Amazon Reviews – Electronics (5-core)
- Files:
  - reviews_Electronics_5.json
  - meta_Electronics.json
  - meta_Electronics_fixed.json
- Source format: JSON
- Output format: Parquet (partitioned by review_year)

---

## Azure Resources
- Storage Account: amazondatalake60304966
- Containers:
  - raw
  - processed
  - curated
- Azure Data Factory: amazon-adf-60304966

---

## Step 1: Upload Raw Data
Uploaded the following files to the raw container:
- reviews_Electronics_5.json
- meta_Electronics.json

---

## Step 2: Fix Metadata File
The original metadata file was not valid JSON. It was converted to valid line-delimited JSON.

Python script used:
```bash
python3 << 'EOF'
import ast, json

input_file = "meta_Electronics.json"
output_file = "meta_Electronics_fixed.json"

with open(input_file, "r") as fin, open(output_file, "w") as fout:
    for line in fin:
        obj = ast.literal_eval(line)
        fout.write(json.dumps(obj) + "\n")

print("Metadata conversion complete.")
EOF
The corrected file was uploaded back to the raw container as:

meta_Electronics_fixed.json

Step 3: Azure Data Factory Linked Service
Linked Service Name: ls_amazon_storage_60304966

Data Store: Azure Data Lake Storage Gen2

Authentication Method: Account key

Connection test: Successful

Step 4: Datasets
Source Dataset
Name: ds_reviews_raw_json

Type: JSON

Container: raw

File: reviews_Electronics_5.json

Sink Dataset
Name: ds_reviews_processed_parquet

Type: Parquet

Container: processed

Directory: reviews/

Schema import: None

Step 5: Mapping Data Flow
Data Flow Name: df_reviews_json_to_parquet_partitioned

Source Configuration
Dataset: ds_reviews_raw_json

Allow schema drift: Enabled

Infer drifted column types: Enabled

Derived Column
Transformation Name: deriveReviewYear

Column: review_year

Expression:

year(toTimestamp(unixReviewTime))
Sink Configuration
Dataset: ds_reviews_processed_parquet

Partition by: review_year

Output format: Parquet

Step 6: Pipeline
Pipeline Name: pl_reviews_ingestion_parquet_partitioned

Activity: Data Flow

Data Flow Used: df_reviews_json_to_parquet_partitioned

Pipeline was run using Debug and completed successfully.

Verification
Verified the processed output in the processed/reviews directory.
Parquet files were generated and partitioned by year:

review_year=1999

review_year=2000

review_year=2001

...

review_year=2014


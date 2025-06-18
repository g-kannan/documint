#### Common Alias: 
Landing Zone, Ingestion Layer, Staging Layer

#### Goal: 
The Bronze layer is where we land all the data from external source systems. The table structures in this layer correspond to the source system table structures "as-is," along with any additional metadata columns that capture the load date/time, process ID, etc. The focus in this layer is quick Change Data Capture and the ability to provide an historical archive of source (cold storage), data lineage, auditability, reprocessing if needed without rereading the data from the source system.

#### Mandatory metadata fields to add:

1. Load Timestamp with timezone
2. Type of update(insert,update,delete)
3. File Name(if applicable)

#### Optional metadata fields to add:
1. Data Pipeline Name


#### Mandatory Data Quality Checks:
1. Duplicates check within batch
2. Replace/Remove nulls within batch
3. Schema Validated(Columns & Data Types)

#### Code Snippet
=== "PySpark"

    ``` python
    from pyspark.sql.functions import current_timestamp
    df.withColumns({"load_timestamp_utc": current_timestamp()})
    ```

#### Bronze Layer Maintenance
1. Move processed files to archive bucket or ensure life cylce policy(move to different tier) is implemented
2. Ensure partitioning in place in the bronze layer
3. Always aim to get data in parquet format for efficient processing


#### Checklist for Bronze Layer
```
1. Implemented the mandatory metadata fields(Yes/No/NA):
2. Implemented the optional metadata fields(Yes/No/NA):
3. Duplicates removed (Yes/No/NA):
4. Nulls removed/replaced (Yes/No/NA):
5. Schema validated (Yes/No/NA):
```
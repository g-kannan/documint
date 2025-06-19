#### Common Alias: 
Landing Zone, Ingestion Layer, Staging Layer

#### Goal: 
The Bronze layer is where we land all the data from external source systems. The table structures in this layer correspond to the source system table structures "as-is," along with any additional metadata columns that capture the load date/time, process ID, etc. The focus in this layer is quick Change Data Capture and the ability to provide an historical archive of source (cold storage), data lineage, auditability, reprocessing if needed without rereading the data from the source system.

#### Mandatory metadata fields to add:

1. Load Timestamp with timezone
2. Type of update(insert,update,delete for CDC)
3. File Name(if applicable)

##### Examples
=== "PySpark"

    ``` python
    from pyspark.sql.functions import current_timestamp,input_file_name
    df.withColumns({"load_timestamp_utc": current_timestamp(), "input_filename": input_file_name()})
    ```

#### Optional metadata fields to add:
1. Data Pipeline Name


#### Mandatory Data Quality Checks:
1. Duplicate Handling
2. Null Handling
3. Essential Columns Check
4. Schema Drift Check(New Columns/Renamed Columns)
4. Column Data Type Validation



#### Bronze Layer Maintenance
1. Move processed files to archive bucket or ensure life cylce policy(move to different tier) is implemented
2. Ensure partitioning in place in the bronze layer
3. Always aim to get data in parquet format for efficient processing


#### Checklist for Bronze Layer Workloads
```
| CheckID | Description |  Implemented(Y/N/NA) |
| --- | --- | --- |
| B1 | Mandatory metadata fields | |
| B2 | Optional metadata fields | |
| B3 | Duplicates Handled | |
| B4 | Nulls Handled | |
| B5 | Essential Columns Check | |
| B6 | Schema Drift Check | |
| B7 | Column Data Type Validation | |
```

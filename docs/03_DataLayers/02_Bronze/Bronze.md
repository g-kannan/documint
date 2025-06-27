#### Common Alias: 
Landing Zone, Ingestion Layer, Staging Layer

#### Goal: 
The Bronze layer is where we land all the data from external source systems. The table structures in this layer correspond to the source system table structures "as-is," along with any additional metadata columns that capture the load date/time, process ID, etc. The focus in this layer is quick Change Data Capture and the ability to provide an historical archive of source (cold storage), data lineage, auditability, reprocessing if needed without rereading the data from the source system.

#### Mandatory metadata fields/columns to add:

1. Load Timestamp with timezone
2. Type of update(insert,update,delete for CDC)
3. File Name(if applicable)

!!! example
    === "PySpark"

        ``` python
        from pyspark.sql.functions import current_timestamp,input_file_name
        df.withColumns({"load_timestamp_utc": current_timestamp(), "input_filename": input_file_name()})
        ```

#### Optional metadata fields to add:
1. Data Pipeline Name


#### Mandatory Data Quality Checks:
1. Handling Duplicates
2. Handling Nulls
3. Essential Columns Check
4. Schema Drift Check(New/Missing Columns)
5. Column Data Type Validation
6. Empty Dataframe Check

!!! example
    === "SchemaDrift(>Spark3.5)"

        ``` python
        from pyspark.testing import assertSchemaEqual
        assertSchemaEqual(source_df.schema, target_df.schema)
        ```

    === "SchemaDrift(<Spark3.5)"

        ``` python
        from loguru import logger

        def schema_drift_check(src_df, tgt_df):
            source_column_list = set(src_df.columns)
            target_column_list = set(tgt_df.columns)
            only_in_source = list(source_column_list - target_column_list)
            only_in_target = list(target_column_list - source_column_list) 
            if len(only_in_source) > 0:
                error_msg = f"Schema drift detected: Additional columns found in Source: {only_in_source}"
                logger.exception(error_msg)
                raise Exception(error_msg)
            if len(only_in_target) > 0:
                error_msg = f"Schema drift detected: Columns missing in Source: {only_in_target}"
                logger.exception(error_msg)
                raise Exception(error_msg)
            else:
                logger.info("Schemas matched for given dataframes")


        schema_drift_check(source_df, target_df)
        ```

    === "EmptyDataframe"
        ``` python
        source_df.isEmpty()
        ```

#### Bronze Layer Maintenance
1. Move processed files to archive bucket or ensure life cylce policy(move to different tier) is implemented
2. Ensure partitioning in place in the bronze layer
3. Always aim to get data in parquet format for efficient processing

#### Mandatory Code Headers

``` markdown
# Metadata
----
| ID | Property | Value |
| --- | --- | --- |
| M1 | Has PII/Regulatory/Sensitive Data | |
| M2 | Timezone for Timestamp Columns | |

# Checklist
----
| ID | Description |  Implemented(Y/N/NA) |
| --- | --- | --- |
| B1 | Mandatory metadata fields | |
| B2 | Optional metadata fields | |
| B3 | Duplicates Handled | |
| B4 | Nulls Handled | |
| B5 | Essential Columns Check | |
| B6 | Schema Drift Check | |
| B7 | Column Data Type Validation | |
| B8 | Empty Dataframe Check | |
| B9 | Exception & Error logging | |

# Change Log
----
| Date | Change | Author |
| --- | --- | --- |
|  |  |  |
```

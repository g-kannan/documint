# Databricks

## Best Practices

### Data Ingestion
#### File\Event based sources:
1. Use [Autoloader](https://docs.databricks.com/aws/en/ingestion/cloud-object-storage/auto-loader/) for easier checkpointing
2. Use [CleanSource](https://docs.databricks.com/aws/en/ingestion/cloud-object-storage/auto-loader/options#common-auto-loader-options) option to maintain the landing/input directory. Available from 16.4 LTS

### Orchestration
#### Parallelism
1. Avoid threadpool executor and use [For-Each](https://docs.databricks.com/aws/en/jobs/for-each) with Concurrency based on requirement. This gives higher visibility of operations and spreads load across workers.

### Security
### Observability
#### Freshness
1. Setup [SQL Alerts](https://learn.microsoft.com/en-in/azure/databricks/sql/user/alerts/) with notifications
2. Setup [Dashboards](https://learn.microsoft.com/en-us/azure/databricks/dashboards/) for specific requirements


## FinOps

## Performance Guide

[Databricks Performance Guide](https://www.databricks.com/discover/pages/optimize-data-workloads-guide)

## Common Operations

### Performing Data Tests/Fixes
```mermaid
graph LR
    ShallowClone --> TestChanges
    TestChanges --> VerifyImpactedRows
    VerifyImpactedRows --> Implement
```

### Unzipping a Zip file within Databricks
``` py
from zipfile import ZipFile
from io import BytesIO

def unzip_file_within_databricks(source_file_path, target_dir_path):
    """
    Unzips a zip file within Databricks and stores the unzipped files in the target directory
    
    Parameters
    ----------
    source_file_path : str
        The path to the zip file to be unzipped - Eg: "abfss://source-path@sadataengci.dfs.core.windows.net/file.zip"
    target_dir_path : str
        The path to the target directory where the unzipped files should be stored Eg: "abfss://target-path@sadataengci.dfs.core.windows.net/target-folder/"

    Returns
    -------
    None
    """

  zip_df = spark.read.format("binaryFile").load(source_file_path)

  zip_content = zip_df.select("content").collect()[0][0]

  with ZipFile(BytesIO(zip_content), 'r') as zipping:
      for fname in zipping.namelist():
          if fname.endswith(".csv"):
            f_content = zipping.read(fname)
            target_filepath = f"{target_dir_path}{fname}"
            print(fname)
            dbutils.fs.put(target_filepath, f_content.decode("utf-8"), overwrite=True)

  print("Unzipped and saved")
```


# Databricks

## Performance Guide

[Databricks Performance Guide](https://www.databricks.com/discover/pages/optimize-data-workloads-guide)

## Observability

### Freshness
1. Setup [SQL Alerts](https://learn.microsoft.com/en-in/azure/databricks/sql/user/alerts/) with notifications
2. Setup [Dashboards](https://learn.microsoft.com/en-us/azure/databricks/dashboards/) for specific requirements

## Daily Tasks

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

source_file_path = "abfss://source-path@sadataengci.dfs.core.windows.net/file.zip"
target_dir_path = "abfss://target-path@sadataengci.dfs.core.windows.net/target-folder/"

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


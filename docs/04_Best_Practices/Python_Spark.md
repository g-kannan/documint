## Python
Additional to [pep8](https://peps.python.org/pep-0008/), here are some additional best practices specific to Data Engineering


Import only the modules you need
Tag versions for modules and packages in requirements.txt
Include modes like "Debug" for verbose logging to console
Use logging instead of print statements

## Spark
1. Only change default configuration on purpose
2. Create pipelines which are idempotent
3. Run Merge only if Source Dataframe is "NOT Empty"
4. Define Schemas explicitly and not infer
5. Use ZSTD over SNAPPY for tables not frequently updated
6. Do NOT use Spark for small workloads(<5GB)


## Architecture
* This website is built using [mkdocs](https://www.mkdocs.org/) and [mkdocs-material](https://squidfunk.github.io/mkdocs-material/).
* Hosted via Cloudflare Pages

## Folder Structure

    mkdocs.yml    # The configuration file.
    docs/
        index.md  # The documentation homepage.
    01_Overview/
        project_layout.md
    02_Tools_Frameworks/
        Python.md
    03_DataFlow/
        01_Source/
            Source.md
        02_Bronze/
            Bronze.md
        03_Silver/
            Silver.md
        04_Gold/
            Gold.md
    04_Best_Practices/
        Cloud_Service_Providers.md
        Python_Spark.md
    05_Data_Platforms/
        BigQuery.md
        Databricks.md
        Snowflake.md
    06_Benchmarks/
    07_Roadmap/
    Contributing.md

## Running Locally

* `mkdocs serve` - Start the live-reloading docs server locally.
...
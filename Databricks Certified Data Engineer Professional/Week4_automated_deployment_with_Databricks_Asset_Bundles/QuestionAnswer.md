Please note: While the provided sources contain excellent material on CI/CD, Databricks Asset Bundles (DABs), testing, and nine official sample questions, they do not cover all the technical specifics for the other exam sections (such as Data Sharing, Cost Optimization, Security, etc.) mentioned in the exam guide. To fulfill your request for a full 59-question mock exam that covers all objectives, I have generated additional questions using external knowledge to bridge those gaps. You may want to independently verify that external information.

Here is your 59-question Databricks Certified Data Engineer Professional mock exam. **Please submit your responses (e.g., 1A, 2B, etc.), and I will provide the answers and your score!**

### Section 1: Developing Code for Data Processing using Python and SQL
**1. The security team is exploring whether the Databricks secrets module can be leveraged for connecting to an external database. After testing the code with all Python variables being defined with strings, they upload the password to the secrets module and configure the correct permissions for the currently active user. They modify their code to the following:**
`password = dbutils.secrets.get(scope="db_creds", key="jdbc_password")`
`print(password)`
**What will happen when this code is executed?**

A) The connection to the external table will succeed; the string "REDACTED" will be printed.
B) The connection to the external table will succeed; the string value of the password will be printed in plain text.
C) An interactive input box will appear in the notebook; if the right password is provided, the connection will succeed, and the password will be printed in plain text.
D) An interactive input box will appear in the notebook; if the right password is provided, the connection will succeed, and the encoded password will be saved to DBFS.

**2. The business reporting team requires that data for their dashboards be updated every hour. The total processing time for the pipeline, which extracts, transforms, and loads data for its runs, is 10 minutes. Assuming normal operating conditions, which configuration will meet their service-level agreement requirements with the lowest cost?**
A) Schedule a job to execute the pipeline once an hour on a dedicated interactive cluster.
B) Schedule a job to execute the pipeline once an hour on a new job cluster.
C) Schedule a Structured Streaming job with a trigger interval of 60 minutes.
D) Configure a job that executes every time new data lands in a given directory.

**3. When testing your Databricks pipeline code, what testing framework is recommended for writing fast, low-cost unit tests that automatically discover test cases starting with the `test_` prefix?**
A) Selenium
B) NUnit
C) Pytest
D) Unittest

**4. What is the default format for new Databricks notebooks created as of December 20, 2024, which you must correctly specify in your `databricks.yml` file?**
A) `.py`
B) `.dbc`
C) `.ipynb`
D) `.sql`

**5. How do Lakeflow Declarative Pipeline expectations assist with integration testing in a CI/CD workflow?**
A) They automatically execute `pytest` scripts in the background.
B) They provide data quality constraints to automatically validate data as it flows through the ETL pipeline.
C) They generate synthetic data for the development catalog.
D) They evaluate the interaction between notebooks and job scheduling APIs.

**6. (External) Which PySpark DataFrame testing function is best used to verify that two DataFrames have the exact same schema and data during a unit test?**
A) `DataFrame.summary()`
B) `assertDataFrameEqual()`
C) `assertSchemaEqual()`
D) `DataFrame.intersect()`

**7. (External) You are building an ETL pipeline using Lakeflow Spark Declarative Pipelines. What API should you use to efficiently apply Change Data Capture (CDC) operations to a target table?**
A) `MERGE INTO`
B) `APPLY CHANGES INTO`
C) `INSERT OVERWRITE`
D) `UPDATE SET`

### Section 2: Data Ingestion & Acquisition
**8. (External) When using Auto Loader to ingest JSON files, how does the `cloudFiles.schemaLocation` option help manage the pipeline?**
A) It strictly enforces that any new columns not present in the original schema will cause the job to fail immediately.
B) It stores the inferred schema and tracks schema evolution over time to persist changes across stream restarts.
C) It deletes corrupted JSON files automatically.
D) It routes streaming data into separate tables based on directory structure.

**9. (External) You need to ingest binary files (like images) using Auto Loader. What format should you specify in the `spark.readStream.format("cloudFiles").option("cloudFiles.format", "<format>")` configuration?**
A) `binaryFile`
B) `image`
C) `parquet`
D) `text`

**10. (External) You are designing an append-only data pipeline. Which table property can be set on a Delta Lake table to ensure that updates and deletes are strictly blocked?**
A) `delta.appendOnly = true`
B) `delta.enableDeletionVectors = false`
C) `delta.isolationLevel = Serializable`
D) `delta.autoOptimize.autoCompact = true`

**11. (External) What is a key architectural advantage of Delta Lake compared to standard Parquet files?**
A) Delta Lake does not require a cluster to run Spark SQL.
B) Delta Lake provides ACID transactions and scalable metadata handling via a transaction log.
C) Delta Lake files are stored in a proprietary format that cannot be read by Apache Spark.
D) Delta Lake stores data entirely in memory, eliminating storage costs.

### Section 3: Data Transformation, Cleansing, and Quality
**12. (External) Which Spark SQL window function is used to return the rank of rows within a window partition without leaving gaps in the rank values?**
A) `RANK()`
B) `DENSE_RANK()`
C) `ROW_NUMBER()`
D) `PERCENT_RANK()`

**13. (External) You have identified a data skew issue during a join operation in PySpark. Which of the following is a valid technique to mitigate data skew?**
A) Applying a salt (random key) to the skewed column before joining.
B) Decreasing `spark.sql.shuffle.partitions` to 1.
C) Changing the join type from an inner join to a full outer join.
D) Caching the larger table in memory.

**14. (External) What is the recommended way to quarantine bad records (e.g., malformed JSON) when using Auto Loader?**
A) Use `cloudFiles.rescuedDataColumn` to route malformed records into a dedicated column.
B) Use a try-catch block inside a Python UDF.
C) Use `cloudFiles.inferColumnTypes = false`.
D) It is impossible; Auto Loader simply drops malformed records silently.

**15. (External) In PySpark, which function allows you to rotate data from rows into columns based on distinct values of a specified column?**
A) `transpose()`
B) `pivot()`
C) `melt()`
D) `unpivot()`

**16. (External) When processing data with Pandas UDFs in PySpark, what underlying technology optimizes the data transfer between the JVM and Python?**
A) Apache Kafka
B) Apache Arrow
C) Delta Lake
D) Apache Avro

### Section 4: Data Sharing and Federation
**17. (External) What is the primary function of Lakehouse Federation in Databricks?**
A) To replicate data from external databases directly into Delta Lake files on cloud storage.
B) To securely query and govern data in external systems (like PostgreSQL or Snowflake) directly from Databricks without moving the data.
C) To share data openly with users who do not have a Databricks workspace using D2O sharing.
D) To federate Unity Catalog metastores across different cloud providers.

**18. (External) When setting up Databricks-to-Databricks (D2D) Delta Sharing, what Unity Catalog object is used to group tables and views together to be shared?**
A) A Share
B) A Recipient
C) A Provider
D) A Catalog

**19. (External) In Delta Sharing, what represents the organization or user that is consuming the shared data?**
A) The Provider
B) The Share
C) The Recipient
D) The Credential

**20. (External) When utilizing Lakehouse Federation, how does Databricks ensure queries run efficiently against the external data source?**
A) By downloading the entire external database into memory before executing the query.
B) By using query pushdown to execute as much of the query as possible on the external database engine.
C) By converting all external data to Parquet format temporarily.
D) By disabling caching entirely to ensure real-time accuracy.

### Section 5: Monitoring and Alerting
**21. Which tool should be used to analyze the execution details of a specific Spark SQL query, including data shuffling, bad data skipping, and inefficient types of joins?**
A) Lakeflow Event Logs
B) System Tables
C) Query Profiler
D) Databricks CLI

**22. (External) You want to track auditing information and compute cost utilization across your entire Databricks account. Which feature provides centralized, queryable logs for this purpose?**
A) Spark UI
B) Unity Catalog System Tables
C) Log Analytics Workspace
D) Databricks Asset Bundles

**23. (External) How can you configure an automated notification (e.g., email or Slack) to trigger if a Databricks SQL query results in a value that exceeds a specific threshold?**
A) Databricks SQL Alerts
B) Databricks CLI validation
C) Auto Loader rescue logs
D) Unity Catalog tags

**24. (External) Where can you view the detailed lineage and execution state transitions of a Lakeflow Spark Declarative Pipeline?**
A) The Spark Master logs
B) The Pipeline Event Logs
C) The Cluster Metrics tab
D) The `databricks.yml` file

**25. (External) Which tab in the Spark UI is most useful for investigating memory usage and garbage collection time for individual worker nodes?**
A) Jobs
B) SQL / DataFrame
C) Executors
D) Environment

### Section 6: Cost & Performance Optimisation
**26. A Structured Streaming job deployed to production has been experiencing delays during peak hours of the day... Holding all other variables constant and assuming records need to be processed in less than 10 seconds, which adjustment will meet the requirement?**
A) Use the trigger once option and configure a Databricks job to execute the query every 8 seconds.
B) Decrease the trigger interval to 5 seconds; triggering batches more frequently may prevent records from backing up and large batches from causing a spill.
C) Decrease the trigger interval to 5 seconds; triggering batches more frequently allows idle executors to begin processing the next batch while longer-running tasks from previous batches finish.
D) The trigger interval cannot be modified without modifying the checkpoint directory.

**27. A data ingestion task requires a 1-TB JSON dataset to be written out to Parquet with a target part-file size of 512 MB. Because Parquet is being used instead of Delta Lake, built-in file-sizing features cannot be used. Which approach will work without rearranging the data?**
A) Ingest the data, execute the narrow transformations, repartition to 2,048 partitions, and then write to Parquet.
B) Set spark.sql.adaptive.advisoryPartitionSizeInBytes to 512 MB, ingest the data, execute the narrow transformations, coalesce to 2,048 partitions, and then write.
C) Set spark.sql.files.maxPartitionBytes to 512 MB, ingest the data, execute the narrow transformations, and then write to Parquet.
D) Set spark.sql.shuffle.partitions to 2,048 partitions, ingest the data, optimize the data by sorting it, and then write to Parquet.

**28. To simplify data layout decisions and optimize query performance, what feature is recommended in Delta Lake over traditional Partitioning and ZOrder?**
A) Auto Compaction
B) Liquid Clustering
C) Deletion Vectors
D) Change Data Feed

**29. (External) What is the primary mechanism by which Change Data Feed (CDF) enhances latency and performance in downstream ETL pipelines?**
A) It automatically scales the cluster based on CPU utilization.
B) It identifies and outputs only the row-level changes (inserts, updates, deletes) between table versions, reducing the amount of data processed downstream.
C) It replaces the Delta transaction log with an in-memory Redis cache.
D) It drops historical data instantly to save storage space.

**30. (External) Why does using Unity Catalog managed tables generally reduce operational overhead and maintenance burden compared to external tables?**
A) Managed tables do not require any cloud storage infrastructure.
B) Databricks automatically manages the storage layout, optimization, and file cleanup (like VACUUM) for managed tables.
C) Managed tables bypass ACID compliance, allowing them to write data significantly faster.
D) External tables require manual editing of the Delta transaction log.

**31. (External) How does data skipping work in Delta Lake?**
A) It uses min/max statistics collected in the transaction log to skip reading files that do not match the query predicate.
B) It randomly samples data to return approximate results quickly.
C) It automatically deletes rows that contain NULL values.
D) It partitions the data into separate databases based on query patterns.

### Section 7: Ensuring Data Security and Compliance
**32. A table named user_ltv is being used to create a view that will be used by data analysts... The following view definition is executed:**
`CREATE VIEW email_ltv AS SELECT CASE WHEN is_member('marketing') THEN email ELSE 'REDACTED' END AS email, ltv FROM user_ltv`
**An analyst who is not a member of the marketing group executes the following query: `SELECT * FROM email_ltv`. What will be the result of this query?**
A) Only the email and ltv columns will be returned; the email column will contain the string "REDACTED" in each row.
B) Three columns will be returned, but one column will be named "REDACTED" and contain only null values.
C) Only the email and ltv columns will be returned; the email column will contain all null values.
D) The email and ltv columns will be returned with the values in user_ltv.

**33. (External) In Unity Catalog, what is the native feature used to automatically obfuscate data in a specific column based on the querying user's group membership, without creating a separate view?**
A) Row Filters
B) Column Masks
C) Data Pseudonymization
D) External Locations

**34. (External) To ensure compliance with data privacy regulations (like GDPR), which SQL command permanently removes files containing deleted or modified data from a Delta table's storage?**
A) `DROP TABLE`
B) `TRUNCATE TABLE`
C) `VACUUM`
D) `OPTIMIZE`

**35. (External) You need to securely anonymize sensitive PII data in a pipeline so that the exact original value cannot be reverse-engineered, but the same input always produces the same output for join operations. Which technique should you apply?**
A) Data Suppression
B) Cryptographic Hashing (e.g., SHA-256)
C) Generalization
D) Random Number Generation

**36. (External) Under Unity Catalog's privilege model, what is the principle of least privilege?**
A) Users should only be granted the minimum permissions necessary to perform their required tasks.
B) Workspace admins automatically have read access to all data.
C) All users should be given `SELECT` on the entire catalog to prevent bottlenecks.
D) Service principals should not be assigned permissions.

**37. (External) You want to restrict access so that users in the "EU_Sales" group only see rows in the `global_sales` table where `region = 'EU'`. What Unity Catalog feature directly addresses this?**
A) Column Masks
B) Row Filters
C) `GRANT USAGE ON REGION`
D) Delta Sharing

### Section 8: Data Governance
**38. (External) In the Unity Catalog hierarchy, how are permissions inherited?**
A) Permissions granted on a Catalog are automatically inherited by all Schemas and Tables within that Catalog.
B) Permissions granted on a Table are automatically inherited by the parent Schema.
C) Permissions are strictly isolated and do not inherit; they must be applied explicitly at every level.
D) Permissions inherit across workspaces but not across Catalogs.

**39. (External) What is the top-level container in the Unity Catalog three-level namespace hierarchy?**
A) Workspace
B) Catalog
C) Schema (Database)
D) Table

**40. (External) What happens to the underlying data files when a managed table is dropped in Unity Catalog?**
A) The data files remain in cloud storage indefinitely until manually deleted.
B) The metadata is removed, but the files are moved to a temporary recycle bin.
C) The underlying data files are automatically deleted from the managed storage location within 30 days.
D) The table is converted to an external table.

**41. (External) To make enterprise data more discoverable, where can a data steward add descriptions and metadata tags for tables and columns in Databricks?**
A) The Catalog Explorer
B) The Cluster Configuration UI
C) The `databricks.yml` bundle file
D) The Spark Master UI

### Section 9: Debugging and Deploying
**42. A Databricks job has been configured with three tasks, each of which is a Databricks notebook. Task A does not depend on other tasks. Tasks B and C run in parallel, with each having a serial dependency on task A. What will be the resulting state if tasks A and B complete successfully but task C fails during a scheduled run?**
A) All logic expressed in the notebook associated with tasks A and B will have been successfully completed; some operations in task C may have been completed successfully.
B) Unless all tasks are completed successfully, no changes will be committed to the Lakehouse; because task C failed, all commits will be rolled back automatically.
C) All logic expressed in the notebook associated with tasks A and B will have been successfully completed; any changes made in task C will be rolled back due to task failure.
D) Because all tasks are managed as a dependency graph, no changes will be committed to the Lakehouse until all tasks have successfully been completed.

**43. Databricks Asset Bundles (DABs) use which human-readable file format to co-version code and specify the artifacts, resources, and configurations of a project?**
A) JSON
B) XML
C) HCL
D) YAML

**44. Which Databricks CLI command is used to evaluate your bundle's configuration files and return warnings if unknown resource properties are found before you deploy?**
A) `databricks bundle run`
B) `databricks bundle validate`
C) `databricks bundle init`
D) `databricks bundle deploy`

**45. Within a Databricks Asset Bundle's `databricks.yml` file, which top-level mapping allows you to explicitly define environments (e.g., `development`, `production`) and set dynamic configuration overrides?**
A) `resources`
B) `bundle`
C) `targets`
D) `workspace`

**46. How do you reference a custom variable named `catalog_dev` dynamically inside your Databricks Asset Bundle configuration file?**
A) `{catalog_dev}`
B) `$catalog_dev`
C) `${var.catalog_dev}`
D) `<<catalog_dev>>`

**47. For developers who prefer an IDE, which tool allows you to run Apache Spark code remotely on a Databricks cluster directly from your local environment?**
A) Databricks CLI
B) Databricks Asset Bundles
C) Databricks Connect V2
D) Databricks REST API

**48. In a CI/CD process utilizing GitHub Actions and GitFlow, what action typically triggers the automated deployment of code into the Staging (QA) environment?**
A) A developer running `databricks bundle deploy -t development` locally.
B) The creation of a hotfix branch from the main branch.
C) Merging a pull request into the `dev` branch.
D) Deleting the `databricks.yml` file.

**49. If a critical issue arises in the production environment, how does the GitFlow branching strategy recommend handling it?**
A) Wait for the next scheduled deployment from the `dev` branch.
B) Branch a `hotfix` directly from `main`, apply the fix, and merge it back into both `main` and `dev`.
C) Log into the Databricks UI and directly edit the production notebook code without version control.
D) Roll back the entire Databricks workspace to a snapshot from the previous week.

**50. When establishing environment isolation for CI/CD, what compute and identity configuration is heavily recommended for the Production environment?**
A) Single-node compute running under the primary developer's user account.
B) Serverless compute executed via a service principal identity.
C) Job compute running under a generic shared user account.
D) Interactive compute clusters manually started by the data engineer.

**51. When initializing a Databricks Asset Bundle from a custom organizational template, what minimum files must the template directory contain?**
A) `databricks.yml` and `src/main.py`
B) `requirements.txt` and `setup.py`
C) `databricks_template_schema.json` and `databricks.yml.tmpl`
D) `pytest.ini` and `README.md`

**52. (External) If a multi-task job fails on the last task after the first two tasks succeeded, how can you fix the code and complete the job without re-running the successful tasks?**
A) Use the "Repair Run" feature in the Databricks Jobs UI.
B) Restart the cluster and run the whole job again.
C) Drop the Delta tables and rebuild the data from scratch.
D) It is impossible; you must execute all tasks again.

### Section 10: Data Modelling
**53. A Delta Lake table was created with the following query: `CREATE TABLE prod.sales_by_stor USING DELTA LOCATION "/mnt/prod/sales_by_store"`. Realizing that the original query had a typographical error, the code below was executed: `ALTER TABLE prod.sales_by_stor RENAME TO prod.sales_by_store`. Which result will occur after running the second command?**
A) All related files and metadata are dropped and recreated in a single ACID transaction.
B) The table name change is recorded in the Delta transaction log.
C) A new Delta transaction log is created for the renamed table.
D) The table reference in the metastore is updated.

**54. A Delta Lake table representing metadata about content posts from users has the following schema: `user_id LONG, post_text STRING, post_id STRING, longitude FLOAT, latitude FLOAT, post_time TIMESTAMP, date DATE`. Based on the schema, which column is a good candidate for partitioning the Delta Table?**
A) `post_id`
B) `post_time`
C) `date`
D) `user_id`

**55. The marketing team wants to share data in an aggregate table with the sales organization, but the field names used by the teams do not match, and a number of marketing-specific fields have not been approved for the sales organization. Which solution addresses the situation while emphasizing simplicity?**
A) Create a view on the marketing table selecting only those fields approved for the sales team; alias the names of any fields that should be standardized to the sales naming conventions.
B) Create a new table with the required schema and use Delta Lake's DEEP CLONE functionality to sync up changes.
C) Use a CTAS statement to create a derivative table from the marketing table, and then configure a production job.
D) Add a parallel table write to the current production pipeline.

**56. (External) In dimensional modeling, what is the purpose of a Slowly Changing Dimension (SCD) Type 2?**
A) It completely overwrites old attribute values with new values, destroying history.
B) It adds new columns to the table to store up to 5 previous changes.
C) It creates a new row for every change, maintaining a full history of dimension changes with start and end effective dates.
D) It stores the dimensional data in a JSON array format within a single row.

**57. (External) When utilizing Liquid Clustering in Delta Lake, how does the system handle new data inserts?**
A) It strictly forces incoming data into static directories based on the cluster keys.
B) It incrementally clusters new data during write operations and improves layout over time with background optimization.
C) It prevents writes from succeeding if the new data does not match the existing clustering pattern.
D) It creates a secondary clustered index file alongside the Parquet data.

**58. (External) What is the main characteristic of a "Bronze" table in a Medallion Architecture?**
A) It contains highly refined, business-level aggregated data.
B) It contains raw, unprocessed data ingested directly from the source system.
C) It contains data that has been filtered, cleansed, and joined to conform to a specific schema.
D) It contains data specifically formatted for machine learning model serving.

**59. (External) In a dimensional model (Star Schema), what is the defining characteristic of a Fact table?**
A) It holds descriptive, textual context about business entities.
B) It holds quantitative measurements and metrics of business events, along with foreign keys to dimension tables.
C) It serves as a master lookup list for application configuration settings.
D) It is an append-only log of system error messages.


---

It looks like you didn't include your selected answers in your message, so I cannot calculate your exact score for you! 

However, here is the complete answer key for the 59-question mock exam so you can grade your own responses. Please remember that the answers marked with *(External)* rely on information outside of the provided course materials, and you may want to independently verify that information.

### Section 1: Developing Code for Data Processing using Python and SQL
**1. A** - The connection to the external table will succeed; the string "REDACTED" will be printed.
**2. B** - Schedule a job to execute the pipeline once an hour on a new job cluster.
**3. C** - Pytest.
**4. C** - `.ipynb`
**5. B** - They provide data quality constraints to automatically validate data as it flows through the ETL pipeline.
**6. B** - `assertDataFrameEqual()` *(External)*.
**7. B** - `APPLY CHANGES INTO` *(External)*.

### Section 2: Data Ingestion & Acquisition
**8. B** - It stores the inferred schema and tracks schema evolution over time to persist changes across stream restarts *(External)*.
**9. A** - `binaryFile` *(External)*.
**10. A** - `delta.appendOnly = true` *(External)*.
**11. B** - Delta Lake provides ACID transactions and scalable metadata handling via a transaction log *(External)*.

### Section 3: Data Transformation, Cleansing, and Quality
**12. B** - `DENSE_RANK()` *(External)*.
**13. A** - Applying a salt (random key) to the skewed column before joining *(External)*.
**14. A** - Use `cloudFiles.rescuedDataColumn` to route malformed records into a dedicated column *(External)*.
**15. B** - `pivot()` *(External)*.
**16. B** - Apache Arrow *(External)*.

### Section 4: Data Sharing and Federation
**17. B** - To securely query and govern data in external systems (like PostgreSQL or Snowflake) directly from Databricks without moving the data *(External)*.
**18. A** - A Share *(External)*.
**19. C** - The Recipient *(External)*.
**20. B** - By using query pushdown to execute as much of the query as possible on the external database engine *(External)*.

### Section 5: Monitoring and Alerting
**21. C** - Query Profiler.
**22. B** - Unity Catalog System Tables *(External)*.
**23. A** - Databricks SQL Alerts *(External)*.
**24. B** - The Pipeline Event Logs *(External)*.
**25. C** - Executors *(External)*.

### Section 6: Cost & Performance Optimisation
**26. B** - Decrease the trigger interval to 5 seconds; triggering batches more frequently may prevent records from backing up and large batches from causing a spill.
**27. C** - Set `spark.sql.files.maxPartitionBytes` to 512 MB, ingest the data, execute the narrow transformations, and then write to Parquet.
**28. B** - Liquid Clustering.
**29. B** - It identifies and outputs only the row-level changes (inserts, updates, deletes) between table versions, reducing the amount of data processed downstream *(External)*.
**30. B** - Databricks automatically manages the storage layout, optimization, and file cleanup (like VACUUM) for managed tables *(External)*.
**31. A** - It uses min/max statistics collected in the transaction log to skip reading files that do not match the query predicate *(External)*.

### Section 7: Ensuring Data Security and Compliance
**32. A** - Only the email and ltv columns will be returned; the email column will contain the string "REDACTED" in each row.
**33. B** - Column Masks *(External)*.
**34. C** - `VACUUM` *(External)*.
**35. B** - Cryptographic Hashing (e.g., SHA-256) *(External)*.
**36. A** - Users should only be granted the minimum permissions necessary to perform their required tasks *(External)*.
**37. B** - Row Filters *(External)*.

### Section 8: Data Governance
**38. A** - Permissions granted on a Catalog are automatically inherited by all Schemas and Tables within that Catalog *(External)*.
**39. B** - Catalog *(External)*.
**40. C** - The underlying data files are automatically deleted from the managed storage location within 30 days *(External)*.
**41. A** - The Catalog Explorer *(External)*.

### Section 9: Debugging and Deploying
**42. A** - All logic expressed in the notebook associated with tasks A and B will have been successfully completed; some operations in task C may have been completed successfully.
**43. D** - YAML.
**44. B** - `databricks bundle validate`.
**45. C** - `targets`.
**46. C** - `${var.catalog_dev}`
**47. C** - Databricks Connect V2
**48. C** - Merging a pull request into the `dev` branch.
**49. B** - Branch a `hotfix` directly from `main`, apply the fix, and merge it back into both `main` and `dev`.
**50. B** - Serverless compute executed via a service principal identity.
**51. C** - `databricks_template_schema.json` and `databricks.yml.tmpl`
**52. A** - Use the "Repair Run" feature in the Databricks Jobs UI *(External)*.

### Section 10: Data Modelling
**53. D** - The table reference in the metastore is updated.
**54. C** - `date`.
**55. A** - Create a view on the marketing table selecting only those fields approved for the sales team; alias the names of any fields that should be standardized to the sales naming conventions.
**56. C** - It creates a new row for every change, maintaining a full history of dimension changes with start and end effective dates *(External)*.
**57. B** - It incrementally clusters new data during write operations and improves layout over time with background optimization *(External)*.
**58. B** - It contains raw, unprocessed data ingested directly from the source system *(External)*.
**59. B** - It holds quantitative measurements and metrics of business events, along with foreign keys to dimension tables *(External)*.

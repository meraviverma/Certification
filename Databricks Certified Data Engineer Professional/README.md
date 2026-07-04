The exam covers:

Developing Code for Data Processing using Python and SQL – 22%
Data Ingestion & Acquisition – 7%
Data Transformation, Cleansing, and Quality – 10%
Data Sharing and Federation – 5%
Monitoring and Alerting – 10%
Cost & Performance Optimisation – 13%
Ensuring Data Security and Compliance – 10%
Data Governance – 7%
Debugging and Deploying – 10%
Data Modelling – 6%

Assessment Details
Type: Proctored certification

Total number of scored questions: 59

Time limit: 120 minutes

### Exam outline
## Section 1: Developing Code for Data Processing using Python and SQL
####  Using Python and Tools for development
   - Design and implement a scalable Python project structure optimized for
Databricks Asset Bundles (DABs), enabling modular development, deployment
automation, and CI/CD integration.
   -  Manage and troubleshoot external third-party library installations and
dependencies in Databricks, including PyPI packages, local wheels, and source
archives.
   - Develop User-Defined Functions (UDFs) using Pandas/Python UDF.
### Building and Testing an ETL pipeline with Lakeflow Spark Declarative Pipelines, SQL, and Apache Spark on the Databricks Platform
  - Build and manage reliable, production-ready data pipelines for batch and
streaming data using Lakeflow Spark Declarative Pipelines and Autoloader.
  - Create and Automate ETL workloads using Jobs via UI/APIs/CLI.
  - Explain the advantages and disadvantages of streaming tables compared to
materialized views.
 - Use APPLY CHANGES APIs to simplify CDC in Lakeflow Spark Declarative
Pipelines.
 -  Compare Spark Structured Streaming and Lakeflow Spark Declarative Pipelines to determine the optimal approach for building scalable ETL pipelines.
 - Create a pipeline component that uses control flow operators (e.g., if/else,
for/each, etc.).
● Choose the appropriate configs for environments and dependencies, high
memory for notebook tasks, and auto-optimization to disallow retries.
● Develop unit and integration tests using assertDataFrameEqual,
assertSchemaEqual, DataFrame.transform, and testing frameworks, to ensure
code correctness, including a built-in debugger.
Section 2: Data Ingestion & Acquisition:
● Design and implement data ingestion pipelines to efficiently ingest a variety of data
● formats including Delta Lake, Parquet, ORC, AVRO, JSON, CSV, XML, Text and Binary from
● diverse sources such as message buses and cloud storage.
● Create an append-only data pipeline capable of handling both batch and streaming
data using Delta.
Section 3: Data Transformation, Cleansing, and Quality
● Write efficient Spark SQL and PySpark code to apply advanced data transformations,
including window functions, joins, and aggregations, to manipulate and analyze large
Datasets.
● Develop a quarantining process for bad data with Lakeflow Spark Declarative Pipelines,
or autoloader in classic jobs.
Section 4: Data Sharing and Federation
● Demonstrate delta sharing securely between Databricks deployments using Databricks
to Databricks Sharing (D2D) or to external platforms using the open sharing protocol
(D2O).
● Configure Lakehouse Federation with proper governance across the supported source
Systems.
● Use Delta Share to share live data from Lakehouse to any computing platform.
Section 5: Monitoring and Alerting
● Monitoring
○ Use system tables for observability over resource utilization, cost, auditing and
workload monitoring.
○ Use Query Profiler UI and Spark UI to monitor workloads.
○ Use the Databricks REST APIs/Databricks CLI for monitoring jobs and pipelines.
○ Use Lakeflow Spark Declarative Pipelines Event Logs to monitor pipelines.
● Alerting
○ Use SQL Alerts to monitor data quality.
○ Use the Lakeflow Jobs UI and Jobs API to set up notifications for job status and
performance issues.
Section 6: Cost & Performance Optimisation
● Understand how / why using Unity Catalog managed tables reduces operations
● Overhead and maintenance burden.
● Understand delta optimization techniques, such as deletion vectors and liquid
clustering.
● Understand the optimization techniques used by Databricks to ensure the performance
of queries on large datasets (data skipping, file pruning, etc.).
● Apply Change Data Feed (CDF) to address specific limitations of streaming tables and
enhance latency.
● Use the query profile to analyze the query and identify bottlenecks, such as bad data
skipping, inefficient types of joins, and data shuffling.
Section 7: Ensuring Data Security and Compliance
● Applying Data Security mechanisms.
○ Use ACLs to secure Workspace Objects, enforcing the principle of least privilege,
including enforcing principles like least privilege, policy enforcement.
○ Use row filters and column masks to filter and mask sensitive table data.
○ Apply anonymization and pseudonymization methods, such as Hashing,
Tokenization, Suppression, and generalisation, to confidential data.
● Ensuring Compliance
○ Implement a compliant batch & streaming pipeline that detects and applies
masking of PII to ensure data privacy.
○ Develop a data purging solution ensuring compliance with data retention policies.
Section 8: Data Governance
● Create and add descriptions/metadata about enterprise data to make it more
discoverable.
● Demonstrate understanding of Unity Catalog permission inheritance model.
Section 9: Debugging and Deploying
● Debugging and Troubleshooting
○ Identify pertinent diagnostic information using Spark UI, cluster logs, system
tables, and query profiles to troubleshoot errors.
○ Analyze the errors and remediate the failed job runs with job repairs and
parameter overrides.
○ Use Lakeflow Spark Declarative Pipelines event logs and the Spark UI to debug
Lakeflow Spark Declarative Pipelines and Spark pipelines.
● Deploying CI/CD
○ Build and deploy Databricks resources using Databricks Asset Bundles.
○ Configure and integrate with Git-based CI/CD workflows using Databricks Git
Folders for notebook and code deployment.
Section 10: Data Modelling
● Design and implement scalable data models using Delta Lake to manage large datasets.
● Simplify data layout decisions and optimize query performance using Liquid Clustering.
● Identify the benefits of using liquid Clustering over Partitioning and ZOrder.
● Design Dimensional Models for analytical workloads, ensuring efficient querying and
aggregation.
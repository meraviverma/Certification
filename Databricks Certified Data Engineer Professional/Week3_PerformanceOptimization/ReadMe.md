### Topic Covered
- Introduction to Spark
- Foundational design principles
- Code optimization techniques
- Strategies to fine-tune performance and reduce costs.
- Data skipping
- Best practices for cluster configuration
- How to optimize code and instance selection.

## Agenda
1) Spark Architecture
    - Spark UI Introduction
2) Designing The Foundation
    - File Explosion
    - Data Skipping
3) Code Optimization
    - skew
    - shuffle
    - Spill
    - Join Optimization Lab
    - Serialization
    - User-Defined Function
4) Fine Tunning: Choosing the right Cluster
    - Fine-Tunning Choosing the right Cluster
    - Pick the best Instance Type


## Spark Architecture - Spark UI Introduction
1. Jobs
    - The secret to Spark’s performances is parallelism. Each parallelized
action is referred to as a job. Each job is broken down into stages.
2. Stages
   - Each job is broken down into stages, which is a set of ordered steps
that, together, accomplish a job.
3. Tasks
   - Tasks are created by the driver and assigned a partition of data to
process. These are the smallest unit of work.

## Introduction to Designing Foundation
- Number of bytes read
- Query complexity/computation
- Number of files accessed
- Parallelism

#### Common Performance Bottleneck

1) Small File Problem 
    - Listing and metadata operation for too many small files can be expensive.
    - Can also result in throttling from cloud storage I/O limits
2) Data Skew 
    - Large amounts of data skew can result in more work handled by a single executor 
    - Even if data read in is not skewed, certain transformations can lead to in-memory skew
3) Processing More Than Needed 
    - Traditional data lake platforms often require rewriting entire datasets or partitions

## Demo File Explosion
- concept of over-partitioning in data management, focusing on how excessive partitioning on high cardinality columns can negatively impact query performance. 

1) Take Aways

    - Avoid over-partitioning, particularly on high-cardinality columns, to maintain efficient query performance.

    - Partitioning by columns with unique values for each row, like IDs, can result in inefficient storage and slow query times.

    - Disabling caching helps highlight the impact of partitioning on query performance by removing optimization effects.

    - When working with large datasets (e.g., 50 million rows), improper partitioning can significantly increase processing time.

    - Databricks' **liquid clustering** helps optimize partitioning and mitigate the small file problems caused by over-partitioning.

## Data Skipping and Liquid Clustering
- Data Skipping
- Z-ordering
- Partitioning challenges
- how Liquid Clustering with predictive optimization improves query performance in Databricks Delta Lake.

### Data Skipping
Simple, well-known I/O pruning technique
- Track file-level stats like min & max 
- Leverage them to avoid scanning irrelevant files

```python
SELECT input_file_name() as 
“file_name”,
 min(col) AS “col_min”,
 max(col) AS “col_max”
FROM table
GROUP BY input_file_name()
```

### Z-Ordering
- Z-ordering is a way to organize data within a table based on a specific column so that
similar values are stored together in the same files. 
- Although Databricks currently recommends liquid clustering over Z-ordering or partitioning for new tables, Z-ordering is still sometimes useful. 
- With Z-ordering, data is physically organized by the chosen column, and after this process, each data file will record the minimum and maximum values for that column in its metadata (for example, in the footer of a Parquet file).
- This setup allows Spark, when running a query that filters on the Z-ordered column, to check these min and max values and skip any files that cannot possibly contain the required data, without even opening them. 
- This reduces the number of files that must be read and improves overall query performance, especially for queries filtering on columns with many distinct values.

#### Databricks Delta Lake and Stats

Databricks Delta Lake gathers statistics for the **first 32 columns** in a table, recording values like min and max for each column in the file metadata. These stats let Spark identify which files are likely to contain relevant query results, allowing it to skip over files that don't match the filter criteria

Filters are applied in order: **partition filters, data filters, and then pushed filters.** Because of possible precision or truncation issues,
statistics for timestamp and string columns may not always lead to exact matches,
requiring a fallback to file scanning. It's also best to **avoid collecting statistics for columns with long strings-either** by placing them outside the first 32 columns or
updating configuration settings-to maintain query speed and system efficiency

1) Databricks Delta Lake collects stats about the first N columns
    - dataSkippingNumIndexedCols = 32
2) These stats are used in queries:
- Metadata only queries: select max(col) from table
    - Queries just the Delta Log, doesn't need to look at the files if col has stats
- Allows us to skip files
    - Partition FIlters, Data Filters, Pushed Filters apply in that order
- TimeStamp and String types aren't always very useful
    - Precision/Truncation prevent exact matches, have to fall back to files sometimes
3) Avoid collecting stats on long strings
    - Put them outside first 32 columns or collect stats on fewer columns
    - alter table change column col after col32
    - set **spark.databricks.delta.properties.defaults.dataSkippingNumIndexedCols** = 3

## Partitioning?
1) Generally not recommended!
    - Partitioning is usually misused/overused
    - Tiny file problems or Skew
2) Good use-cases for partitioning
    - Isolating data for separate schemas (single->multiplexing)
    - GDPR/CCP use cases where you commonly delete a partitions worth of data
    - Use cases requiring a physical boundary to isolate data SCD Type 2, partition on
    current or not for better performance.
3) If you partition
    - Choose column with low cardinality
    - Try to keep each partition less than 1tb and greater than 1gb
    - Tables expected to grow TBs
    - Partition (usually) on a date, zorder on commonly used predicates in where clauses

## Liquid Clustering

Liquid clustering is a data layout optimization technique that replaces table partitioning and ZORDER. It simplifies table management and optimizes query performance by automatically organizing data based on clustering keys.


Innovative technique to clustering data layout to support efficient query access and reduce data management and
tuning overhead. It's flexible and adaptive to data pattern changes, scaling, and data skew .

Liquid clustering is an innovative technique that Databricks has developed to reduce data management overhead and provide more support for efficient query access. It offers a flexible and adaptive approach that changes with your data and scales very well, helping to avoid data skew. 

When to use liquid clustering:

- Queries that filter on high cardinality columns.
- Tables with heavy data skew.
- Fast growing tables that require maintenance and tuning effort.
- Tables with concurrent write requirements.
- Tables with varied or changing access patterns.
- Tables where a typical partition key might return results from too many or too few partitions.

You can enable liquid clustering on an existing **unpartitioned table** or during table creation. Clustering is not compatible with **partitioning or ZORDER.**

Benefits:

1) Best performance out of the box
    - Clustering on write
2) Most consistent data skipping
   - Immune to data skew
3) Minimal write amplification on table maintenance
   - True incremental optimize
4) Row Level Concurrency
   - Simplify logic of concurrent writers
5) Reduced Cognitive Overhead
- No worrying about cardinality

**Liquid Clustering** is an innovative data layout technique developed by Databricks to make query access more efficient while significantly reducing the need for manual data management and tuning. As mentioned earlier, Databricks currently recommends Liquid Clustering over traditional Z-ordering or disk partitioning for new tables.

Here is how Liquid Clustering works and why it is an improvement over older methods:

*   **Flexible, "Liquid" Boundaries:** Unlike traditional partitioning that forces data into rigid boundaries (which can create a mess of tiny files), Liquid Clustering targets a specific file size. Databricks intelligently decides which ranges of data to combine so that your file sizes stay roughly the same.
*   **Eliminates Data Skew:** Because the system dynamically combines data to maintain consistent file sizes, it is completely immune to data skew (the problem where some partitions have very little data while others have too much).
*   **Reduced Cognitive Overhead:** With Liquid Clustering, you do not need to worry about whether you are clustering on high or low cardinality columns. The layout is adaptive and changes flexibly as your data patterns scale and evolve over time.
*   **Optimized Maintenance and Writing:** It uses stored metadata to automatically cluster new data into existing clusters on write. Furthermore, it performs "true incremental optimization," meaning there is minimal write amplification during table maintenance, and it supports row-level concurrency to simplify logic when multiple users are writing data at the same time.

Ultimately, Liquid Clustering is designed to give you the best performance and the most consistent data skipping right out of the box, functioning automatically without the strict rules required by older layout methods.

**Liquid Clustering is automatically enabled** so that you get optimal performance right out of the box. 

While the sources do not provide the exact SQL syntax or code required to configure a specific table with this feature, they do illustrate that you can cluster a table by specific columns, such as **customer ID and date**. 

Once it is set up, the system manages the clustering for you:
*   **Automatic clustering on write:** The system stores metadata so that any new data you write is automatically grouped into the existing clusters.
*   **Intelligent file management:** It intelligently decides which ranges of data to combine to reach a target file size, rather than forcing data into rigid boundaries. 

## Enable liquid clustering

1) Create tables with clustering

```python
(DeltaTable.create()
  .tableName("table1")
  .addColumn("col0", dataType = "INT")
  .addColumn("col1", dataType = "STRING")
  .clusterBy("col0")
  .execute())

df = spark.read.table("table1")
df.write.clusterBy("col0").saveAsTable("table2")

df = spark.read.table("table1")
df.writeTo("table1").using("delta").clusterBy("col0").create()

(spark.readStream.table("source_table")
  .writeStream
  .clusterBy("column_name")
  .option("checkpointLocation", checkpointPath)
  .toTable("target_table")
)

```

2) Enable on existing tables

```python
ALTER TABLE <table_name>
CLUSTER BY (<clustering_columns>)
```

3) Convert a partitioned table to liquid clustering

To convert an existing partitioned Delta Lake table to liquid clustering, use REPLACE PARTITIONED BY WITH CLUSTER BY in an ALTER TABLE statement.

ALTER TABLE <table_name>
REPLACE PARTITIONED BY WITH CLUSTER BY [( <clustering_columns> ) | AUTO]


Performance improvements for tables that suffer from poor data-skipping or over-partitioning.
Automatic performance improvements, using CLUSTER BY AUTO, for tables with frequently changing query patterns.
Clustering columns are flexible and simple to alter, whereas partitioning is rigid and difficult to alter.
Reduced write conflicts because tables with liquid clustering allow for row-level concurrency. See Row-level concurrency.

ALTER TABLE t1 REPLACE PARTITIONED BY WITH CLUSTER BY (day, id);
OPTIMIZE t1;

ALTER TABLE t2 REPLACE PARTITIONED BY WITH CLUSTER BY AUTO;

ALTER TABLE t3 REPLACE PARTITIONED BY WITH CLUSTER BY;


## Table Statistics
1) Collects statistics on all columns in table
2) Helps Adaptive Query Execution
   - Choose proper join type
   - Select correct build side in a hash-join
   - Calibrating the join order in a multi-way join

ANALYZE TABLE mytable COMPUTE STATISTICS FOR ALL COLUMNS

## Predictive Optimization

1) Predictive optimization for Unity Catalog managed tables
- Predictive optimization automatically runs OPTIMIZE, VACUUM, and ANALYZE on Unity Catalog managed tables (Delta Lake and Iceberg) on Databricks, eliminating manual maintenance and time spent tracking performance issues.

With predictive optimization enabled, Databricks automatically does the following:
- Identifies tables that would benefit from maintenance operations and queues those operations to run.
- Collects statistics when data is written to a managed table.

Predictive optimization in Databricks runs three key operations on Unity Catalog–managed tables, each designed to balance performance and cost:

### 🔧 Operations
- **OPTIMIZE**  
  - Performs **incremental clustering** for tables with liquid clustering enabled.  
  - Consolidates small files into optimally sized ones, improving query speed and reducing overhead.  
  - Helps maintain efficient data file layout for faster reads.

- **VACUUM**  
  - Cleans up **unused data files** that are no longer referenced by the table.  
  - Reduces storage costs and keeps the environment tidy.  
  - Ensures compliance with retention policies while freeing up space.

- **ANALYZE**  
  - Collects **table statistics** (row counts, column cardinality, distribution).  
  - Improves query planning and execution by giving the optimizer better insights.  
  - Triggered via `ANALYZE TABLE … COMPUTE STATISTICS`.

Here’s how predictive optimization can be enabled or disabled across different scopes in Databricks:

### 🌐 Account Level
- **Who can do it:** Account admins  
- **Effect:** Applies to **all metastores** in the account.  
- **Inheritance:** Catalogs and schemas inherit this setting by default, but you can override it.  
- **Steps:**  
  1. Go to the **Accounts Console**.  
  2. Navigate to **Settings → Feature enablement**.  
  3. Select the option you want (e.g., **Enabled**, **Disabled**).  

---

### 📂 Catalog Level
```sql
ALTER CATALOG catalog_name ENABLE PREDICTIVE OPTIMIZATION;
ALTER CATALOG catalog_name DISABLE PREDICTIVE OPTIMIZATION;
ALTER CATALOG catalog_name INHERIT PREDICTIVE OPTIMIZATION;
```

---

### 📑 Schema (or Database) Level
```sql
ALTER SCHEMA schema_name ENABLE PREDICTIVE OPTIMIZATION;
ALTER SCHEMA schema_name DISABLE PREDICTIVE OPTIMIZATION;
ALTER SCHEMA schema_name INHERIT PREDICTIVE OPTIMIZATION;
```

---

### 📊 Table Level
```sql
ALTER TABLE table_name ENABLE PREDICTIVE OPTIMIZATION;
ALTER TABLE table_name DISABLE PREDICTIVE OPTIMIZATION;
ALTER TABLE table_name INHERIT PREDICTIVE OPTIMIZATION;
```

---

🔎 **Key Point:**  
- **ENABLE** → Turns predictive optimization on explicitly.  
- **DISABLE** → Turns it off explicitly.  
- **INHERIT** → Follows the setting from the parent (schema → catalog → account).  

This hierarchy gives you fine-grained control: you can keep predictive optimization on globally but disable it for specific catalogs, schemas, or tables where you don’t want automatic OPTIMIZE, VACUUM, and ANALYZE operations running.


---

To verify whether **predictive optimization** is enabled for a catalog, schema, or table, you can use the `DESCRIBE EXTENDED` command. The **Predictive Optimization** field in the output will show the current status:

### ✅ Syntax
```sql
DESCRIBE CATALOG EXTENDED catalog_name;
DESCRIBE SCHEMA EXTENDED schema_name;
DESCRIBE TABLE EXTENDED table_name;
```

### 📊 Output Interpretation
- **ENABLED** → Predictive optimization is explicitly turned on.  
- **DISABLED** → Predictive optimization is explicitly turned off.  
- **INHERIT** → The object inherits the setting from its parent (schema → catalog → account).  

For example:
```sql
DESCRIBE TABLE EXTENDED patient_records;
```
might return:
```
Predictive Optimization: INHERIT (from schema)
```

This means the table follows whatever setting is applied at the schema level. If the schema is also set to **INHERIT**, it will ultimately follow the catalog or account-level configuration.

---

👉 Best practice:  
- Use **INHERIT** for most objects so they follow the global account/catalog policy.  
- Override with **ENABLE** or **DISABLE** only for special cases (e.g., staging tables where you don’t want automatic OPTIMIZE/VACUUM/ANALYZE).  


### ⚙️ Key Features Predictive Optimization

- **Automatic Maintenance**  
  - Runs background tasks on Delta tables without manual intervention.  
  - Keeps tables performant and storage-efficient continuously.

- **Supported Maintenance Operations**  
  - **OPTIMIZE** → Improves query performance by optimizing file sizes.  
  - **VACUUM** → Reduces storage costs by deleting unused data files.  
  - **ANALYZE** → Collects table statistics to enhance query planning.

- **Set-and-Forget Approach**  
  - Once enabled, it intelligently schedules and executes maintenance jobs.  
  - No ongoing supervision required from users.

- **Serverless Computing**  
  - Uses serverless compute resources.  
  - Eliminates the need to manually manage or provision clusters.  

---

# Lab: Data Skipping and Liquid Clustering


# Code Optimization
- Skew
- Shuffle
- Spill
- Serialization
- Adaptive Query Execution in action

# Skew

**Data skew** occurs when your data becomes unevenly distributed across different partitions, resulting in some partitions containing significantly more records than others. 

For example, if you aggregate a dataset based on a "City" column, and one city has a population that is twice as large as the others, the resulting partition for that specific city will naturally hold twice as much data. 

This imbalance creates major performance bottlenecks. Because that skewed partition contains more records, it requires **twice as much RAM and takes twice as long to process**. In distributed systems like Spark, a processing stage can only finish when its longest-running task completes, meaning **the entire system is slowed down by that single bloated partition**. Extreme cases of data skew can even cause data to spill over to disk or trigger hard-to-diagnose Out of Memory (OOM) errors.

Fortunately, there are several ways to handle and mitigate data skew:

*   **Liquid Clustering:** As we discussed earlier, Databricks' newer Liquid Clustering feature intelligently combines data ranges to target a specific file size, effectively eliminating data skew automatically without rigid partition boundaries.
*   **Adaptive Query Execution (AQE):** Enabled by default in newer versions of Spark, AQE automatically detects skewed partitions—specifically, partitions that are at least 256MB and 5 times larger than the average partition size. It then breaks those massive partitions down into smaller, similar-sized pieces so the workload can be evenly distributed across your compute cores. 
*   **Filtering:** If the skew is caused by irrelevant data—such as joining on a column that contains a massive amount of `null` values—you can often fix the issue simply by filtering those values out.
*   **Skew Hints & Salting:** In situations where AQE cannot automatically solve the issue, you can use "skew hints" to explicitly tell Spark which table, column, and values are causing the skew so it can try to resolve it. If all else fails, you can use a manual technique called "salting," which involves appending random integer suffixes to the skewed column values to force that massive partition to break apart.

### How does AQE decide when to split a skewed partition?

Adaptive Query Execution (AQE) decides to split a skewed partition by checking it against two default thresholds:

*   The partition contains at least **256MB of data**.
*   The partition is **at least 5 times larger** than the average partition size.

If a partition meets both criteria, AQE automatically breaks it down into smaller, similar-sized pieces. You can also change these default values to fine-tune AQE's behavior for your specific workloads.

However, AQE can only make this decision if your job uses **2,000 or fewer shuffle partitions**. If your job exceeds 2,000 shuffle partitions, Spark stops tracking specific block sizes and only retains the average sizes, which completely prevents AQE from detecting skew. To fix this, you must either reduce the number of shuffle partitions to under 2,000 or adjust your Spark configurations to a value greater than your shuffle partition count.


# Shuffle

Now that I have the specific documentation on the topic, I can provide a more precise definition based on your materials!

In Apache Spark, a **shuffle** is the act of moving data from the output of one stage to the input of another. It is a critical operation that involves redistributing and reorganizing data across different partitions. 

Here is a breakdown of how and why shuffles occur:

*   **Triggered by Wide Transformations:** Shuffles are a side effect of **wide transformations**. These are operations that require two stages to complete, such as `join()`, `distinct()`, `groupBy()`, or `orderBy()`. 
*   **The Map and Reduce Process:** During a two-stage job, data is first read into Stage 1 where a "map" transformation is performed. The results of this mapping dictate that the data needs to be reorganized and moved into Stage 2 so it can be pushed through a "reducer" function. The movement between these two stages is the shuffle.
*   **Physical Network Movement:** Because different workers are performing specific tasks, shuffling means the data is being sent "all over the place" and represents actual **network movement from one worker to another** across your cloud service provider. 

Since shuffles involve moving data across the network (which is computationally expensive), mitigating or minimizing them is a key optimization strategy. Based on the sources, there are several ways you can mitigate the impact of shuffles:

*   **Reduce the Amount of Data:** The simplest way to lessen the impact of a shuffle is to shrink the data before it happens. You should preemptively filter out unnecessary records and remove any unneeded columns.
*   **Optimize Your Cluster Hardware:** You can reduce network I/O overhead by using fewer, larger workers (which means you still pay for disk I/O, but reduce the network traffic). Additionally, using fast storage like NVMe and SSDs will speed up the necessary shuffle reads and writes.
*   **Re-evaluate Join Strategies:** If a shuffle is caused by a join operation, you can rethink how the join is executed:
    *   *Change the order:* Reorder the join.
    *   *Switch the strategy:* Dynamically switch to different join strategies, such as a Broadcast Hash Join, Shuffle Hash Join (which is the default for Databricks Photon), or Sort-Merge Join (which is the default for open-source Spark).
*   **Denormalize Datasets:** You can denormalize your datasets to avoid the join (and the resulting shuffle) altogether. However, the sources note that with modern Spark features like Adaptive Query Execution (AQE), this strategy is becoming less necessary.
*   **Bucketing:** Bucketing pre-sorts your partitions, which eliminates the sorting step in a Sort-Merge Join. However, the sources heavily caution against this strategy, noting that it is "hard to get right and is an expensive operation" (especially for datasets that change frequently). In fact, the sources explicitly advise that bucketing is not worth considering unless your datasets are larger than 1 to 5 terabytes.


## Demo Shuffle
- Data shuffling across executors introduces network overhead, which can significantly slow down job execution.

- Misconfigured defaults, such as a disabled cache or an improperly set auto-broadcast join threshold, can cause unnecessary shuffles during join operations.

- Broadcast joins are ideal for small tables (under ~100MB) and help avoid shuffling by sending the entire table to each executor.

- Aggregations typically involve shuffling, but are generally less resource-intensive than joins.

- Fine-tuning Spark settings and understanding your data's movement through the system are key to reducing execution time and improving performance.

# Serialization

Here’s a polished **Markdown summary** of Spark UDF serialization issues and mitigation strategies, structured for clarity and easy reference:

---

## 🚀 Apache Spark UDF Serialization

## 🔎 What is Serialization in Spark?
Serialization is the process of packaging custom **User-Defined Functions (UDFs)** and distributing them to executors across the cluster so they can run in parallel.  

While Spark’s native SQL and DataFrame APIs are highly optimized, UDF serialization introduces significant performance bottlenecks.

---

## ⚠️ Why Serialization Hurts Performance

### 1. Massive Row-by-Row Overhead
- Every parameter and return value of a UDF must be serialized/deserialized for **each row**.
- This creates heavy computational overhead and slows down execution.

### 2. The "Black Box" Penalty
- Spark’s **Catalyst Optimizer** cannot analyze inside UDFs.
- Treats them as opaque black boxes → prevents query-level optimizations before and after the UDF.

### 3. Python’s Extra Burden
- Python UDFs are the slowest:
  - Require **pickling** for serialization.
  - Spark must start a **Python interpreter** on every executor.
  - Each row must be converted back and forth between Python objects and Spark DataFrame rows.

---

## ✅ How to Mitigate Serialization Issues

### 🔹 1. Avoid UDFs Entirely
- Prefer **Spark built-in functions** (e.g., `array`, `map`, `filter`, `transform`).
- Port business logic into native Spark SQL/DataFrame APIs whenever possible.

### 🔹 2. Use Vectorized UDFs
- If Python UDFs are unavoidable:
  - Use **Vectorized UDFs** (Pandas UDFs with Apache Arrow).
  - Process batches of rows at once instead of row-by-row.

### 🔹 3. Use Typed Transformations (Scala)
- For Scala:
  - Prefer **typed transformations** (`map`, `flatMap`, `reduce`) over stock UDFs.
  - These integrate better with Spark’s optimizer.

---

## 📦 Serialization in Data Spill
- When Spark spills data from **RAM → Disk**, it serializes and compresses the data.
- That’s why **Spill (Disk)** size in Spark UI is always smaller than **Spill (Memory)** size.

---

## 📝 Key Takeaways
- UDFs = flexibility but poor performance.
- Native Spark functions = best optimization path.
- Vectorized UDFs (Python) and typed transformations (Scala) = safer alternatives.
- Serialization is unavoidable, but smart design minimizes its impact.

---

# Demo: User-Defined Functions
You’ll see how Python UDFs can introduce bottlenecks and learn strategies to improve execution

- UDFs can slow down Spark queries significantly if not used carefully.

- Python UDFs require serialization and don’t automatically take advantage of Spark’s built-in parallelism.

- Repartitioning data to match the number of available cores can help speed up Python UDF execution.

- SQL UDFs are more efficient than Python UDFs because they avoid serialization and benefit from Spark’s Catalyst optimizer.

- For best performance, favor built-in functions or SQL-based UDFs whenever possible.


```python
from pyspark.sql.functions import *
from pyspark.sql.types import *
import time

## Create the Python UDF
@udf("double")
def F_to_Celasius(f):
    # Let's pretend some fancy math takes one second per row
    time.sleep(1)
    return (f - 32) * (5/9)

spark.sql('DROP TABLE IF EXISTS celsius')

## Prep the data
celsius_df = (spark
              .table('device_data')
              .withColumn("celsius", F_to_Celsius(col('temperature_F')))
            )

## Create the table
(celsius_df
 .write
 .mode('overwrite')
 .saveAsTable('celsius')
)
```

Summary
The repartition command is a recommended best practice to ensure your UDF runs in a parallel, distributed manner.

```sql
%sql

-- Create the same function
DROP FUNCTION IF EXISTS farh_to_cels;

CREATE FUNCTION farh_to_cels (farh DOUBLE)
  RETURNS DOUBLE RETURN ((farh - 32) * 5/9);


-- Use the function to create the table
DROP TABLE IF EXISTS celsius_sql;

CREATE OR REPLACE TABLE celsius_sql AS
SELECT farh_to_cels(temperature_F) as Farh_to_cels_convert 
FROM device_data;


-- View the data
SELECT * 
FROM celsius_sql;
```

# Fine-Tuning: Choosing the Right Cluster

# ⚡ Databricks Cluster Optimization Guide

Choosing and fine-tuning the right Databricks cluster depends heavily on the **specific workload** you are running. Databricks provides three primary cluster types, along with several strategies to maximize performance and minimize costs.

---

## 1️⃣ Choose the Right Cluster Type

- **All-Purpose Compute (Interactive):**
  - Best for **Data Science & Engineering development** and iterative testing.
  - ✅ Enable **Auto-Scale** and **Auto-Stop** to avoid paying for idle compute.
  - ✅ Develop/test on a **subset of data** before scaling.

- **Jobs Compute:**
  - Designed for **production workflows, ingestion, ETL batch jobs**.
  - Single-user clusters → lower cost + better isolation for debugging.
  - ⚙️ Size clusters **statically** according to SLA requirements.

- **SQL Warehouses:**
  - Optimized for **high-concurrency ad-hoc SQL analytics & BI reporting**.
  - 🚀 Includes **Photon engine** out-of-the-box.
  - ☁️ Serverless options → instant startup + reduced costs.

---

## 2️⃣ Fine-Tune with Autoscaling

- **Ad-hoc / Business Analytics:**
  - Set a **large variance range** for workers.
  - Allows cluster to adapt to unpredictable query loads.

- **Production Batch Jobs:**
  - Static sizes usually sufficient.
  - Use autoscaling only as a **buffer** for unexpected spikes.

---

## 3️⃣ Optimize Compute Costs with Spot Instances

- Spot instances = spare cloud VMs at **discounted rates**.
- 💡 **Golden Rule:**  
  - **Never use a spot instance for the driver node!**
  - Keep driver on **on-demand instance**.
  - Use spot instances for **workers** only.
- For tight SLAs → configure **spot fallback to on-demand**.

---

## 4️⃣ Enable the Photon Engine

- Photon = highly optimized query engine.
- Benefits:
  - ⚡ Up to **40% ETL cost savings**.
  - ⚡ Faster query performance.
  - ⚡ Zero tuning/setup required.
  - ⚡ Supports SQL, Python, Scala, R, Java.

---

## 5️⃣ VM Selection & Runtime Best Practices

- Always use the **latest LTS Databricks Runtime**.
- Select **latest generation VMs**.
- Start with **general-purpose VMs** → experiment with memory/computation-optimized VMs for specific workloads.

---

## 📝 Key Takeaways
- Match cluster type to workload (Interactive, Jobs, SQL Warehouses).
- Use autoscaling wisely → large ranges for ad-hoc, static for production.
- Spot instances = cost saver, but **never for drivers**.
- Photon engine = best TCO + performance boost.
- Keep runtime & VM selection **up-to-date** for maximum efficiency.


## Cluster Optimization Recommendations

For optimal cluster performance and cost management, Databricks provides five specific recommendations for tailoring your cluster type and configuration to your workload:

*   **Data Science and Data Engineering (DS & DE) Development:** Use all-purpose compute clusters with auto-scale and auto-stop enabled, and make sure to develop and test on a subset of your data to keep resource use minimal.
*   **Ingestion and ETL Jobs:** Use jobs compute clusters and size them according to the specific Service Level Agreement (SLA) required for the job.
*   **Ad-hoc SQL Analytics:** Use a (serverless) SQL warehouse and ensure that auto-scale and auto-stop are enabled.
*   **BI Reporting:** Use an isolated SQL warehouse that is sized specifically according to your BI SLAs.

In addition to choosing the right cluster types, you should follow these general best practices for configuration:
*   Enable **spot instances** specifically on your worker nodes to save on costs.
*   Use the **latest LTS (Long Term Support) Databricks Runtime** whenever possible for performance and stability.
*   Enable the **Photon engine** for the best Total Cost of Ownership (TCO) when applicable.
*   Select the **latest generation VMs**, starting your tests with general-purpose VMs before testing memory-optimized or compute-optimized VMs to see what works best.

## 🔄 Databricks Autoscaling Guide

## 🌟 What is Autoscaling?
Autoscaling enables a Databricks cluster to **dynamically resize itself** by automatically adjusting the number of worker nodes based on workload demands.

- When demand increases → cluster size grows → faster execution.  
- When demand decreases → cluster size shrinks → reduced costs compared to fixed-size clusters.

---

## ⚙️ How to Configure Autoscaling
To enable autoscaling:
1. Activate the **Autoscaling** feature.
2. Define a **minimum and maximum number of workers**.
3. Experiment to find the optimal range for your workload.

---

## 📌 Use-Case Recommendations

- **Ad-hoc usage / Business Analytics**
  - Set a **large variance range** (higher upper limit).
  - Provides flexibility for unpredictable query loads.

- **Production Batch Jobs**
  - Autoscaling often **not required** (static clusters suffice).
  - Can set an **upper limit buffer** for occasional spikes.

- **Streaming**
  - Fully supported in streaming environments.
  - Works seamlessly with **Delta Live Tables**.

---

## ☁️ Serverless Autoscaling
- For **serverless clusters** (e.g., serverless SQL warehouses):
  - Autoscaling is **completely automatic**.
  - System optimizes resources without manual intervention.

---

## 📝 Key Takeaways
- Autoscaling = balance between **performance** and **cost efficiency**.
- Large ranges → best for **ad-hoc analytics**.  
- Static sizing → best for **predictable batch jobs**.  
- Streaming + serverless → autoscaling is **fully automated**.



## Photon

**Photon** is Databricks' **world-record-achieving query engine** built specifically for modern hardware that operates with **zero tuning or setup required**. As we touched upon earlier when discussing cluster selection and shuffles, it is designed to execute a massive variety of workloads seamlessly on a single engine. 

Here are the key features and benefits of using Photon:

*   **Massive Performance Gains:** Photon has set data warehousing world records by outperforming previous benchmarks by more than 2x. It delivers up to **12x better price-to-performance** compared to other cloud data warehouses and offers a 2-3x reduction in query execution times compared to open-source (OSS) Spark.
*   **Significant Cost Savings:** Because it processes data so quickly, your clusters run for shorter periods. Databricks notes that ETL customers have saved up to **40% on their compute spend** simply by enabling Photon.
*   **Seamless Adoption (No Code Changes):** One of its biggest advantages is that you do not need to rewrite or tune your existing code to use it. Photon integrates directly with standard Spark APIs and offers broad language support for **SQL, Python, Scala, R, and Java**.
*   **Versatility:** It is capable of handling everything from exploration and ETL to high-concurrency, low-latency, batch, and streaming workloads, regardless of whether you are dealing with small or big data.
*   **Availability:** Photon comes **automatically included on Databricks SQL Warehouses**. For other cluster types, Databricks highly recommends enabling it as a best practice to achieve the best Total Cost of Ownership (TCO) whenever applicable.

## Spot Instance
---

**Spot instances** are spare, currently unused virtual machine (VM) instances offered by cloud service providers at a significantly lower, below-market rate. They allow you to take advantage of available compute resources and **reduce your overall costs**, making them an excellent option for ad-hoc or shared clusters.

However, the trade-off for the discounted price is that **the cloud provider can reclaim the instances at any time if demand increases**. Because they can be suddenly withdrawn, they are generally not recommended for mission-critical jobs with strict Service Level Agreements (SLAs).

If you decide to use spot instances to optimize your cluster, Databricks recommends following these best practices:

*   **Never use a spot instance for your driver node!** You should always keep your driver on an on-demand VM instance to ensure the job's control layer remains stable.
*   **Use spot instances exclusively for your worker nodes**. This ensures that if the spot instances are reclaimed by the provider, your workers might drop, but your main job will not completely fail because the on-demand driver is still running.
*   If you have a workflow with a tight SLA, you can combine these options by using **spot instances with a fallback to on-demand instances**. This configuration allows you to benefit from the cost savings when spot capacity is available, while preventing disruptions if the capacity is withdrawn.

# Instance Type

An **instance type** refers to the specific virtual machine (VM) hardware configuration provided by a cloud service provider (such as AWS, Azure, or GCP) that you use to run your Databricks cluster nodes. 

When you select an instance type, you are choosing the underlying hardware that will execute your workloads. According to the sources, you should focus on four main characteristics when evaluating an instance type:

*   **Core-to-RAM ratio:** The amount of memory available per processing core (for example, a machine with a ratio of 1 core to 2 GB of RAM).
*   **Processor type:** The speed and generation of the CPU, such as an Intel Cascade 3.6 GHz, Intel Xeon, or an AWS Graviton processor.
*   **Local vs. remote storage:** Whether the machine has physically attached local disks or relies on network-attached remote storage.
*   **Storage medium:** The type of drive used, such as extremely fast local NVMe drives versus standard local SSDs.

Cloud providers group these machine configurations into different "families" that are optimized for specific types of work:
*   **AWS:** Examples include the `c5` or `c7gd` (compute-optimized), `m7gd` (general purpose), and `r7gd` (memory-optimized).
*   **Azure:** Examples include the `f-series`, `eav4`, `dav4`, and `Edsv4`.
*   **GCP:** Examples include `n2-highcpu`, `n2-standard`, and `n2-highmem`.

## Find Best Instance Type

When choosing the best instance types for your Databricks cluster, you should keep an open mind and test different configurations to see what works best for your specific workload. 

The most important hardware factors to consider are the **core-to-RAM ratio**, **processor type**, **local versus remote storage**, and the **storage medium** (such as fast local NVMe versus slower remote disks).

Here is a guide to selecting the right instances based on the provided sources:

### 1. General Cloud Recommendations
*   **AWS:** Do not automatically default to the i3 series. You should explore **m7gd and r7gd** instances, which have better processors and much more stable spot market pricing. Graviton instances also work very well and should be tried early on.
*   **Azure:** Try the **eav4, dav4, and F-series** instances before defaulting to the L-series, and use the ACU (Azure Compute Unit) metric to compare VM performance.
*   **GCP:** The default instances recommended by GCP are generally quite good.
*   *Note on network-optimization:* Network-optimized instances are rarely needed, though they can sometimes help if you are using the Photon engine.

### 2. General Rules of Thumb
*   Keep the total memory of any single machine **under 128 GB**.
*   For your very first run, set your shuffle partitions (`spark.sql.shuffle.partitions`) to **twice the number of cores** in your cluster. 
*   Avoid carrying over legacy configurations from old platforms unless absolutely necessary.
*   Remember that a larger cluster isn't necessarily more expensive. If you double the size of the cluster and it runs the job in half the time, the compute cost is exactly the same. 

### 3. Sizing the Driver Node
Unless you are trying to be the absolute cheapest, you should simply **leave your driver node the exact same size as your worker nodes** (typically 4 to 8 cores and 16 to 32 GB of RAM). 

You only need to increase the size of your driver if you are:
*   Running many concurrent jobs or streams on the same machine.
*   Committing massive amounts of data (100,000+ files) to a Delta table.
*   Collecting a large amount of data back to the driver to use in Pandas or R.

### 4. The "If This, Then That" (IFTTT) Selection Process
Databricks recommends a step-by-step decision tree to pick your specific VM families:

**Step 1: Are you using the Photon Engine?**
*   **Yes:** Start with AWS (`m6gd`, `r6gd`, `i4i`, `m7gd`, `r7gd`), Azure (`Edsv4`), or GCP (`n2-highmem`, `n2-standard`).
*   **No:** Proceed to Step 2.

**Step 2: Is your job an ETL workload using joins, windows, group bys, or aggregations?**
*   **Yes:** Start with AWS (`c7gd`, `c6gd`), Azure (`fsv2`), or GCP (`n2-highcpu`).
*   **No:** Start with AWS (`c7g`, `c6g`), Azure (`fsv2`), or GCP (`e2-highcpu`).

**Step 3: Run the job and check for Data Spill**
Once your cluster is running, check the Spark UI for the longest-running query. Are you experiencing data spill?
*   **No:** Stop here. Your cluster is perfectly tuned. 
*   **Yes:** Update your `spark.sql.shuffle.partitions` to `auto` (or divide your largest shuffle read stage size by 200MB), and run the job again. 

**Step 4 & 5: Upgrade Instance Types if Spill Persists**
If you updated your shuffle partitions but still see spill, you need to upgrade to memory-optimized instances. 
*   **First Upgrade:** AWS (`m7gd`), Azure (`dav4`, `dasv4`), GCP (`n2-standard`).
*   **Second Upgrade:** If it still spills, move to AWS (`r7gd`, `r6gd`), Azure (`Edsv4`), or GCP (`n2-highmem`).
%md
# Types of Tasks

Configuration options and instructions vary by task.  
The following task types are available:

- 📓 **Notebook**  
- 🖼️ **Visual data prep (Lakeflow designer)**  
- 🔒 **Clean Room notebook**  
- 🐍 **Python script**  
- 📦 **Python wheel**  
- 🧮 **SQL**  
- 🔗 **Pipeline**  
- 🔄 **Database Table Sync pipeline**  
- 📥 **Ingestion pipeline**  
- 🔔 **SQL Alert (Public Preview)**  
- 📊 **Dashboards**  
- 📈 **Power BI**  
- 🛠️ **dbt**  
- 🛠️ **dbt platform (Public Preview)**  
- 📁 **JAR**  
- ⚡ **Spark Submit**  
- 🗂️ **Run Job**  
- 🔀 **If/else**  
- 🔁 **For each**

# SET TAG

**Applies to:** ✅ Databricks SQL, ✅ Databricks Runtime 16.1 and above  

Sets a tag on a catalog, schema, table, view, volume, column, function, or external metadata object.

---

### 🔄 Public Preview
- Setting tags on **external metadata objects** is in *Public Preview* and requires **Databricks Runtime 18.2 or above**.

---

### 🔑 Permissions
To add tags to Unity Catalog securable objects, you must:
- Own the object **OR** have all of the following privileges:
  - `APPLY TAG` on the object  
  - `USE SCHEMA` on the object's parent schema  
  - `USE CATALOG` on the object's parent catalog  

ℹ️ External metadata objects are not contained in a catalog or schema, so `USE CATALOG` and `USE SCHEMA` do not apply.  

If the tag is **governed**, you also need `ASSIGN` permission on the governed tag.

---

### 📝 Syntax
```sql
SET TAG ON
    { CATALOG catalog_name |
      COLUMN relation_name . column_name |
      EXTERNAL METADATA external_metadata_name |
      { FUNCTION | PROCEDURE } function_name |
      { SCHEMA | DATABASE } schema_name |
      TABLE relation_name |
      VIEW relation_name |
      VOLUME volume_name }
    tag_key [ = tag_value ]

```

# Required Permissions

### 🛠️ Unity Catalog – MODIFY permission
You must have **MODIFY** permission to perform the following operations:
- `ALTER COLUMN`
- `ADD COLUMN`
- `DROP COLUMN`
- `SET TBLPROPERTIES`
- `UNSET TBLPROPERTIES`

---

### 🔑 Unity Catalog – MANAGE permission or ownership
You must have **MANAGE** permission **or** be the **owner** of the object to perform:
- `SET OWNER TO`
- `PREDICTIVE OPTIMIZATION`

%md
# Setting Tags in Databricks SQL

In Databricks SQL, there are **two forms** to set tags on tables:

---

### 🔹 Plural Form – `ALTER TABLE ... SET TAGS`
Allows setting one or multiple tags at once.

```sql
-- Multiple tags
ALTER TABLE table_name 
SET TAGS ('tag_key1' = 'tag_value1', 'tag_key2' = 'tag_value2');

-- Single tag
ALTER TABLE table_name 
SET TAGS ('tag_key' = 'tag_value');

---

### 🔹 Singular Form – `SET TAG ON TABLE`
Used to set **individual tags**.

```sql
-- With key-value
SET TAG ON TABLE table_name tag_key = tag_value;

-- Key-only (value optional)
SET TAG ON TABLE table_name tag_key;
```

---

### ℹ️ Notes
- `tag_value` is **optional**.  
- If not specified, the tag assignment will be set as **key-only**.

CREATE TABLE AS SELECT statements, or CTAS statements create new Delta tables and populate them using the output of a SELECT query. So, when dropping the source table, the target table will not be affected.

# 📋 Clone a Table in Databricks

You can clone a **Delta Lake** or **Apache Iceberg** table using the `CLONE` command to create an independent copy at a specific version.

- **Deep clones** → Copy both **data and metadata**.  
- **Shallow clones** → Copy only **metadata**, referencing source data files (cheaper in compute and storage).

Databricks also supports cloning **Parquet** and **Apache Iceberg** tables.  
For Unity Catalog, see **Shallow clone for Unity Catalog tables**.

> ℹ️ **Note:** Streaming tables and materialized views do **not** support `CLONE`.

---

## 🔄 Clone Types

| Type          | SQL Syntax        | Description |
|---------------|------------------|-------------|
| **Deep clone** | `CLONE` or `DEEP CLONE` | Copies both data and metadata (including stream metadata). Streams can resume on the clone target. |
| **Shallow clone** | `SHALLOW CLONE` | Copies only metadata (schema, partitioning, invariants, nullability, TBLPROPERTIES). No data files copied. Cheaper to create. |

**Metadata cloned:** schema, partitioning info, invariants, nullability, TBLPROPERTIES.  
**Deep clone only:** stream metadata, `COPY INTO` metadata.  
**Not cloned:** table description, user commit metadata, Delta Lake history, Unity Catalog properties (tags).

---

## 📊 Clone Metrics

After completion, `CLONE` reports metrics as a single row DataFrame:

- `source_table_size` → Size of source table (bytes)  
- `source_num_of_files` → Number of files in source table  
- `num_removed_files` → Files removed if replacing a table  
- `num_copied_files` → Files copied (0 for shallow clones)  
- `removed_files_size` → Size of removed files (bytes)  
- `copied_files_size` → Size of copied files (bytes)

---

## 🔑 Permissions

### Table Access Control
- `SELECT` on source table  
- `CREATE` on database (if creating new table)  
- `MODIFY` on table (if replacing existing table)

### Cloud Provider Permissions
- **Deep clone:**  
  - Readers → read access to clone’s directory  
  - Writers → write access to clone’s directory  
- **Shallow clone:**  
  - Readers → read access to both source data files and clone’s directory  
  - Writers → write access to clone’s directory  

---

## ✅ Recommendation
For cross‑organization **read‑only access**, Databricks recommends **OpenSharing**.  
See: *What is OpenSharing?*

%md
# 🔄 Create Deep or Shallow Clones

The following examples demonstrate how to create **deep** and **shallow clones** of tables in Databricks.

---

## 🟦 Deep Clone

```sql
-- Create a deep clone
CREATE TABLE target_table CLONE source_table;

-- Replace an existing target
CREATE OR REPLACE TABLE target_table CLONE source_table;

-- Create a deep clone, or skip if target already exists
CREATE TABLE IF NOT EXISTS target_table CLONE source_table;

```
# ⚡ Automatic Liquid Clustering in Databricks

Automatic Liquid Clustering dynamically adapts to evolving and unpredictable query patterns by continuously reorganizing data based on recent query filters.  
This improves performance when query predicates vary across multiple columns, where static strategies like **partitioning** or **Z-ordering** are less effective.

Automatic Liquid Clustering is designed to dynamically adapt to evolving and unpredictable query patterns by continuously reorganizing data based on recent query filters. This is especially beneficial when query predicates frequently change across multiple columns, making static strategies like partitioning or Z-ordering less effective.

Partitioning works best when filters are stable and predictable, often on date/time columns. Z-ordering optimizes clustering for known high-cardinality columns with consistent filtering. When query filters are varied and unpredictable, Automatic Liquid Clustering provides the agility to improve performance without manual tuning.

Liquid clustering is a data layout optimization technique that replaces table partitioning and ZORDER. It simplifies table management and optimizes query performance by automatically organizing data based on clustering keys.

Unlike traditional partitioning, you can redefine clustering keys without rewriting existing data. This allows your data layout to evolve alongside changing analytic needs. Liquid clustering applies to both streaming tables and materialized views.
---

## 🔹 When to Use
Databricks recommends liquid clustering for **all new tables**, including streaming tables and materialized views.  
It is especially beneficial for:
- Queries filtering on **high cardinality columns**  
- Tables with **heavy data skew**  
- **Fast-growing tables** requiring maintenance and tuning  
- Tables with **concurrent write requirements**  
- Tables with **changing or varied access patterns**  
- Tables where partition keys return too many or too few partitions  

---

## 🛠️ Enable Liquid Clustering

### During Table Creation

```sql
-- Create table with clustering
CREATE TABLE table1 (col0 INT, col1 STRING) CLUSTER BY (col0);

-- Create from existing data with clustering
CREATE TABLE table2 CLUSTER BY (col0)
AS SELECT * FROM table1;

-- Copy structure including clustering configuration
CREATE TABLE table3 LIKE table1;

-- Multiple clustering columns
CREATE TABLE table1 (
  col0 STRING,
  col1 DATE,
  col2 BIGINT
)
CLUSTER BY (col0, col1);
```

### On Existing Tables
```sql
ALTER TABLE <table_name>
CLUSTER BY (<clustering_columns>);
```

---

## 🔄 Convert Partitioned Tables to Liquid Clustering  
*(Databricks Runtime 18.1 and above)*

```sql
ALTER TABLE <table_name>
REPLACE PARTITIONED BY WITH CLUSTER BY [(<clustering_columns>) | AUTO];
```

Options:
- `( <clustering_columns> )` → Specify new clustering columns  
- `AUTO` → Use current partition columns and let predictive optimization adapt  
- No options → Keep current partition columns as clustering columns  

---

## 📌 Examples

```sql
-- Convert partitioned table to cluster by different columns
ALTER TABLE t1 REPLACE PARTITIONED BY WITH CLUSTER BY (day, id);
OPTIMIZE t1;

-- Use automatic liquid clustering
ALTER TABLE t2 REPLACE PARTITIONED BY WITH CLUSTER BY AUTO;

-- Keep current partition columns as clustering columns
ALTER TABLE t3 REPLACE PARTITIONED BY WITH CLUSTER BY;
```

---

## ✅ Benefits
- Performance improvements for tables suffering from poor data-skipping or over-partitioning  
- Automatic optimization with `CLUSTER BY AUTO` for changing query patterns  
- Flexible clustering columns (easy to alter vs rigid partitions)  
- Reduced write conflicts with **row-level concurrency** 

# Lakeflow Declarative Pipelines

https://docs.databricks.com/aws/en/ldp/expectation-patterns?language=Python%C2%A0Module
https://docs.databricks.com/aws/en/ldp/expectations?language=Python

```python
rules = {
    "valid_check_in": "(check_in IS NOT NULL)",
    "valid_check_out": "(check_out IS NOT NULL)",
}
quarantine_rules = "NOT({0})".format(" AND ".join(rules.values()))
 
@dlt.table(partition_cols=["is_quarantined"])
@dlt.expect_all(rules)
def silver_reservations():
return (
    spark.readStream.table("bronze_reservations")
                     .withColumn("is_quarantined", expr(quarantine_rules))
)
```

This function streams all rows from the bronze_reservations table into the silver_reservations table, adds a new Boolean column called is_quarantined to flag records with missing check-in or check-out dates, and partitions the table by that flag.


The dlt.expect_all(rules) decorator applies data quality expectations but does not drop invalid rows; it simply records the validation results for monitoring purposes. As a result, both valid and invalid records are retained in the same table, making it easy to trace and fix data quality issues without losing information.

This design is a common pattern in Lakeflow Declarative Pipelines for managing data quality. Instead of discarding bad data outright, teams often quarantine it within the same dataset by tagging and partitioning. This allows for continuous ingestion and validation of streaming data while supporting later review or remediation of problematic records, ensuring both data reliability and auditability in production data pipelines.

Note: Databricks has recenlty open-sourced this solution, integrating it into the Apache Spark ecosystem under the name Spark Declarative Pipelines (SDP).

## ✅ Using Expectations in ETL Pipelines

Expectations apply **quality constraints** that validate data as it flows through ETL pipelines.  
They provide greater insight into **data quality metrics** and allow you to **fail updates** or **drop records** when detecting invalid records.

---

## 📌 What Are Expectations?
- Expectations are **optional clauses** in pipeline materialized view, streaming table, or view creation statements.  
- They apply **data quality checks** on each record passing through a query.  
- Expectations use **standard SQL Boolean statements** to specify constraints.  
- You can combine multiple expectations for a single dataset and set expectations across all dataset declarations in a pipeline.

---

## 🏷️ Expectation Name
- Each expectation must have a **name**, used as an identifier to track and monitor the expectation.  
- Choose a name that clearly communicates the metric being validated.  

**Example:**
```sql
-- Validate that customer age is between 0 and 120
CONSTRAINT valid_customer_age CHECK (age BETWEEN 0 AND 120);
```

## 🔎 Constraint to Evaluate
- The **constraint clause** is a SQL conditional statement that evaluates to **true or false** for each record.  
- The constraint contains the actual logic for what is being validated.  
- When a record fails this condition, the expectation is triggered.

---

## 🚫 Constraints Cannot Contain
- Custom Python functions  
- External service calls  
- Subqueries referencing other tables

```python
@dp.table
@dp.expect("valid_customer_age", "age BETWEEN 0 AND 120")
def customers():
  return spark.readStream.table("datasets.samples.raw_customers")

```

```python
#The syntax for a constraint in Python is:

Python
@dp.expect(<constraint-name>, <constraint-clause>)

#Multiple constraints can be specified:

Python
@dp.expect(<constraint-name>, <constraint-clause>)
@dp.expect(<constraint2-name>, <constraint2-clause>)

Examples:

Python
# Simple constraint
@dp.expect("non_negative_price", "price >= 0")

# SQL functions
@dp.expect("valid_date", "year(transaction_date) >= 2020")

# CASE statements
@dp.expect("valid_order_status", """
   CASE
     WHEN type = 'ORDER' THEN status IN ('PENDING', 'COMPLETED', 'CANCELLED')
     WHEN type = 'REFUND' THEN status IN ('PENDING', 'APPROVED', 'REJECTED')
     ELSE false
   END
""")

# Multiple constraints
@dp.expect("non_negative_price", "price >= 0")
@dp.expect("valid_purchase_date", "date <= current_date()")

# Complex business logic
@dp.expect(
  "valid_subscription_dates",
  """start_date <= end_date
    AND end_date <= current_date()
    AND start_date >= '2020-01-01'"""
)

# Complex boolean logic
@dp.expect("valid_order_state", """
   (status = 'ACTIVE' AND balance > 0)
   OR (status = 'PENDING' AND created_date > current_date() - INTERVAL 7 DAYS)
""")

```

### ⚠️ Action on Invalid Records

You must specify an **action** to determine what happens when a record fails the validation check.  
The following table describes the available actions:

---

## 📊 Available Actions

| Action | SQL Syntax | Python Syntax | Result |
|--------|------------|---------------|--------|
| **warn (default)** | `EXPECT` | `dp.expect` | Invalid records are written to the target. |
| **drop** | `EXPECT ... ON VIOLATION DROP ROW` | `dp.expect_or_drop` | Invalid records are dropped before data is written to the target. The count of dropped records is logged alongside other dataset metrics. |
| **fail** | `EXPECT ... ON VIOLATION FAIL UPDATE` | `dp.expect_or_fail` | Invalid records prevent the update from succeeding. Manual intervention is required before reprocessing. |

---

### ℹ️ Notes
- **warn** → Default behavior, invalid records are still written.  
- **drop** → Invalid records are removed, metrics log the count.  
- **fail** → Update fails entirely, requiring manual intervention before retry.  


# ⚖️ Handling Invalid Records with Expectations

Expectations allow you to define **actions** for records that fail validation checks.  
You can choose to **retain**, **drop**, or **fail** on invalid records depending on your pipeline requirements.

---

## 🟢 Retain Invalid Records (Default)

Retaining invalid records is the **default behavior**.  
Use the `expect` operator when you want to **keep records** that violate the expectation but still collect metrics on how many records pass or fail.

- Records that violate the expectation are added to the target dataset along with valid records.

**Python Example:**
```python
@dp.expect("valid timestamp", "timestamp > '2012-01-01'")
```
## 🟡 Drop Invalid Records

Use the `expect_or_drop` operator to **prevent further processing** of invalid records.  
Records that violate the expectation are **dropped** before being written to the target dataset.

**Python Example:**
```python
@dp.expect_or_drop("valid_current_page", "current_page_id IS NOT NULL AND current_page_title IS NOT NULL")
```

---

## 🔴 Fail on Invalid Records

When invalid records are **unacceptable**, use the `expect_or_fail` operator to **stop execution immediately**.  
If the operation is a table update, the system atomically **rolls back the transaction**.

**Python Example:**
```python
@dp.expect_or_fail("valid_count", "count > 0")
```

---

### 📌 Summary
- **Retain (`expect`)** → Invalid records are kept, metrics collected.  
- **Drop (`expect_or_drop`)** → Invalid records are removed before writing.  
- **Fail (`expect_or_fail`)** → Execution stops, transaction rolled back.  

```python
valid_pages = {"valid_count": "count > 0", "valid_current_page": "current_page_id IS NOT NULL AND current_page_title IS NOT NULL"}

@dp.table
@dp.expect_all(valid_pages)
def raw_data():
  # Create a raw dataset

@dp.table
@dp.expect_all_or_drop(valid_pages)
def prepared_data():
  # Create a cleaned and prepared dataset

@dp.table
@dp.expect_all_or_fail(valid_pages)
def customer_facing_data():
  # Create cleaned and prepared to share the dataset
  ```

  ```python
  from pyspark import pipelines as dp
from rules_module import *
from pyspark.sql.functions import expr, col

def get_rules(tag):
  """
    loads data quality rules from a table
    :param tag: tag to match
    :return: dictionary of rules that matched the tag
  """
  return {
    row['name']: row['constraint']
    for row in get_rules_as_list_of_dict()
    if row['tag'] == tag
  }

@dp.table
@dp.expect_all_or_drop(get_rules('validity'))
def raw_farmers_market():
  return (
    spark.read.format('csv').option("header", "true")
      .load('/databricks-datasets/data.gov/farmers_markets_geographic_data/data-001/')
  )

@dp.table
@dp.expect_all_or_drop(get_rules('maintained'))
def organic_farmers_market():
  return (
    spark.read.table("raw_farmers_market")
      .filter(expr("Organic = 'Y'"))
  )
  ```

## Row count validation

```python
@dp.materialized_view(
  name="count_verification",
  comment="Validates equal row counts between tables"
)
@dp.expect_or_fail("no_rows_dropped", "a_count == b_count")
def validate_row_counts():
  return spark.sql("""
    SELECT * FROM
      (SELECT COUNT(*) AS a_count FROM table_a),
      (SELECT COUNT(*) AS b_count FROM table_b)""")

```

## Missing record detection
```python
@dp.materialized_view(
  name="report_compare_tests",
  comment="Validates no records are missing after joining"
)
@dp.expect_or_fail("no_missing_records", "r_key IS NOT NULL")
def validate_report_completeness():
  return (
    spark.read.table("validation_copy").alias("v")
      .join(
        spark.read.table("report").alias("r"),
        on="key",
        how="left_outer"
      )
      .select(
        "v.*",
        "r.key as r_key"
      )
  )
```

## Primary key uniqueness

```python
@dp.materialized_view(
  name="report_pk_tests",
  comment="Validates primary key uniqueness"
)
@dp.expect_or_fail("unique_pk", "num_entries = 1")
def validate_pk_uniqueness():
  return (
    spark.read.table("report")
      .groupBy("pk")
      .count()
      .withColumnRenamed("count", "num_entries")
  )
```

## Schema evolution pattern
```python
@dp.table
@dp.expect_all_or_fail({
  "required_columns": "col1 IS NOT NULL AND col2 IS NOT NULL",
  "valid_col3": "CASE WHEN col3 IS NOT NULL THEN col3 > 0 ELSE TRUE END"
})
def evolving_table():
  # Legacy data (V1 schema)
  legacy_data = spark.read.table("legacy_source")

  # New data (V2 schema)
  new_data = spark.read.table("new_source")

  # Combine both sources
  return legacy_data.unionByName(new_data, allowMissingColumns=True)
```

## Range-based validation pattern

```python
@dp.view
def stats_validation_view():
  # Calculate statistical bounds from historical data
  bounds = spark.sql("""
    SELECT
      avg(amount) - 3 * stddev(amount) as lower_bound,
      avg(amount) + 3 * stddev(amount) as upper_bound
    FROM historical_stats
    WHERE
      date >= CURRENT_DATE() - INTERVAL 30 DAYS
  """)

  # Join with new data and apply bounds
  return spark.read.table("new_data").crossJoin(bounds)

@dp.table
@dp.expect_or_drop(
  "within_statistical_range",
  "amount BETWEEN lower_bound AND upper_bound"
)
def validated_amounts():
  return spark.read.table("stats_validation_view")
```

## Quarantine invalid records

```python
from pyspark import pipelines as dp
from pyspark.sql.functions import expr

rules = {
  "valid_pickup_zip": "(pickup_zip IS NOT NULL)",
  "valid_dropoff_zip": "(dropoff_zip IS NOT NULL)",
}
quarantine_rules = "NOT({0})".format(" AND ".join(rules.values()))

@dp.view
def raw_trips_data():
  return spark.readStream.table("samples.nyctaxi.trips")

@dp.table(
  temporary=True,
  partition_cols=["is_quarantined"],
)
@dp.expect_all(rules)
def trips_data_quarantine():
  return (
    spark.readStream.table("raw_trips_data").withColumn("is_quarantined", expr(quarantine_rules))
  )

@dp.view
def valid_trips_data():
  return spark.read.table("trips_data_quarantine").filter("is_quarantined=false")

@dp.view
def invalid_trips_data():
  return spark.read.table("trips_data_quarantine").filter("is_quarantined=true")

```

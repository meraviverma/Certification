%md
# 📘 Exam Prep – Databricks SQL & Delta Lake Concepts

---

## ❓ Question 1 – Setting Tags
**Which of the following is NOT a correct command to set tags on a table in Databricks?**

- ✅ `SET TAG ON TABLE table_name tag_key;`  
- ✅ `SET TAG ON TABLE table_name tag_key = tag_value;`  
- ✅ `ALTER TABLE table_name SET TAGS ('tag_key1' = 'tag_value1', 'tag_key2' = 'tag_value2');`  
- ❌ `ALTER TABLE table_name SET TAG tag_key = tag_value;` ← **Incorrect**

**Explanation:**  
Databricks SQL supports two forms:  
- **Plural form:** `ALTER TABLE ... SET TAGS (...)`  
- **Singular form:** `SET TAG ON TABLE ...`  
The syntax `ALTER TABLE ... SET TAG` is invalid.

---

## ❓ Question 2 – Task Types
**Which of the following is NOT a valid task type in Databricks Jobs?**

- ✅ Python wheel  
- ✅ SQL query  
- ✅ If/else condition  
- ❌ REST API call ← **Incorrect**

**Explanation:**  
Databricks Jobs support task types like **Python wheel, SQL query, If/else condition**, but **REST API call** is not a valid task type.

---

Here’s a properly formatted **Databricks Markdown (`%md`) block** for the scenarios you shared, keeping all the content intact but structured for clarity:


## ❓ Question 3 – CTAS & Dropping Tables
A Delta Lake table was created with:
```sql
CREATE TABLE target AS SELECT * FROM source;
```

A data engineer runs:
```sql
DROP TABLE source;
```

### ✅ Correct Answer
**Only the source table will be dropped, while the target table will not be affected.**

### ❌ Incorrect Options
- Both target and source tables will be dropped  
- An error will occur indicating dependencies  
- Target table will no longer be queryable  

### 📌 Explanation
- CTAS (`CREATE TABLE AS SELECT`) creates a **new independent Delta table**.  
- Dropping the source does not affect the target.  

Reference: [Databricks CTAS Syntax](https://docs.databricks.com/sql/language-manual/sql-ref-syntax-ddl-create-table-using.html)

---

## ❓ Question 4 – Syncing Clones
A Delta Lake table `orders_archive` was created with:
```sql
CREATE TABLE orders_archive DEEP CLONE orders;
```

The engineer wants to sync new changes from `orders` to `orders_archive`.

### ✅ Correct Answer
```sql
CREATE OR REPLACE TABLE orders_archive DEEP CLONE orders;
```

### ❌ Incorrect Options
- `SYNC orders_archive`  
- `REFRESH orders_archive`  
- `INSERT OVERWRITE orders_archive SELECT * FROM orders`  

### 📌 Explanation
- Cloning can occur incrementally.  
- Running `CREATE OR REPLACE TABLE ... DEEP CLONE` re-syncs the clone with the source.  
- `DESCRIBE HISTORY orders_archive` will show a new CLONE version.  

Reference: [Databricks Clone Documentation](https://docs.databricks.com/delta/clone.html)

---

## ❓ Question 5 – Automatic Liquid Clustering
A team is considering **partitioning, Z-ordering, and Liquid Clustering** for a managed Delta table in Unity Catalog.

**Which scenario best indicates that Automatic Liquid Clustering is recommended?**

### ✅ Correct Answer
**The table experiences diverse, frequently changing query filters across multiple columns, with unpredictable access patterns.**

### ❌ Incorrect Options
- Stable clustering keys identified  
- Heavy filtering by consistent date ranges  
- Not applicable to managed tables  

### 📌 Explanation
- **Partitioning** → Best for stable, predictable filters (e.g., date/time).  
- **Z-ordering** → Best for known high-cardinality columns with consistent filters.  
- **Automatic Liquid Clustering** → Best for **unpredictable, varied query patterns**.  

---

# 📌 Key Takeaways
- CTAS creates independent tables; dropping the source does not affect the target.  
- Deep clones can be re-synced with `CREATE OR REPLACE TABLE ... DEEP CLONE`.  
- Automatic Liquid Clustering is optimal for **unpredictable query filters** across multiple columns.  

# Query Profile

A **query profile** helps you visualize the details of a query execution and troubleshoot performance bottlenecks.

### 🔍 What it shows
- Each query operator (scan, filter, join, aggregation, etc.)
- Metrics such as:
  - Execution time
  - Number of rows processed
  - Memory consumption

### ⚡ Why it’s useful
- Identify the slowest part of a query execution at a glance.
- Assess the impact of query modifications.
- Detect and fix common mistakes in SQL statements:
  - Exploding joins
  - Full table scans
  - Excessive shuffles

### ✅ Key takeaway
Use query profiles to continuously monitor and optimize SQL performance, ensuring efficient and reliable pipelines.


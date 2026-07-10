Topic:
1) Introduction
2) Storing Data Securely
3) Regulatory Compliance
4) Data Privacy
5) Unity Catalog
    - Key Concepts and Components
    - Audit Your Data


# Introduction
Ranging from secure data storage and Unity Catalog to protecting PII and working with streaming data in a hands-on lab.

- Store sensitive data appropriately to simplify granting access and processing deletes.
- Perform data masking and configure fine-grained access control to configure appropriate privileges to sensitive data.
- Process deletions to ensure compliance with the right to be forgotten.

# 📘 Module: Storing Data Securely

As data regulations continue to evolve, organizations need clear strategies to ensure compliance and protect sensitive information.  
This module introduces the fundamentals of compliance and data privacy, highlighting key regulations and practical approaches for managing risk in data-driven environments.  
We’ll cover the importance of regulatory compliance, explore common frameworks, and examine how platforms like **Dare** can help streamline compliance efforts across the data lifecycle.

---

## 🎯 Learning Outcomes

By the end of this module, you will:

- ✅ **Understand** why compliance and data privacy are essential for responsible data management.  
- ✅ **Identify** key regulations relevant to data teams, such as **GDPR** and **CCPA**.  
- ✅ **Learn** strategies for maintaining compliance, including **data classification** and **access controls**.  
- ✅ **Explore** how **Dare** supports compliance workflows across the data lifecycle.  
- ✅ **Recognize** the role of compliance in building **trust** and mitigating **organizational risk**.  

---

## 🌐 Key Focus Areas
- Importance of regulatory compliance in modern data ecosystems  
- Overview of common frameworks (GDPR, CCPA, HIPAA, etc.)  
- Practical approaches: classification, access controls, monitoring  
- Dare’s role in automating compliance workflows  
- Building trust through responsible data management  

---

✨ *This module sets the foundation for responsible, secure, and compliant data practices in any organization.*

# 🔐 Regulatory Compliance

Regulatory compliance regarding the secure storage and management of Personally Identifiable Information (PII) and other sensitive data is largely guided by two primary frameworks:  
- **General Data Protection Regulation (GDPR)** in the EU  
- **California Consumer Privacy Act (CCPA)** in the US  

Since the specifics of these laws differ slightly, companies doing business in both regions are advised to define a **single global policy** that satisfies both to simplify data management.

---

## 📜 Core Compliance Requirements
To maintain regulatory compliance, organizations must implement policies to:
- 📢 **Inform customers** about what personal information is being collected.  
- 🔍 **Quickly identify data** associated with a given user.  
- ✏️ **Update, delete, or export** a user's personal information upon request.  
- ⏱️ **Process requests in a timely fashion** to avoid regulatory penalties.  

---

## ⏳ Timelines and Penalties
Failing an audit or improperly handling user data can result in significant financial consequences:

- **GDPR**  
  - Response required within **30 days**.  
  - Penalties: up to **4% of annual revenue** or **€20 million**, whichever is greater.  

- **CCPA**  
  - Confirm receipt within **10 business days**.  
  - Process requests within **45 days**.  
  - Fines: up to **$2,500 per violation** and **$750 per consumer per incident**.  

---

## ⚙️ How Databricks Simplifies Compliance
The **Databricks Data Intelligence Platform** and **Delta Lake architecture** provide built-in tools to help organizations avoid compliance headaches:

- 📉 **Reduced Data Redundancy**: Lakehouse architecture minimizes multiple copies of PII across systems.  
- ✅ **Data Quality Enforcement**: Schema enforcement prevents overlooked PII due to mismatches or errors.  
- 🔄 **Reliable Data Modification**: Delta Lake ensures transactional guarantees for updates/deletions.  
- 🚀 **Built-in Optimizations**: Z-order indexing speeds up locating PII; `VACUUM` manages obsolete data.  
- 📑 **Auditing Capabilities**: Delta transaction logs enable reliable audits of processing and deletion requests.  

---

✨ *Databricks empowers organizations to meet compliance requirements efficiently while maintaining trust and reducing risk.*

# 🔎 Data Privacy: Identify, Protect, and Manage

When managing data privacy, organizations must consider three key aspects to evaluate their current state, secure sensitive information, and handle user rights: **Identify**, **Protect**, and **Manage**.  

---

## 1️⃣ Identify PII
The first step is to establish your current state by understanding what data you have, where its source is located, and how accurately you can detect data that needs protection.  

**Key Practices:**
- 🗂️ **Data Discovery:** Conduct a comprehensive inventory to categorize, label, and locate sensitive information across the organization.  
- 🏷️ **Data Classification:** Group discovered data by sensitivity level (PII, financial data, health records) to ensure correct compliance protocols.  
- 🗺️ **Data Mapping:** Create a clear picture of how data flows through the organization, where it is stored, and who has access to it.  

---

## 2️⃣ Protect PII
The second step requires assessing your security options. Organizations must decide whether to **do nothing**, **anonymize**, or **pseudonymize** their data. This requires balancing security with utility.  

**Key Practices:**
- 🔐 **Data Security Measures:** Implement safeguards such as access controls, encryption, and firewalls to prevent unauthorized access.  
- 📉 **Data Handling:** Practice data minimization by limiting collection strictly to what is necessary for business purposes.  

---

## 3️⃣ Manage PII
The final aspect revolves around how you search across your data and efficiently apply actions to it. Regulations like **GDPR** and **CCPA** grant users rights such as the "right to be forgotten," rectification, and restriction of processing.  

**Key Practices:**
- 📑 **Data Governance:** Establish policies and procedures for handling personal data throughout its lifecycle.  
- ⚖️ **Compliance Management:** Ensure strict adherence to GDPR, CCPA, HIPAA, and other data protection regulations.  
- 📊 **Monitoring & Auditing:** Regularly assess and update privacy practices to address evolving regulatory requirements and security threats.  

---

✨ *By following the Identify → Protect → Manage framework, organizations can build trust, reduce risk, and ensure responsible data stewardship.*

# Unity Catalog
we’ll explore the core concepts of using Unity Catalog for data governance, including dynamic views, row filters, and column masking for access control. We’ll also cover auditing strategies, evaluate the security model, and conclude with a demonstration of Unity Catalog's key features.

- Understand the fundamental concepts of using Unity Catalog for data governance.
- Learn how to implement dynamic views, row filters, and column masking to manage data access at the row and column levels.
- Explore various auditing approaches to track and ensure data security.
- Evaluate Unity Catalog's security model and data isolation features to implement effective data governance strategies.

# Key Concepts and Components

To address the complexities of managing data privacy, security, and regulatory compliance, Databricks uses **Unity Catalog** as the core governance layer for all data and AI assets.  
Historically, organizations struggled with fragmented governance:  
- Data lakes offered only coarse, file-level permissions.  
- Data warehouses and ML tools used isolated, incompatible security models.  

Unity Catalog solves this by providing a **unified governance architecture** across the entire data lifecycle.

---

## 1️⃣ The Data Security Model
The foundation of Databricks governance relies on **Access Control Lists (ACLs)**, which define *who* can access *what* and *how*.  

- 👤 **Principals ("Who")**: Users, service principals, or groups.  
  *Best practice*: Grant permissions at the **group level** for maintainability.  
- 📦 **Securables ("What")**: Assets such as metastores, catalogs, schemas, tables, views, volumes, ML models, and functions.  
- 🔑 **Privileges ("How")**: Permissions like `SELECT`, `MODIFY`, `EXECUTE`, `CREATE TABLE`, `USE CATALOG`.  

**Management Options:**  
- Catalog Explorer UI  
- SQL (`GRANT` / `REVOKE`) in notebooks  
- Databricks CLI, REST APIs, or Terraform  

---

## 2️⃣ Fine-Grained Access Control
Sensitive data (e.g., PII) requires controls beyond table-level access. Unity Catalog supports:

- 🪞 **Dynamic Views**: Restrict access by filtering or redacting data via SQL functions.  
- 🧩 **Row-Level Security & Column Masking**: Attach UDFs with `SET ROW FILTER` or `SET MASK` to enforce group-based restrictions dynamically.  

---

## 3️⃣ Automated Data Lineage
Unity Catalog auto-captures **runtime data lineage**, tracking flows across:  
- Tables, columns, dashboards, jobs, notebooks, ML models  

📊 *Benefit*: Enables instant answers to "Where did this data come from?" and supports compliance audits by showing downstream impacts of pipeline changes.  

---

## 4️⃣ Data Discovery & Tagging
Unity Catalog simplifies the **Identify** phase of privacy management:  

- 🔍 **Unified Search**: Users only discover assets they have permission to access.  
- 🤖 **AI-Generated Metadata**: Auto-create table/column comments for clarity.  
- 🏷️ **Tagging Best Practices**: Mark assets (e.g., "PII", "Sensitive") at ingestion to enable tag-based security policies and easier discovery.  

---

✨ *Unity Catalog provides a single, unified governance layer—ensuring secure, compliant, and efficient management of all data and AI assets.*

# 🔐 Access Control Lists (ACLs) in Databricks

In Databricks, **Access Control Lists (ACLs)** are the core mechanism used to control exactly *who* can access specific data objects and *what* actions they can perform.  
The ACL model is built around three fundamental concepts: **Principals ("Who")**, **Securables ("What")**, and **Privileges ("How")**.

---

## 1️⃣ Principals (The "Who")
A principal is the entity being granted or denied access. In Databricks, a principal can be:
- 👤 **User** – an individual account.  
- ⚙️ **Service Principal** – typically used for automated tools or applications.  
- 👥 **Group** – a collection of users or service principals.  

💡 *Best Practice*: Manage permissions at the **group level** for scalability and maintainability.

---

## 2️⃣ Securables (The "What")
Securables are the specific data or AI assets being protected. Unity Catalog supports a wide range of securables:

- 📊 **Data Objects**: Catalogs, schemas (databases), tables, views, volumes  
- 🤖 **AI & Compute Assets**: Machine learning models, functions  
- 🗄️ **Storage & Federation**: Metastores, external locations, storage credentials, query federation, foreign catalog connections  
- 🔗 **Sharing**: Delta Sharing recipients  

---

## 3️⃣ Privileges (The "How")
Privileges define the specific actions a principal can perform on a securable object.  

**Examples:**
- `SELECT` – read/query data  
- `MODIFY` – update or delete data  
- `EXECUTE` – run functions or models  
- `CREATE TABLE` – create new tables  
- `USE CATALOG` / `USE SCHEMA` – navigate and use catalogs or schemas  

⚠️ *Note*: Not all privileges apply to all objects. For example, `MODIFY` on a view is meaningless since views are read-only.

---

## ⚙️ Managing ACLs
Databricks offers multiple ways to manage ACLs depending on workflow:

- 📝 **SQL Commands**: Use `GRANT` and `REVOKE` directly in notebooks or Databricks SQL.  
  *Example*:  
  ```sql
  GRANT SELECT ON TABLE t TO analysts;

# 📊 Audit Your Data Unity Catalog System Tables

To effectively monitor and manage the **security, privacy, and cost** of your data environment, Databricks Unity Catalog includes extensive **system tables**.  
Located in the **`system` catalog**, these tables make your lakehouse more transparent and allow you to perform analytics on your own operational data.

---

## 🔎 Key Operational Insights

### 1️⃣ Object Metadata
- Query the **`information_schema`** to track ownership, inventory, tags, and data access.  
- Quickly answer questions like:  
  - What tables exist in a specific catalog?  
  - Who owns a particular gold table?  
  - Who last updated it?  
  - Who currently has access privileges?  

---

### 2️⃣ Billing Logs
- Found in the **`billing` schema** within the **`Usage` table**.  
- Helps you understand **cost allocation** across your data estate.  
- Track:  
  - Daily trend of **Databricks Unit (DBU)** consumption  
  - DBU usage by SKU for the month  
  - Top 10 users or jobs consuming the most DBUs  

---

### 3️⃣ Audit Logs
- Located in the **`access` schema** under the **`audit` table**.  
- Provide near-real-time visibility into **who accessed what, and when**.  
- Use cases:  
  - Investigate which tables are accessed most frequently  
  - See what a specific user viewed in the last 24 hours  
  - Identify who deleted a specific table  

⚠️ *Note: Audit logs are currently in Public Preview. Check with your workspace administrator to confirm availability.*  

---

### 4️⃣ Lineage Data
- Available in the **`table_lineage` table** (within the `access` schema).  
- Enables queries on **upstream and downstream data flows** at both table and column levels.  
- Benefits:  
  - Trace exactly what tables are sourced from a given table  
  - Identify which user queries are reading from it  
  - Comprehensively track data lineage for compliance and auditing  

⚠️ *Note: Lineage data is also in Preview mode.*  

---

✨ *System tables empower organizations with transparency, enabling proactive monitoring of compliance, security, and cost across the Databricks lakehouse.*

# 📊 Example Queries with Unity Catalog System Tables

Databricks **system tables** enable powerful analytics on operational data.  
Here are examples of the types of questions you can answer using each category:

---

## 1️⃣ Object Metadata (`information_schema`)
Understand the state, ownership, and access levels of catalog objects.

- 📦 **Inventory**:  
  *Query*: `SELECT table_name FROM system.information_schema.tables WHERE table_catalog="sales";`  
  *Question*: "What tables are in the sales catalog?"  

- 📝 **Activity**:  
  *Query*: `SELECT table_name, last_altered_by, last_altered FROM system.information_schema.tables WHERE table_schema = "churn_gold" ORDER BY 1, 3 DESC;`  
  *Question*: "Who last updated the gold tables and when?"  

- 🔐 **Security**:  
  *Query*: `SELECT grantee, table_name, privilege_type FROM system.information_schema.table_privileges WHERE table_name = "login_data_silver";`  
  *Question*: "Who has access to this table?"  

- 👤 **Ownership**:  
  *Query*: `SELECT table_owner FROM system.information_schema.tables WHERE table_catalog = "retail_prod" AND table_schema = "churn_gold" AND table_name = "churn_features";`  
  *Question*: "Who owns this gold table?"  

---

## 2️⃣ Billing Logs (`billing` schema → `Usage` table`)
Track and allocate costs across your data estate.
---

### 1️⃣ Daily Trend in DBU Consumption
```sql
SELECT usage_date AS `Date`, 
       SUM(usage_quantity) AS `DBUs Consumed`
FROM system.billing.usage
GROUP BY usage_date
ORDER BY usage_date ASC;
```

---

### 2️⃣ DBUs by SKU (Current Month)
```sql
SELECT sku_name AS `SKU`, 
       SUM(usage_quantity) AS `DBUs`
FROM system.billing.usage
WHERE month(usage_date) = month(CURRENT_DATE)
GROUP BY sku_name
ORDER BY `DBUs` DESC;
```

---

### 3️⃣ Top 10 Users by DBU Consumption
```sql
SELECT identity_metadata.run_as AS `User`, 
       SUM(usage_quantity) AS `DBUs`
FROM system.billing.usage
GROUP BY identity_metadata.run_as
ORDER BY `DBUs` DESC
LIMIT 10;
```

---

### 4️⃣ Jobs Consuming the Most DBUs
```sql
SELECT usage_metadata.job_id AS `Job ID`, 
       SUM(usage_quantity) AS `DBUs`
FROM system.billing.usage
GROUP BY usage_metadata.job_id
ORDER BY `DBUs` DESC;
```
 

---

## 3️⃣ Audit Logs (`access` schema → `audit` table`)
Gain near-real-time visibility into user actions for security and compliance.

- 🔎 "Who accesses this table the most?"  
- 👤 "What tables does this user access most frequently?"  
- ⏱️ "What has this user accessed in the last 24 hours?"  
- ❌ "Who deleted this table?"  

Databricks **audit logs** (located in `system.access.audit`) provide near-real-time visibility into user actions for **security** and **compliance**.  
Here are examples of useful queries:

---

### 1️⃣ Who Accesses This Table the Most?
```sql
SELECT user_identity.email, COUNT(*) AS access_count
FROM system.access.audit
WHERE request_params.table_full_name = "main.uc_deep_dive.login_data_silver"
  AND service_name = "unityCatalog"
  AND action_name = "generateTemporaryTableCredential"
GROUP BY user_identity.email
ORDER BY access_count DESC
LIMIT 1;
```

---

### 2️⃣ What Has This User Accessed in the Last 24 Hours?
```sql
SELECT request_params.table_full_name
FROM system.access.audit
WHERE user_identity.email = "ifi.derekli@databricks.com"
  AND service_name = "unityCatalog"
  AND action_name = "generateTemporaryTableCredential"
  AND datediff(now(), event_time) < 1;
```

---

### 3️⃣ Who Deleted This Table?
```sql
SELECT user_identity.email
FROM system.access.audit
WHERE request_params.full_name_arg = "main.uc_deep_dive.login_data_silver"
  AND service_name = "unityCatalog"
  AND action_name = "deleteTable";
```

---

### 4️⃣ What Tables Does This User Access Most Frequently?
```sql
SELECT request_params.table_full_name, COUNT(*) AS access_count
FROM system.access.audit
WHERE user_identity.email = "ifi.derekli@databricks.com"
  AND service_name = "unityCatalog"
  AND action_name = "generateTemporaryTableCredential"
GROUP BY request_params.table_full_name
ORDER BY access_count DESC
LIMIT 1;
```

---

## 4️⃣ Lineage Data (`access` schema → `table_lineage` table`)
Trace upstream and downstream dependencies for your data sources.

- 🔗 "What tables are sourced from this table?"  
- 👥 "What user queries read from this table?"  


✨ *System tables empower you to monitor metadata, costs, security, and lineage—making your lakehouse transparent and auditable.*


## 1️⃣ What Tables Are Sourced from This Table?
```sql
SELECT DISTINCT target_table_full_name
FROM system.access.table_lineage
WHERE source_table_name = "login_data_bronze";
```

📊 *Purpose*: Identify all downstream tables that are derived from `login_data_bronze`.

---

## 2️⃣ What User Queries Read from This Table?
```sql
SELECT DISTINCT entity_type, entity_id, source_table_full_name
FROM system.access.table_lineage
WHERE source_table_name = "login_data_silver";
```

📊 *Purpose*: Track which user queries or entities are reading from `login_data_silver`.

---

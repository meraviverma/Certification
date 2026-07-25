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

# 🔒 Data Isolation in Databricks Unity Catalog

In **Databricks Unity Catalog**, data isolation is managed through a hierarchical structure that allows organizations to physically and logically separate their data assets based on **regional, organizational, or environmental needs**.

---

## 📂 The Primary Unit: Catalogs
While the **metastore** is the top-level container, **data isolation should begin at the catalog level**.

- **Organizational Mirroring:** Catalogs often reflect organizational units or SDLC scopes (e.g., `Development` vs. `Production`).
- **Separation of Storage:** Catalogs can be stored separately from the parent metastore for stronger isolation.
- **Workspace Binding:** Catalogs can be bound to specific workspaces, ensuring sensitive data is processed only in designated environments.
- **Inherited Permissions:** Ideal for setting inherited permissions, enabling efficient and granular access control across schemas and tables.

---

## 🌍 Regional Isolation: Metastores
Metastores provide a higher level of **regional isolation**.

- **One per Region:** Account admins create one metastore per region, assigned to multiple workspaces in that region.
- **Physical Separation:** Each metastore’s storage is physically separated by default.
- **Not the Main Unit:** Metastores define regional boundaries, but **catalogs** are the primary unit for isolating data between teams/projects.

---

## 🗄️ Storage-Level Isolation
Unity Catalog supports physical separation of data via **storage configuration**.

- **Flexible Configuration:** Storage can be defined at the metastore, catalog, or schema level.
- **External Locations:** Bind external storage locations to specific workspaces for enhanced isolation and auditability.
- **Access Best Practices:** Prevent direct user access to cloud storage containers to avoid bypassing Unity Catalog’s access controls.

---

## ⚙️ Workload & Workspace Isolation
Isolation can also be enforced at the **compute and environment level**.

- **Single-User Clusters:** Provide isolation for individual users (often used for POCs).
- **Multi-User Clusters & SQL Warehouses:** Safely support users with different privilege levels via shared access modes.
- **Multiple Workspaces:** Isolate groups of users who are not intended to collaborate.

---

## 🛡️ Governance Models
Isolation strategies depend on the chosen governance model:

- **Centralized Governance:** A central admin group owns the metastore and manages permissions across all objects.
- **Distributed Governance:** Catalogs act as **data domains**. Domain owners manage governance independently from other domains.

---

✅ **Key Takeaway:**  
Use **catalogs as the primary isolation unit**, metastores for **regional boundaries**, and storage/workspace configurations for **physical and workload separation**. Align governance (centralized vs. distributed) with organizational maturity and domain ownership.

## **Matastore**
In Databricks Unity Catalog, a **metastore** is the **top-level container for metadata** and the foundation of the data hierarchy. It is used to manage data assets—such as tables, views, and volumes—and the permissions that govern access to them.

Key characteristics of a metastore include:

*   **Hierarchical Organization:** It organizes data objects using a **three-level namespace** (catalog > schema > table/view/volume).
*   **Regional Management:** Account administrators create **one metastore per region**, which can then be assigned to multiple workspaces within that same region.
*   **Regional Isolation:** Metastores provide regional isolation by default, as the physical storage for each metastore is typically separated from others within the same account.
*   **Data Isolation Limitations:** While they offer regional boundaries, metastores are **not intended to be the primary units of data isolation**; instead, data isolation should begin at the **catalog level**.
*   **Governance and Storage:**
    *   In a **centralized governance model**, metastore owners manage permissions across all objects.
    *   For **managed tables**, the cloud storage is directly associated with the metastore.
    *   It is a best practice to set a **group** as the metastore administrator.
*   **Migration from Legacy Systems:** Databricks recommends migrating away from legacy **Hive metastores**, as they are considered less secure than the governance provided by Unity Catalog.

## 📂 Catalog: Primary Unit of Data Isolation

In **Databricks Unity Catalog**, a **catalog** is the first level of the three-level namespace  
(**catalog → schema → table/view/volume**) used to organize data assets.  
It is specifically **intended as the primary unit of data isolation** within an organization.

---

## 🔑 Key Functions and Characteristics

- **Organizational & SDLC Mirroring**  
  Catalogs often mirror organizational units or software development lifecycle stages,  
  such as separate catalogs for **Production** and **Development** data.

- **Data Isolation**  
  Catalogs can be **bound to specific workspaces**, ensuring sensitive data is processed only in designated environments.

- **Storage Management**  
  Best practice is to **store catalogs separately** from the parent metastore for stronger physical separation.  
  Storage locations can be configured directly at the catalog level to meet cloud bucket/account requirements.

- **Inherited Permissions**  
  Catalogs are the **ideal location to set inherited permissions**, enabling efficient and granular access control  
  across all schemas and tables within that catalog.

---

## 🛡️ Governance Models

- **Distributed Governance**  
  A catalog (or set of catalogs) serves as a **data domain**.  
  The owner of that domain can create and manage assets independently from other domains.

- **Ownership Best Practice**  
  Databricks recommends assigning **a group** as the catalog owner rather than an individual user,  
  ensuring maintainable and scalable governance.

---

✅ **Key Takeaway:**  
Catalogs are the **foundation of data isolation** in Unity Catalog, balancing logical organization,  
physical separation, and governance flexibility.

## 📦 Unity Catalog: Volumes for Non-Tabular Data

In **Databricks Unity Catalog**, a **volume** is a securable object used to manage and govern **non-tabular data**.  
It resides within the three-level namespace: **catalog → schema → volume**, alongside tables and views.

---

## 🔑 Key Aspects of Volumes

- **Supported Data Types**  
  Volumes can store **structured, semi-structured, and unstructured** formats.

- **Common Use Cases**  
  Ideal for storing files that are not suitable for tables, such as:  
  - Libraries  
  - Configuration files  
  - Checkpoint folders  

- **Relationship to Tables**  
  Data stored in volumes **cannot be registered as tables** or handled using table-based operations.  
  Instead, volumes provide a governance layer for non-tabular datasets, complementing table governance.

---

## 🗂️ Types of Volumes

- **Managed Volumes**  
  Stored in locations directly managed by Unity Catalog.

- **External Volumes**  
  Registered against directories located in **external locations** (cloud storage paths + storage credentials).

---

## ⚙️ Best Practice

- Databricks recommends **avoiding DBFS** (Databricks File System) in Unity Catalog-enabled workspaces.  
- Use **volumes** instead for storing all unstructured data to ensure proper governance and security.

---

✅ **Key Takeaway:**  
Volumes extend Unity Catalog’s governance to **non-tabular data**, ensuring consistent security and management across all data types.

## 🗄️ Physical Data Separation in Unity Catalog

In **Databricks Unity Catalog**, physically separating data involves configuring specific storage locations  
at different levels of the hierarchy to ensure that certain datasets are stored within designated cloud accounts or buckets.

---

## 🔑 Key Methods & Best Practices

- **Catalog-Level Storage**  
  - Preferred method for isolation: **store catalogs separately** from the parent metastore.  
  - Enables separation by organizational units or SDLC stages (e.g., `Production` vs. `Development`).

- **External Locations & Credentials**  
  - Managed through **external locations** (cloud storage path + storage credential).  
  - Unity Catalog reads/writes data on behalf of users, ensuring strong control and auditability.

- **Workspace Binding**  
  - Bind external locations and credentials to specific **workspaces**.  
  - Ensures sensitive data is only accessible and processed in authorized environments.

- **Hierarchical Configuration**  
  - Storage can be configured at the **metastore, catalog, or schema level**.  
  - Allows granular control, e.g., isolating data within a specific schema from other schemas in the same catalog.

- **Cloud-Level Permissions**  
  - Beyond Databricks ACLs, sensitive data (like PII) should be stored in containers with **restricted cloud-level access**.  
  - Limit direct user access to cloud storage to prevent bypassing Unity Catalog governance.

- **Managed vs. External Volumes**  
  - **Managed Volumes:** Stored in Unity Catalog-managed locations.  
  - **External Volumes:** Registered against external directories (cloud paths + credentials).  
  - Recommended for non-tabular data to ensure proper governance.

---

✅ **Key Takeaway:**  
Physical separation in Unity Catalog is achieved through **catalog-level storage, external locations, workspace binding, and cloud-level permissions**.  
This layered approach ensures sensitive data remains isolated, governed, and secure across organizational boundaries.


# 🔐 Demo: Securing Data with Unity Catalog

In this demo, we provided a comprehensive walkthrough of securing data using **Unity Catalog**,  
covering foundational features such as:

- Namespace management  
- Privilege settings  
- Dynamic views  
- Row filtering  
- Column masking  
- Tagging & discoverability  
- Lineage tracking  
- AI-generated documentation  

You learned how to set up and manage data assets, control access, and protect sensitive information effectively.

---

## 📌 Key Takeaways

- **Namespace Management & Access Controls**  
  Unity Catalog enables secure organization of data assets through effective namespace management and granular access controls.

- **Granular Privilege Settings**  
  Features like dynamic views, row filtering, and column masking provide fine-grained control over data access.

- **Enhanced Data Management**  
  Tagging and discoverability make assets easier to locate, categorize, and manage across teams.

- **Transparency & Compliance**  
  Lineage tracking ensures visibility into data flow, supports integrity, and provides an audit trail for compliance.

- **AI-Generated Documentation**  
  Simplifies asset management, promotes collaboration, and improves understanding across teams.

---

✅ **Conclusion:**  
Unity Catalog provides a **holistic governance framework** that secures both tabular and non-tabular data,  
ensuring compliance, transparency, and collaboration across the organization.

# PII Data Security

## 🔑 Overview
This module introduces **pseudonymization** and **anonymization** techniques, focusing on their implementation in automated ETL (Extract, Transform, Load) processes. It also covers **best practices for handling PII (Personally Identifiable Information)** and reviews common **data protection methods**, highlighting their applications and benefits in safeguarding sensitive information.

---

## 🎯 Learning Objectives
By the end of this module, you will be able to:

- **Understand and implement pseudonymization and anonymization** in automated ETL workflows.  
- **Apply best practices for handling PII data** to ensure privacy and regulatory compliance.  
- **Explore common data protection techniques** (e.g., encryption, masking, tokenization) and evaluate their applications in real-world scenarios.  

# Pseudonymization & Anonymization

# 🔐 Securing Personally Identifiable Information (PII)

To secure PII at the **data modeling level**, organizations typically use two primary approaches: **pseudonymization** and **anonymization**.  
While these techniques reduce visibility and the risk of data exfiltration, they do not eliminate risk entirely, as **re-identification is often possible** with sufficient time and access to additional data.

---

## 1️⃣ Pseudonymization
Pseudonymization replaces PII with artificial identifiers (tokens or hashes) to protect data at the **record level**.  
⚠️ Under regulations like **GDPR**, pseudonymized data is **still considered personal data**.

**Key Characteristic:**  
- ✅ **Reversible** process — authorized users can re-identify data using keys or lookup tables.

**Methods:**
- 🔒 **Hashing:** Apply a function (e.g., SHA) to convert PII into a random string.  
  - Add a **salt** (random string) before hashing to prevent reversal of deterministic hashes.  
  - Use **Databricks Secrets** to securely store salt values.
- 🗝️ **Tokenization:** Convert PII into keys and store original values in a **secure lookup table (token vault)**.  
  - End-user tables contain only tokens, which are smaller and faster to read.

---

## 2️⃣ Anonymization
Anonymization protects **entire datasets** by **irreversibly altering** personal data so subjects cannot be identified directly or indirectly.  
This is often used in **Business Intelligence**, where analysts need trends and aggregations rather than individual records.

**Methods:**
- 🚫 **Data Suppression:** Exclude specific PII columns or remove rows where demographic groups are too small (risk of identification).  
- 🌍 **Generalization:** Remove specificity to provide insights without revealing identities.  
  - **Categorical Generalization:** Replace city names with regions or countries.  
  - **Binning:** Group values into ranges (e.g., 10-year age bands, salary brackets).  
  - **Truncating IP Addresses:** Round to `/24 CIDR` (replace last byte with zero) to generalize location.  
  - **Rounding:** Round numerical data (e.g., nearest 5 or 100) to suppress outliers.

---

## ✅ Summary
- **Pseudonymization** → Record-level, reversible, useful for operational analytics.  
- **Anonymization** → Dataset-level, irreversible, useful for BI and compliance.  
- Both techniques **reduce risk but don’t eliminate it** — strong governance and layered security are essential.

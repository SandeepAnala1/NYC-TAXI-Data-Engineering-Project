
# 1) Data Redundancy in Azure Storage Accounts
- **Definition:** Storing multiple copies of data in different locations or systems.  
- **Purpose:** Ensures data availability, integrity, and protection against failures or disasters.  

---

### Why is Data Redundancy Needed?  
1. **Data Availability:** Keeps data accessible during hardware failures or outages.  
2. **Disaster Recovery:** Restores data after system crashes or regional disasters.  
3. **Fault Tolerance:** Ensures systems function despite component failures.  
4. **Data Integrity:** Protects against accidental deletions or corruption.  
5. **Compliance:** Meets legal or industry data security requirements.  
6. **Load Balancing:** Improves system performance and reduces bottlenecks.  

## Redundancy Options
- **Key Decision:** When creating a storage account in Azure, choosing the appropriate redundancy option is essential.
- **Default Option:** Azure configures data to be stored with **Geo-Redundant Storage (GRS)** by default.
  - GRS replicates data across different geographic regions.
  - Ensures availability and protection against data loss due to regional failures.

## Locally Redundant Storage (LRS)
- **Definition:** A less expensive alternative to GRS.
- **How it works:** 
  - Keeps multiple copies of data within the same data center.
  - Provides redundancy against hardware failures within a single data center.
  - Does not protect against data loss due to a data center-level event.

## Geo-Redundant Storage (GRS)
- Also known as **Geo-replication storage**.
- Replicates data across different geographic regions to ensure higher availability.

## Cost Considerations
- **LRS:** Cheaper option as it does not involve geographic replication.
- **GRS:** More expensive due to replication across regions.

## Data Lake Considerations
- When creating a **Data Lake**:
  - The **data redundancy option** is a required configuration.
  - The **hierarchical namespace** must be enabled to create a data lake (not required for blob storage accounts).

## Practical Application
- **Current Setup:** Using **LRS** for the storage account to reduce costs.
- **Production Environment:** GRS may be a more suitable choice if avoiding data loss due to a regional event is critical.
--------------------------------------------

# 2) Short Notes on Hierarchical Namespace  

- **Feature for Data Lake**: Essential for creating a data lake; without it, the storage account defaults to blob storage.  
- **Blob Storage vs. Data Lake**:  
  - Blob storage: Flat file structure with files in containers.  
  - Data lake: Supports a folder hierarchy (containers → folders → subfolders).  
- **Folder Structure**: Enables organized file storage with containers and nested folders, unlike blob storage.  
- **Data Analysis**: Facilitates easier data analysis and table creation due to structured organization.  
- **Optional Setting**: Must enable hierarchical namespace during storage account creation; not enabled by default.  
- **Practical Example**: Common structure includes containers like **bronze**, **silver**, and **gold**, with folders for specific files in each.
  
-------------------------------------------------

# 3) Recursive File Lookup in Spark

Spark provides the ability to recursively load files from directories and subdirectories into a DataFrame. This is particularly useful when your files are organized in a nested folder structure, and you want to read all files without specifying each folder explicitly.

#### Key Points:

1. **Recursive File Lookup Option**:
   - Use the `recursiveFileLookup` option in Spark.
   - Set it to `true` to enable recursive discovery of files in all subdirectories.

```python
# Reading trip type data
df_trip = spark.read.format('parquet')\
                    .schema(myschema)\
                    .option('header',True)\
                    .option('recursiveFileLookup', True)\
                    .load('abfss://bronze@nyctaxiassnsa.dfs.core.windows.net/trip_2023/')
```
------------------------------------------------------

# 4) **Delta Lake**
- **Definition**: An open-source storage layer on top of data lakes, providing **ACID transactions**, **schema enforcement**, and **data versioning**.
- **Key Features**:
  - Combines the scalability of data lakes with the reliability of traditional databases.
  - Supports **batch** and **streaming** data.
  - Handles **big data challenges** like data quality and consistency.

---

### **Delta Log**
- **Definition**: A transaction log that records every change made to a Delta table.
- **Functionality**:
  - Stores metadata about the Delta table and its versions in JSON format.
  - Helps maintain **atomicity** and **consistency**.
  - Enables **fault tolerance** by keeping a record of all transactions.

---

### **Versioning**
- **Definition**: Delta Lake keeps a **version history** of changes to a Delta table.
- **Key Benefits**:
  - Allows tracking of modifications (insert, delete, update).
  - Supports debugging, auditing, and replicating datasets at specific points in time.

---

### **Time Travel**
- **Definition**: A feature in Delta Lake that allows querying data at any **previous state** by specifying a timestamp or version.
- **Use Cases**:
  - **Debugging**: Access historical data to debug errors.
  - **Auditing**: Retrieve older snapshots for compliance or analysis.
  - **Recovery**: Restore a table to a previous state in case of accidental changes.

--- 

Let me know if you’d like detailed examples or use cases for these concepts!

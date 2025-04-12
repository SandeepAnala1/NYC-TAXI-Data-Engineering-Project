# NYC Taxi Data Engineering Project

## 📌 Project Overview  
In this end-to-end data engineering project, I’ve built a robust pipeline to process NYC taxi data using the medallion architecture. The goal was to ingest raw data from a public API, apply structured transformations, and store the output in a curated, query-friendly format. This solution demonstrates dynamic pipeline creation, PySpark-based transformations, and Delta Lake implementation on Azure.

---

## 🛠️ Tools & Technologies  

- **Azure Data Factory (ADF):** Used for orchestrating and ingesting data from the NYC Taxi API using parameterized pipelines.  
- **Python (PySpark):** Utilized within Databricks for all data transformation logic.  
- **Databricks:** Employed for running Spark jobs and managing data processing in silver and gold layers.  
- **Delta Lake:** Used in the gold layer to enable versioning, ACID transactions, and time travel.  
- **Parquet:** The file format used for storing data in the bronze and silver layers due to its performance advantages.  
- **Azure Data Lake Storage (ADLS) Gen2:** Served as the central storage for all medallion layers.  
- **NYC Taxi Open Data Set:** Primary data source, accessed through HTTP API.  
- **CSV:** Lookup files manually uploaded to the lake (trip type and zone info).  
- **Azure Portal:** Used to provision and manage cloud resources.  
- **Azure Active Directory (Entra ID):** Used to configure secure access via service principals.  
- **Power BI:** Connected to Databricks to visualize curated data.  
- **Spark SQL:** Used within Databricks to query Delta tables.

---

## 🏗️ Architecture  

The project follows the **Medallion Architecture**:
![image](https://github.com/user-attachments/assets/504bc586-0d47-4b93-b72a-82c0d4da2d8d)


- **Bronze Layer (Raw Zone):**  
  - Ingested monthly green taxi trip data from the NYC Taxi website using ADF.  
  - Stored in Parquet format in ADLS.  
  - Included manually uploaded CSV lookup files.

- **Silver Layer (Transformed Zone):**  
  - Cleaned and transformed raw data using PySpark.  
  - Selected relevant columns, casted data types, and added derived fields (e.g., year, month).  
  - Output stored in Parquet format in ADLS.

- **Gold Layer (Curated Zone):**  
  - Transformed data from the silver layer was written in Delta format.  
  - Created Delta tables within Databricks for analytical querying.  
  - Enabled Delta Lake features like versioning and time travel.

- **Serving Layer:**  
  - Connected Power BI to the Databricks SQL endpoint to consume curated Delta tables for visualization.

---

## 🔄 Pipeline Implementation  

### Data Sources:  
- **NYC Taxi Trip Records:** Accessed via HTTP API with monthly Parquet files organized by year and month.  
- **Lookup Files:** Two CSV files (trip_type.csv, taxi_zone_lookup.csv) uploaded manually.

### Data Ingestion (ADF):  
- Created a **dynamic pipeline** using parameters to construct file URLs.  
- Used “For Each” to iterate through months, and “If Condition” to handle different URL formats.  
- Ingested data into the bronze layer in Parquet format.  
- Configured an HTTP linked service and parameterized datasets.

### Data Transformation (Databricks & PySpark):  
- Read raw data and lookup files into DataFrames.  
- Defined a custom schema for better control over data types.  
- Applied transformations including timestamp conversion, column selection, and field derivation.  
- Wrote output to the silver layer in Parquet format.

### Curated Output (Gold Layer):  
- Read data from the silver layer and wrote it as Delta tables.  
- Created a database (`gold`) in Databricks for structured querying.  
- Demonstrated Delta Lake capabilities like version control and time travel.

### Data Consumption (Power BI):  
- Connected Power BI directly to Databricks using the SQL endpoint.  
- Enabled consumption of gold layer tables for visualization.

---

## 📊 Results  

- Built a **fully automated and parameterized pipeline** for ingesting monthly NYC Taxi data.  
- Utilized efficient file formats (Parquet and Delta) to optimize storage and processing.  
- Established a **structured data lake** using bronze, silver, and gold zones.  
- Enabled **data auditing and recovery** through Delta Lake features.  
- Connected the final output to **Power BI** for analytical consumption.

---

## ✅ Conclusion  

This project demonstrates a modern data engineering workflow on Azure—from ingestion to transformation to consumption. The implementation showcases how to:

- Create dynamic and reusable data pipelines in Azure Data Factory.  
- Apply scalable transformations using PySpark in Databricks.  
- Leverage Parquet and Delta Lake for efficient and reliable data storage.  
- Build a secure and modular data lake architecture.  
- Connect curated data to Power BI for business intelligence use cases.

---

## 📌 Future Enhancements  

- We can Integrate additional datasets to enrich analysis.  
- And add automated data quality checks and logging mechanisms.  
- We can also Implement schema evolution and Change Data Feed (CDF) features in Delta Lake.  
- And Extend the Power BI reporting to include advanced dashboarding.

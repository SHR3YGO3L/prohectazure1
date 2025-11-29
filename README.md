# Azure End-to-End Data Engineering Project

## Project Overview

This project demonstrates a comprehensive data engineering pipeline built entirely on the Microsoft Azure cloud platform. It implements a modern data lakehouse strategy using the **Medallion Architecture** (Bronze, Silver, Gold layers) to ingest, process, and serve data for analytical reporting.

The goal of this project is to simulate a real-world enterprise scenario where data is extracted from an external API, processed using distributed computing, and loaded into a data warehouse for business intelligence insights.

## Architecture

The solution follows a linear data flow pipeline:

**Source System** $\rightarrow$ **Ingestion** $\rightarrow$ **Storage (Data Lake)** $\rightarrow$ **Transformation** $\rightarrow$ **Serving** $\rightarrow$ **Visualization**

### The Medallion Architecture
* **Bronze Layer (Raw):** Stores data in its native format as it is ingested from the source system. No transformations are applied here.
* **Silver Layer (Cleansed):** Data is cleaned, filtered, and transformed. This layer serves as the "Single Source of Truth."
* **Gold Layer (Curated):** Data is aggregated and modeled (Star Schema) for specific business reporting needs.

## 🛠️ Tech Stack & Services Used

* **Source:** **GitHub API** (HTTP Source)
* **Orchestration:** **Azure Data Factory (ADF)**
* **Storage:** **Azure Data Lake Storage Gen2 (ADLS Gen2)**
* **Compute/Transformation:** **Azure Databricks** (PySpark)
* **Data Warehouse:** **Azure Synapse Analytics** (Dedicated SQL Pool)
* **Visualization:** **Power BI**
* **Security/Governance:** Azure Active Directory (Entra ID), Azure Key Vault (for secret management)

## Dataset

The project utilizes a relational dataset fetched via API, consisting of several key tables simulating an e-commerce or retail environment:
* `Customers`: Customer demographics and details.
* `Calendar`: Date dimension for time-series analysis.
* `Sales/Orders`: Transactional data.

##  Implementation Details

### Phase 1: Data Ingestion (Source to Bronze)
* **Tool:** Azure Data Factory (ADF).
* **Process:**
    * Created Linked Services to connect to the HTTP Source (GitHub API) and the ADLS Gen2 sink.
    * Developed **Dynamic Pipelines** using parameters and `ForEach` loops to iterate through multiple datasets/files efficiently.
    * Ingested raw JSON/CSV data directly into the `bronze` container in ADLS Gen2.

### Phase 2: Data Transformation (Bronze to Silver)
* **Tool:** Azure Databricks (PySpark).
* **Process:**
    * Mounted the ADLS Gen2 storage to Databricks.
    * Read raw data from the Bronze layer.
    * Performed data cleaning, type casting, and standardizing date formats.
    * Handled multiple tables and joined data where necessary.
    * Wrote the cleaned data to the `silver` container in Delta/Parquet format.

### Phase 3: Data Serving (Silver to Gold)
* **Tool:** Azure Synapse Analytics.
* **Process:**
    * Created a Serverless or Dedicated SQL Pool.
    * Created external tables or pipelines to load transformed data from the Silver layer into the warehouse.
    * Modeled the data for optimized query performance.

### Phase 4: Reporting
* **Tool:** Power BI.
* **Process:**
    * Established a direct connection to Azure Synapse Analytics.
    * Created a data model with proper relationships.
    * Designed a dashboard featuring key KPIs such as Total Customers, Sales Trends, and Order volumes.

##  Key Features
* **Dynamic Orchestration:** Instead of hardcoding datasets, the pipeline dynamically fetches and processes files, making the solution scalable.
* **Secure Connectivity:** Uses Managed Identities and Key Vaults to handle credentials securely.
* **Scalable Compute:** Leverages the distributed power of Spark (Databricks) to handle large datasets.
* **End-to-End Automation:** The entire flow is automated via ADF triggers.

##  How to Run

1.  **Azure Setup:** Create a Resource Group and provision ADLS Gen2, Data Factory, Databricks, and Synapse Analytics.
2.  **Storage:** Create containers named `bronze`, `silver`, and `gold` in your storage account.
3.  **ADF Pipeline:** Import the pipeline definitions (or recreate them) to fetch data from the provided API endpoint.
4.  **Databricks:** Import the notebooks, configure the cluster, and mount your storage.
5.  **Synapse:** Set up your SQL Pool and create the necessary schemas.
6.  **Power BI:** Connect your desktop client to the Synapse SQL endpoint.



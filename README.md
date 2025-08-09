# Earthquake Data Pipeline – Databricks + Azure Data Factory

## Project Description

This project implements an **end-to-end data pipeline** for ingesting, transforming, and analyzing earthquake data retrieved from the **USGS (United States Geological Survey) API**, the official seismic monitoring agency of the United States.

The solution follows the **Medallion Architecture** (Bronze → Silver → Gold) and is developed using **Databricks notebooks**, with the entire workflow orchestrated in **Azure Data Factory**.

---

## Objectives

- **Ingest seismic data** from the public USGS API.
- **Process and transform** the data using PySpark in Databricks.
- Apply the **Medallion Architecture** for structured and clean data layers.
- **Store results** in optimized formats for analytics and reporting.
- **Orchestrate the workflow** using Azure Data Factory.

---

## Technologies Used

- **Databricks** – Development of notebooks with PySpark.
- **PySpark** – Distributed data processing.
- **Azure Data Lake Storage Gen2** – Storage for Bronze, Silver, and Gold layers.
- **Azure Data Factory** – Orchestration and scheduling of pipelines.
- **USGS Earthquake API** – Real-time seismic data source.
- **Medallion Architecture** – Layered design for data quality and enrichment.

---

## Project Structure

```bash
earthquake_databricks_adf_pipeline/
│

├── 01_ingestion_bronze.py     # Ingestion from API to Bronze layer
├── 02_transform_silver.py     # Cleaning and normalization
├── 03_enrichment_gold.py      # Enrichment and final preparation
│
├── adf_pipeline/
│   ├── pipeline.json              # ADF pipeline definition
│
├── README.md
└── requirements.txt               # Required dependencies
```

## Medallion Architecture
- **Bronze**: Raw ingested data exactly as received from the USGS API (append-only, minimal transformations).
- **Silver**: Cleaned and standardized data (types normalized, null handling, deduplication, basic QC).
- **Gold**: Enriched, aggregated, and analytics-ready datasets (KPIs by time/region/magnitude, etc.).

## End-to-End Workflow
1. Extraction (Bronze)
Bronze notebook (01_ingestion_bronze.py) requests events from the USGS API (e.g., last day/week/month endpoints) and lands them in ADLS Bronze.
Raw data stored in JSON string format.

2. Transformation (Silver)
Notebook 02_transform_silver.py cleans and normalizes fields (timestamps, coordinates, magnitude types, depth), enforces schema, and checks for nulls.
Outputs Silver Parquet tables .

3. Enrichment (Gold)
Notebook 03_enrichment_gold.py derives metrics (e.g., add city using the coordinates, classifies the earthquake).
Stores Gold datasets in parquet.

4. Orchestration (ADF)
An ADF pipeline triggers the notebooks in sequence (Bronze → Silver → Gold), handles retries, logging, and scheduling (e.g., hourly/daily).
ADF linked services reference Databricks and ADLS; parameters control date ranges and run modes (full/incremental).


**Author**

César Rangel

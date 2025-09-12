# GizmoBox
Gizmobox: End-to-End ETL Pipeline on Azure Databricks
1. Executive Summary
This document outlines the architecture, implementation, and key outcomes of the Gizmobox project, an end-to-end ETL (Extract, Transform, Load) solution built on Azure Databricks. The project's primary goal was to create a scalable and robust data pipeline to ingest, clean, and aggregate data from diverse sources, providing actionable insights for business clients. By leveraging the Medallion Architecture and Azure's cloud services, the project successfully delivered a fully automated and reliable data platform.

2. Project Objective
To engineer a comprehensive ETL solution for Gizmobox that handles the entire data lifecycle, from ingestion of raw files to the delivery of clean, aggregated data ready for analytics and reporting. The solution was designed to be resilient to data inconsistencies and to scale with business growth.

3. Technology Stack
Cloud Platform: Microsoft Azure

ETL & Compute: Azure Databricks

Data Storage: Azure Data Lake Storage Gen2, Azure SQL Database

Data Governance: Databricks Unity Catalog

Authentication: Azure Access Connector for Databricks

4. Architectural Overview: The Medallion Architecture
The project's data architecture is based on the Medallion Lakehouse pattern, which structures data into three distinct layers: Bronze, Silver, and Gold. This approach ensures data quality, governance, and traceability throughout the pipeline.

Bronze Layer (Raw Data)
This layer acts as the landing zone for all raw data. Data is ingested as-is, with minimal transformations, maintaining a complete and immutable history of the source data.

Silver Layer (Cleaned & Enriched)
In this layer, data from the Bronze layer is cleaned, transformed, and enriched. Data quality checks and business logic are applied to create a single source of truth.

Gold Layer (Curated & Aggregated)
This final layer contains highly refined and aggregated data, optimized for specific business use cases, reporting, and dashboarding.

5. End-to-End ETL Pipeline Breakdown
a. Data Ingestion (Bronze Layer)
Customers: JSON files from the customers folder in Azure Data Lake Storage were ingested directly.

Addresses: TSV files from the addresses folder were ingested and stored as Delta tables.

Payments: CSV files from the payments folder were ingested as an external table using the abfss protocol, allowing Databricks to directly read the data from Azure Data Lake Storage.

Orders: JSON files from the orders folder were ingested. A key challenge was handling a data quality issue where the date column lacked proper quotes. This was addressed by implementing custom parsing logic within the Databricks notebook before loading the data into the Bronze Delta table.

Refunds: Data from the refunds table in the Azure SQL Database was extracted and loaded into a Bronze Delta table.

b. Data Transformation (Silver Layer)
All raw data from the Bronze layer was processed to create a clean, conformant dataset. Key transformations included:

Data Type Casting: Ensuring all columns have the correct data types.

Data Quality Checks: Identifying and handling nulls, duplicates, and inconsistent values.

Schema Enforcement: Applying a consistent schema to all incoming data.

Specific Logic: A custom function was developed in a Databricks notebook to fix the inverted comma issue in the orders JSON data, ensuring that the date column was correctly parsed for downstream processing.

c. Data Aggregation (Gold Layer)
The clean data from the Silver layer was aggregated and transformed to meet business requirements, resulting in curated tables and views. Examples include:

customer_address: A view created by joining customer and address data to provide a unified table for client-facing applications.

monthly_order_summary: An aggregated table summarizing orders and revenue by month for business intelligence.

6. Key Accomplishments & Project Management
End-to-End Ownership: Successfully managed the entire project lifecycle, from setting up Azure resources (Storage Container, SQL DB, Access Connector) to deploying the final data models.

Infrastructure as Code: Created and managed compute clusters and Unity Catalog volumes, tables, and views entirely within Databricks notebooks.

Data Quality Handling: Proactively identified and solved a critical data quality issue in the raw JSON data, preventing errors in downstream analytics.

Unified Data Platform: Built a cohesive data lakehouse using Unity Catalog to centralize data governance and access control across all Databricks workloads.

Azure Databricks NYC Taxi – End-to-End Data Engineering Project
This document provides a complete, production-quality overview of an end-to-end data engineering pipeline built using Azure, Databricks, Delta Lake, and the NYC Taxi dataset.
1. Project Overview
This project demonstrates how to ingest, store, transform, orchestrate, and analyze large-scale datasets on Azure using modern data engineering tools. It uses the NYC Taxi dataset as a practical real-world example.
2. Architecture
The solution uses Azure Data Factory for ingestion, Azure Data Lake Storage Gen2 for data storage, Azure Databricks for processing, Delta Lake for optimized storage layers, and Power BI or Databricks SQL for reporting.

Architecture Diagram (text representation):
                Public Data Source
                       |
                Azure Data Factory
                       |
         ┌───────────────────────────────────┐
         │ Azure Data Lake Storage Gen2       │
         │ raw → bronze → silver → gold       │
         └───────────────────────────────────┘
                       |
                Azure Databricks
                       |
                Databricks SQL / Power BI
3. Medallion Architecture (Delta Lake)
The project follows the Bronze/Silver/Gold medallion architecture:
- RAW: Raw files ingested as-is.
- BRONZE: Schema applied, minimal cleanup.
- SILVER: Cleaned, deduplicated, normalized data.
- GOLD: Aggregate tables ready for analytics and BI.
4. Repository Structure
The repository includes infrastructure code, Databricks notebooks, transformation scripts, tests, and deployment scripts.

Structure:
├── notebooks/
│   ├── 0-setup.py
│   ├── 1_bronze_ingest.py
│   ├── 2_silver_transform.py
│   └── 3_gold_aggregates.py
├── infrastructure/main.tf
├── scripts/deploy_databricks_notebooks.sh
└── tests/test_transforms.py
5. Setup Instructions
1. Deploy resources via Terraform.
2. Configure Databricks cluster and secret scopes.
3. Ingest raw NYC Taxi data into ADLS using Azure Data Factory or manual upload.
4. Execute Databricks ETL notebooks in order: setup → bronze → silver → gold.
5. Connect Power BI or Databricks SQL Warehouse to GOLD tables for visualization.
6. Databricks ETL Workflow
The ETL process uses PySpark notebooks and Delta Lake tables.

Bronze Layer:
- Read raw CSV/JSON files
- Apply basic schema
- Store as Delta

Silver Layer:
- Clean data
- Deduplicate rows
- Convert timestamps

Gold Layer:
- Produce analytical aggregates such as trips per day, average fare, vendor metrics.
7. CI/CD Pipeline
The CI/CD workflow uses GitHub Actions to:
- Deploy notebooks to Databricks
- Run unit tests using pytest
- Validate Terraform
- Promote code to production environments
8. Testing Approach
Unit tests validate data transformations and ensure data quality rules:
- Column type checks
- Null validations
- Row count expectations
- Business logic (e.g., trip distance > 0)
9. Security Best Practices
Security includes:
- Managed Identity usage
- Key Vault for secret storage
- Restricted ADLS access policies
- Encrypted storage accounts
- Databricks cluster access controls
10. Monitoring
Monitoring and observability tools used:
- Azure Monitor for logs and metrics
- Databricks job logs and execution history
- Delta Lake expectations for data quality
- ADF pipeline monitoring and alerts
11. Technologies Used
- Azure Databricks
- Delta Lake
- Azure Data Factory
- Azure Data Lake Storage Gen2
- Azure Key Vault
- Terraform
- GitHub Actions
- Power BI
12. Project Goals
This project aims to:
- Demonstrate real-world data engineering architecture
- Build an enterprise-grade pipeline on Azure
- Showcase Databricks and Delta Lake best practices
- Provide a reusable template for future pipelines

# Azure Databricks NYC Taxi – End-to-End Data Engineering Project


📘 Project Overview

This project builds a complete Azure-based data pipeline:

Ingest NYC Taxi trip data → Azure Data Lake (raw)

Process with Databricks + Delta Lake into
Bronze → Silver → Gold layers

Serve analytics via Databricks SQL or Power BI

Deploy infrastructure using Terraform

CI/CD with GitHub Actions

🏗 Architecture
NYC Taxi Public Data
        ↓
Azure Data Lake Storage Gen2 (Raw)
        ↓
Databricks (Bronze → Silver → Gold)
        ↓
Delta Lake Managed Tables
        ↓
Power BI / Databricks SQL Warehouse

🥇 Medallion Architecture
Raw Layer

Unmodified JSON/CSV files exactly as received.

Bronze Layer

Basic parsing

Minimal cleaning

Schema-on-read

Stored in Delta format

Silver Layer

Data quality checks

Typed schema

Deduplication

Error handling

Gold Layer

BI-ready data marts

Aggregated KPIs

Optimized for reporting

📁 Repository Structure
.
├── README.md
├── notebooks/
│   ├── 0-setup.py
│   ├── 1_bronze_ingest.py
│   ├── 2_silver_transform.py
│   └── 3_gold_aggregates.py
├── infrastructure/
│   └── main.tf
├── scripts/
│   └── deploy_databricks_notebooks.sh
└── tests/
    └── test_transforms.py

🚀 Getting Started
Prerequisites

Azure Subscription

Azure Databricks Workspace

Terraform CLI

GitHub Account

Power BI (optional)

Quickstart
git clone https://github.com/<your-username>/azure-databricks-nyc-taxi.git
cd azure-databricks-nyc-taxi

🔄 ETL Flow (Databricks)
0-setup.py

Configure storage locations

Create secret scopes

Mount ADLS (if used)

1_bronze_ingest.py

Reads raw data → writes Bronze Delta

df.write.format("delta") \
  .mode("append") \
  .partitionBy("pickup_date") \
  .save("/mnt/delta/bronze/nyc_taxi")

2_silver_transform.py

Clean code

Type casting

Deduplication

Null handling

3_gold_aggregates.py

KPI creation

Aggregated marts for BI

🏗 Infrastructure (Terraform)

Resources created:

Resource Group

Storage Account (ADLS Gen2)

Databricks Workspace

Key Vault

Service Principal / Managed Identity

Run:

cd infrastructure
terraform init
terraform plan
terraform apply

🔧 CI/CD

Implement using GitHub Actions:

Validate Terraform

Run PySpark unit tests

Deploy notebooks to Databricks

Promote to production

Workflow example steps:

terraform validate

pytest

Databricks CLI upload

🧪 Testing & Data Quality

Use pytest for local tests:

pytest tests/


Recommended checks:

Schema validation

Null checks

Range validations (e.g. trip_distance > 0)

Duplicate detection

🔐 Security

Use Azure Key Vault for all secrets

Enable Databricks secret scopes

Use Managed Identity where possible

Implement least-privilege RBAC

Enable encryption at rest (CMK optional)

📊 Monitoring & Cost Control

Enable cluster autoscaling

Use cluster pools

Track costs via Azure Cost Management

Enable logging to Azure Monitor

Configure job alerts

🔧 Tech Stack

Azure Databricks

ADLS Gen2

Delta Lake

PySpark

Azure Key Vault

Terraform

GitHub Actions

Power BI

Azure Databricks NYC Taxi – End-to-End Data Engineering Project

This repository contains an end-to-end data pipeline built on Azure + Databricks + Delta Lake, using the NYC Taxi dataset as a real-world example.
It demonstrates modern data engineering practices including medallion architecture, orchestration, PySpark transformations, CI/CD, and Power BI analytics.

📌 Project Overview

This project builds a full data platform on Azure:

Ingest raw NYC taxi trip data (CSV/JSON) into Azure Data Lake Storage Gen2 (ADLS)

Transform using Azure Databricks and Delta Lake (Bronze → Silver → Gold)

Schedule workflows using ADF or Databricks Jobs

Serve analytics through Databricks SQL or Power BI

Manage infra via Terraform

Implement unit tests using pytest

This repo is a portfolio-friendly, industry-standard demonstration of a modern data stack.

🏗️ Architecture
                ┌──────────────────────────────┐
                │     Public Data Source        │
                │     (NYC Taxi Trip Data)      │
                └──────────────┬───────────────┘
                               │ (ADF Copy)
                               ▼
┌────────────────────────────────────────────────────────┐
│              Azure Data Lake Storage Gen2              │
│      raw/ → bronze/ → silver/ → gold/ (Delta)          │
└───────────────┬───────────────────────┬────────────────┘
                │                       │
                │ (PySpark ETL)         │ (SQL)
                ▼                       ▼
      ┌──────────────────┐     ┌──────────────────────┐
      │ Azure Databricks │     │ Databricks SQL        │
      │  Notebooks/Jobs  │     │ BI + Dashboards       │
      └─────────┬────────┘     └──────────┬───────────┘
                │                         │
                │ (Schedules/Triggers)    │
                ▼                         ▼
         ┌─────────────┐         ┌──────────────────────┐
         │ Azure ADF    │         │ Power BI             │
         │ Pipelines    │         │ Reports              │
         └─────────────┘         └──────────────────────┘

🌊 Medallion Architecture
Layer	Purpose	Example
Raw	Original ingest, unmodified	JSON/CSV
Bronze	Schema applied, minimal cleanup	Delta table
Silver	Cleaned, deduped, normalized	Typed columns, QA checks
Gold	BI-ready aggregates	Daily KPIs, top vendors, KPIs
📂 Repository Structure
├── README.md
├── notebooks/
│   ├── 0-setup.py
│   ├── 1_bronze_ingest.py
│   ├── 2_silver_transform.py
│   ├── 3_gold_aggregates.py
├── infrastructure/
│   └── main.tf
├── scripts/
│   └── deploy_databricks_notebooks.sh
└── tests/
    └── test_transforms.py

⚙️ Setup Instructions
1️⃣ Clone Repository
git clone https://github.com/<your-username>/azure-databricks-nyc-taxi.git
cd azure-databricks-nyc-taxi

2️⃣ Provision Azure Resources (Terraform)

Inside infrastructure/main.tf you will configure:

Resource Group

Storage Account (ADLS Gen2)

Databricks Workspace

Key Vault

Service Principal

Networking/Identity

Example:

cd infrastructure
terraform init
terraform plan
terraform apply

3️⃣ Configure Databricks

Create a secret scope (Key Vault backed)

Add:

sp-client-id

sp-client-secret

tenant-id

Attach a cluster and run:

00-setup notebook

Configure ADLS mounts or direct abfss:// reads

4️⃣ Ingest Data (ADF or Manual Upload)
Option A: Azure Data Factory Pipeline

Create Copy Activity

Source: HTTP → NYC Taxi files (or local blob)

Sink: ADLS raw/nyc_taxi/

Option B: Manual Upload

Upload dataset to:

raw/nyc_taxi/


(You can request “generate 1000 taxi JSON file” and I will produce it.)

5️⃣ Run ETL in Databricks
Bronze Load

Reads raw JSON/CSV

Applies schema and writes Delta table

Silver Transform

Type casting

Dedupe

Create timestamps

Gold Aggregates

Daily KPIs

Trip distance statistics

Vendor quality metrics

📊 Power BI / Databricks SQL

Expose Gold Delta tables through:

🔹 Databricks SQL Warehouse

OR

🔹 Azure Synapse External Table

OR

🔹 Power BI → "Lakehouse" connector

Common BI Outputs:

Trips per day

Average fare per mile

Pickup hotspots (geo heatmap)

Vendor performance

🚀 Deployment Workflow (CI/CD)
GitHub Actions Pipeline Includes:

Databricks CLI authentication

Notebook deployment

PyTest for data transformation unit tests

Terraform deploy on main branch

🧪 Testing

In tests/test_transforms.py:

Validate schema

Row count > 0

Null checks

Business rules (e.g., trip distance > 0)

Run:

pytest tests/

🔐 Security

Use Managed Identity when possible

Store secrets in Key Vault only

Limit ADLS access to Databricks service principal

Enable DBFS root encryption

📈 Monitoring

Databricks Job metrics

ADF pipeline monitoring

Delta Lake expectations

Alerts for failed jobs

Azure Cost Management budgets

📘 Technologies Used

Azure Databricks (PySpark, Delta Lake)

Azure Data Lake Storage Gen2

Azure Data Factory

Azure Key Vault

Terraform

Power BI

GitHub Actions

🎯 Project Goals

Demonstrate real-world data engineering workflows

Show Delta architecture implementation

Build a professional Azure-based portfolio project

Enable reusable templates for future pipelines

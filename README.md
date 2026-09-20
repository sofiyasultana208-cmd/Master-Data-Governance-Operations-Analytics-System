# Master-Data-Governance-Operations-Analytics-System

An end-to-end data engineering and operations analytics project built to demonstrate Master Data Management (MDM), automated ETL data governance, SQL Root Cause Analysis (RCA), and interactive executive reporting using Python, SQLite, Advanced SQL, and Power BI.

📌 Executive Summary
Operational reporting and decision-making often suffer from data quality defects such as duplicate primary keys, invalid contact formats, missing metadata, and orphan foreign keys.

This project simulates a real-world enterprise dataset and establishes an automated Data Governance & Quality Engine. The pipeline checks incoming data against strict governance policies, routes corrupted records to an audit log table for root cause analysis, and loads sanitized master data into a relational Star Schema data model for executive SLA tracking and financial analytics.

🛠️ Tech Stack & Architecture
Programming & Automation: Python (Pandas, NumPy, Datetime)

Databases & Querying: SQLite, Advanced SQL (CTEs, Window Functions, Aggregations)

Business Intelligence & Visualization: Power BI Desktop, DAX (Data Analysis Expressions)

Development Environment: Google Colab / Jupyter Notebook

⚙️ Data Pipeline & Project Workflow
Code snippet
graph TD
    A[Raw Operational Data] --> B[Python Data Governance Engine]
    B -->|Corrupted Records| C[ETL Audit Log Table]
    B -->|Sanitized Master Data| D[Production Database / Excel]
    C --> E[SQL Root Cause Analysis]
    D --> F[Power BI Star Schema Data Model]
    F --> G[Executive DAX Dashboard]
1. Raw Data Ingestion & Governance Auditing (Python & SQLite)
Generated relational structures for Customers (Dim_Customer), Products/Services (Dim_Product), and Transactions (Fact_Transactions).

Injected real-world data quality defects (e.g., non-unique primary keys, invalid email regex patterns, negative/missing revenue amounts, and orphan customer IDs).

Programmed a Python validation engine that intercepts bad records, writes audit metadata into an etl_audit_log table, and preserves 100% data reconciliation without silent downstream failures.

2. SQL Root Cause Analysis (RCA)
Authored Advanced SQL queries utilizing Common Table Expressions (CTEs) and aggregations to analyze defect distributions and calculate total operational risk impact across systems:

SQL
WITH DefectSummary AS (
    SELECT 
        TableName,
        DefectType,
        Severity,
        COUNT(RecordID) AS TotalDefects
    FROM etl_audit_log
    GROUP BY TableName, DefectType, Severity
),
TotalLogCount AS (
    SELECT COUNT(*) AS TotalSystemDefects FROM etl_audit_log
)
SELECT 
    ds.TableName,
    ds.DefectType,
    ds.Severity,
    ds.TotalDefects,
    ROUND((CAST(ds.TotalDefects AS FLOAT) / tlc.TotalSystemDefects) * 100, 2) AS DefectPercentage
FROM DefectSummary ds
CROSS JOIN TotalLogCount tlc
ORDER BY ds.TotalDefects DESC;
3. Data Modeling & DAX Measures (Power BI)
Imported the sanitized datasets into Power BI Desktop, structured a 1-to-many Star Schema, and created business intelligence metrics:

Overall SLA Compliance Rate:

Code snippet
Overall SLA Compliance Rate = 
DIVIDE(
    CALCULATE(
        COUNTROWS(Fact_Transactions),
        Fact_Transactions[ActualSLA_Days] <= RELATED(Dim_Product[StandardSLA_Days])
    ),
    COUNTROWS(Fact_Transactions),
    0
)
Data Health Score:

Code snippet
Data Health Score = 
VAR TotalRecordsProcessed = COUNTROWS(Fact_Transactions) + COUNTROWS(Dim_Customer)
VAR TotalErrors = COUNTROWS(ETL_Audit_Log)
RETURN
DIVIDE(TotalRecordsProcessed - TotalErrors, TotalRecordsProcessed, 1)
📊 Key Results & Impact
100% Data Reconciliation: Successfully isolated defects into audit logs while protecting production environments from corrupted financial metrics.

Automated Data Quality Tracking: Established dynamic Data Health Scores to give stakeholders real-time visibility into data pipeline reliability.

SLA Visibility: Delivered dynamic tracking across product lines, identifying operational bottlenecks in service delivery times.

📂 Repository Structure
Plaintext
├── data/
│   ├── Operations_Governance_Dataset.xlsx    # Sanitized multi-sheet Excel dataset
│   └── operations_raw.db                     # SQLite raw & staging database
├── notebooks/
│   └── Data_Governance_Pipeline.ipynb        # Google Colab ETL & SQL RCA code
├── reports/
│   └── Operations_Governance_Dashboard.pbix  # Power BI dashboard file
├── README.md                                 # Project documentation
🚀 How to Run This Project
Clone the Repository:

Bash
git clone https://github.com/your-username/master-data-governance-analytics.git
cd master-data-governance-analytics
Execute Python Pipeline:

Open notebooks/Data_Governance_Pipeline.ipynb in Google Colab or Jupyter.

Run all cells to generate raw data, execute the governance engine, and export Operations_Governance_Dataset.xlsx.
https://ai.studio/apps/e8b953f6-2c74-421a-92d3-909c970d4a8a

Open Power BI Dashboard:

Download Operations_Governance_Dataset.xlsx.

Open Power BI Desktop and load the dataset to view or modify the interactive reports.

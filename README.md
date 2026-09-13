# 📊 Material Master Data Quality Dashboard

> **A Power BI and SQL Server data-quality solution for monitoring, identifying, and analysing Material Master Data issues.**

[![Power BI](https://img.shields.io/badge/Power%20BI-Dashboard-F2C811?logo=powerbi\&logoColor=black)](https://powerbi.microsoft.com/)
[![SQL Server](https://img.shields.io/badge/SQL%20Server-Data%20Validation-CC2927?logo=microsoftsqlserver\&logoColor=white)](https://www.microsoft.com/sql-server)
[![SQL](https://img.shields.io/badge/SQL-Data%20Quality-4479A1?logo=postgresql\&logoColor=white)](https://www.microsoft.com/sql-server)
[![Status](https://img.shields.io/badge/Status-In%20Progress-orange)](#project-status)

---

## 📌 Project Overview

This project focuses on **Material Master Data Quality Management** using SQL Server and Microsoft Power BI.

Material Master Data is a critical component of enterprise operations because it contains information used across procurement, inventory, manufacturing, sales, logistics, and reporting processes.

Poor-quality material master data can result in:

* Duplicate material records
* Missing mandatory information
* Invalid material attributes
* Incorrect classifications
* Inconsistent descriptions
* Incomplete supplier or purchasing information
* Reporting inaccuracies
* Operational inefficiencies

The objective of this project is to build a practical **Data Quality Monitoring Dashboard** that allows users to identify problematic material records and understand the overall quality of the material master.

The project combines **SQL-based validation rules** with **Power BI reporting** to transform raw material records into actionable data-quality insights.

---

# 🎯 Project Objectives

The main objectives of this project are to:

1. Build a simulated Material Master dataset.
2. Introduce realistic data-quality problems into the dataset.
3. Store and manage the data using SQL Server.
4. Develop SQL validation rules to identify data-quality issues.
5. Categorise records according to different data-quality dimensions.
6. Create a consolidated data-quality view for reporting.
7. Connect the validated data to Power BI.
8. Develop an interactive dashboard for monitoring data quality.
9. Allow users to drill into individual problematic material records.
10. Demonstrate how Master Data Management concepts can be applied using modern data tools.

---

# 🏗️ Solution Architecture

The overall solution follows a simple data-quality pipeline:

```text
┌─────────────────────────┐
│     Source Dataset      │
│                         │
│ Material Master Records │
└────────────┬────────────┘
             │
             ▼
┌─────────────────────────┐
│       SQL Server        │
│                         │
│ Material Master Table   │
└────────────┬────────────┘
             │
             ▼
┌─────────────────────────┐
│   Data Quality Rules    │
│                         │
│ Duplicate Checks        │
│ Completeness Checks     │
│ Validity Checks         │
│ Consistency Checks      │
└────────────┬────────────┘
             │
             ▼
┌─────────────────────────┐
│ Material Quality View   │
│                         │
│ vw_material_quality     │
└────────────┬────────────┘
             │
             ▼
┌─────────────────────────┐
│       Power BI          │
│                         │
│ Data Quality Dashboard  │
└─────────────────────────┘
```

---

# 🧰 Technology Stack

| Technology       | Purpose                                  |
| ---------------- | ---------------------------------------- |
| **SQL Server**   | Data storage and validation              |
| **T-SQL**        | Data-quality rules and analysis          |
| **Power BI**     | Interactive dashboard and visualisation  |
| **DAX**          | KPI calculations and dashboard measures  |
| **CSV**          | Simulated source data                    |
| **Git / GitHub** | Version control and portfolio management |

---

# 📁 Dataset

The project uses a simulated **Material Master dataset** containing hundreds of material records.

The dataset represents the type of master data that may exist within an ERP environment.

Example material attributes include:

| Field                | Description                             |
| -------------------- | --------------------------------------- |
| Material ID          | Unique identifier for the material      |
| Material Description | Name/description of the material        |
| Material Type        | Classification of the material          |
| Material Group       | Group/category assigned to the material |
| Base Unit            | Primary unit of measure                 |
| Plant                | Associated plant/location               |
| Supplier             | Supplier information                    |
| Status               | Material status                         |
| Creation Date        | Date the material was created           |
| Last Updated         | Last modification date                  |

The dataset intentionally contains data-quality problems so that the dashboard can demonstrate how such issues can be detected and monitored.

---

# 🔎 Data Quality Framework

The project evaluates material master data across several dimensions.

## 1. Completeness

Checks whether required fields contain values.

Examples:

* Missing Material Description
* Missing Material Type
* Missing Material Group
* Missing Base Unit
* Missing Plant
* Missing Supplier

A record with one or more mandatory fields missing is flagged as having a completeness issue.

---

## 2. Uniqueness

Checks whether material records are duplicated.

Example:

```text
Material ID
-----------
MAT000101
MAT000102
MAT000102  ← Duplicate
MAT000103
```

Duplicate material IDs can cause:

* Incorrect reporting
* Multiple representations of the same material
* Inventory inconsistencies
* Procurement issues

---

## 3. Validity

Checks whether attribute values conform to predefined business rules.

Examples:

```text
Material Type must belong to an approved list.

Base Unit must be a recognised unit.

Status must be one of the permitted values.

Material ID must follow the expected format.
```

---

## 4. Consistency

Checks whether related fields contain logically compatible values.

For example:

```text
Material Type = Raw Material
Material Group = Finished Goods
```

may represent an inconsistent classification depending on the defined business rules.

---

# 🧪 SQL Data Quality Validation

The validation framework uses SQL Server to identify problematic records before they are consumed by Power BI.

Each validation rule is assigned a unique identifier.

Example structure:

| Rule ID | Data Quality Dimension | Example Check                      |
| ------- | ---------------------- | ---------------------------------- |
| DQ001   | Completeness           | Missing Material ID                |
| DQ002   | Completeness           | Missing Material Description       |
| DQ003   | Completeness           | Missing Material Type              |
| DQ004   | Validity               | Invalid Material Type              |
| DQ005   | Validity               | Invalid Base Unit                  |
| DQ006   | Uniqueness             | Duplicate Material ID              |
| DQ007   | Validity               | Invalid Status                     |
| DQ008   | Completeness           | Missing Plant                      |
| DQ009   | Completeness           | Missing Supplier                   |
| DQ010   | Consistency            | Inconsistent attribute combination |

The exact validation logic is implemented in SQL Server.

---

# 🗄️ SQL Quality View

After applying the validation rules, the results are consolidated into a reporting-friendly SQL view:

```sql
vw_material_quality
```

This view provides Power BI with a structured dataset containing the original material information together with its data-quality results.

Conceptually, the output looks like:

| Material ID | Description     | Quality Status | Error Count | Error Type                 |
| ----------- | --------------- | -------------- | ----------: | -------------------------- |
| MAT000001   | Steel Bolt      | Valid          |           0 | NULL                       |
| MAT000002   | Aluminium Sheet | Issue          |           1 | Missing Supplier           |
| MAT000003   | Motor Assembly  | Issue          |           2 | Duplicate + Invalid Status |

This approach keeps much of the data-quality logic within the SQL layer while allowing Power BI to focus on analysis and visualisation.

---

# 📊 Power BI Dashboard

The validated SQL dataset is connected to Power BI to create an interactive Material Master Data Quality Dashboard.

The dashboard is designed to answer questions such as:

* How many materials exist?
* How many unique materials exist?
* How many records contain data-quality issues?
* What percentage of records are valid?
* Which data-quality dimensions have the most issues?
* Which materials require remediation?
* What types of errors occur most frequently?

---

# 📄 Dashboard Page 1 — Data Quality Overview

### Status: ✅ Completed

The first page provides an executive-level overview of Material Master Data Quality.

The purpose of this page is to allow users to quickly understand the overall condition of the dataset.

### Key KPIs

The page includes high-level metrics such as:

* **Total Materials**
* **Unique Materials**
* **Valid Materials**
* **Materials with Issues**
* **Overall Data Quality %**
* **Completeness**
* **Validity**
* **Uniqueness**

These KPIs provide an immediate snapshot of the health of the material master.

---

## 📈 Page 1 Analysis

The overview page allows users to identify whether the dataset has a generally healthy or problematic data-quality profile.

For example:

```text
Total Records
      │
      ├── Valid Records
      │
      └── Records With Issues
                 │
                 ├── Completeness Issues
                 ├── Validity Issues
                 ├── Duplicate Issues
                 └── Consistency Issues
```

This provides a starting point for further investigation.

---

# 📄 Dashboard Page 2 — Data Quality Issues

### Status: ✅ Completed

The second page focuses on identifying the individual records that contain data-quality problems.

While Page 1 answers:

> **"How good is our material master data?"**

Page 2 focuses on:

> **"Which records are causing the problems?"**

The page allows users to analyse problematic materials at a more granular level.

---

## 🔍 Page 2 Analysis

The detailed issue view helps users identify:

* Material IDs requiring attention
* Records with missing information
* Invalid values
* Duplicate records
* Multiple issues within a single material
* The specific data-quality issue associated with a record

This makes the dashboard more useful for **data cleansing and remediation activities**, rather than simply reporting an overall percentage.

---

# 🔄 Dashboard Navigation Concept

The two completed pages form a progression:

```text
PAGE 1
Executive Overview
       │
       │ Identify overall problems
       ▼
PAGE 2
Detailed Issues
       │
       │ Identify affected records
       ▼
Future Pages
Remediation / Analysis / Trends
```

---

# 📐 DAX Measures

Power BI DAX measures are used to calculate the key dashboard KPIs.

Examples of the types of measures implemented include:

### Total Materials

```DAX
Total Materials =
COUNTROWS('vw_material_quality')
```

### Valid Materials

```DAX
Valid Materials =
CALCULATE(
    COUNTROWS('vw_material_quality'),
    'vw_material_quality'[Quality Status] = "Valid"
)
```

### Materials With Issues

```DAX
Materials With Issues =
CALCULATE(
    COUNTROWS('vw_material_quality'),
    'vw_material_quality'[Quality Status] = "Issue"
)
```

### Data Quality %

```DAX
Data Quality % =
DIVIDE(
    [Valid Materials],
    [Total Materials],
    0
)
```

> The exact DAX implementation may vary depending on the final Power BI data model.

---

# 🧩 Project Structure

The project is organised around the separation of source data, SQL validation, and reporting.

```text
Material-Master-Data-Quality/
│
├── README.md
│
├── data/
│   └── material_master.csv
│
├── sql/
│   ├── 01_create_database.sql
│   ├── 02_create_tables.sql
│   ├── 03_insert_data.sql
│   ├── 04_data_quality_rules.sql
│   ├── 05_material_quality_view.sql
│   └── 06_analysis_queries.sql
│
├── powerbi/
│   └── Material_Master_Data_Quality.pbix
│
├── documentation/
│   └── project_documentation.md
│
└── screenshots/
    └── dashboard/
```

---

# 🚦 Project Status

| Component                     |     Status    |
| ----------------------------- | :-----------: |
| Dataset creation              |   ✅ Complete  |
| SQL Server setup              |   ✅ Complete  |
| Material Master table         |   ✅ Complete  |
| Data-quality validation rules |   ✅ Complete  |
| Quality view                  |   ✅ Complete  |
| Power BI connection           |   ✅ Complete  |
| Dashboard Page 1              |   ✅ Complete  |
| Dashboard Page 2              |   ✅ Complete  |
| Dashboard Page 3              |   ⏳ Planned   |
| Advanced trend analysis       |   ⏳ Planned   |
| Remediation tracking          |   ⏳ Planned   |
| Final documentation           | ⏳ In progress |

> **Current milestone: Page 2 completed.**

The project is intentionally documented based on the current implementation. Future dashboard functionality is listed as planned rather than presented as completed work.

---

# 🚀 Future Improvements

Several enhancements can be added to take the project from a basic data-quality dashboard to a more complete **Master Data Management solution**.

## Page 3 — Data Quality by Dimension

A future page could provide deeper analysis of:

* Completeness
* Validity
* Uniqueness
* Consistency

Example:

```text
Data Quality
│
├── Completeness ── 94%
├── Validity ────── 91%
├── Uniqueness ──── 97%
└── Consistency ─── 89%
```

---

## Page 4 — Material-Level Investigation

A dedicated material profile page could allow users to select a Material ID and inspect:

* Material attributes
* Quality status
* Failed validation rules
* Error count
* Data-quality history
* Required remediation

---

## Page 5 — Data Quality Trends

If historical snapshots are introduced, the dashboard could monitor data quality over time.

Example:

```text
Data Quality %
100% ┤
 95% ┤              ╭────
 90% ┤        ╭─────╯
 85% ┤   ╭────╯
 80% ┤───╯
     └────────────────────
       Jan  Feb  Mar  Apr
```

This would allow users to determine whether data quality is improving or deteriorating.

---

# 💼 Business Value

A Material Master Data Quality dashboard can support organisations by providing visibility into the quality of critical master data.

Improved material data quality can help reduce:

### Procurement Issues

Incorrect or incomplete material information can create problems during purchasing processes.

### Inventory Issues

Duplicate or inconsistent materials can contribute to inaccurate inventory reporting.

### Reporting Issues

Poor master data can affect analytical reports and KPIs.

### Operational Inefficiencies

Users may spend significant time manually identifying and correcting data issues.

### Governance Problems

A centralised quality-monitoring approach provides better visibility into master-data health.

---

# 🧠 Skills Demonstrated

This project demonstrates practical experience in:

### Data Engineering

* SQL Server
* T-SQL
* Data validation
* Data transformation
* Data-quality rules
* SQL views
* Data modelling

### Business Intelligence

* Power BI
* DAX
* KPI development
* Interactive dashboards
* Data visualisation
* Drill-down analysis

### Data Quality / MDM

* Master Data Management
* Data-quality dimensions
* Completeness
* Validity
* Uniqueness
* Consistency
* Data remediation concepts
* Data governance

### Development Practices

* Git version control
* Structured project organisation
* SQL scripting
* Documentation
* Reproducible analytics workflow

---

# 🔬 Example Business Scenario

Imagine an organisation with thousands of materials stored in its ERP system.

Over time, users create materials independently and inconsistent information begins to appear:

```text
MAT10001 → Stainless Steel Bolt
MAT10001 → Stainless Steel Bolt
MAT10002 → Stainless Steel BOLT
MAT10003 → NULL
MAT10004 → Invalid Material Type
```

Without a data-quality monitoring system, these issues may remain hidden.

The solution developed in this project provides a structured approach:

```text
Raw Material Data
        ↓
SQL Validation
        ↓
Quality Rules
        ↓
Quality Status
        ↓
Power BI
        ↓
Identify Problems
        ↓
Data Remediation
```

---

# 📌 Key Takeaways

This project demonstrates how SQL Server and Power BI can work together to create a practical **Master Data Quality Management solution**.

Rather than simply displaying business metrics, the dashboard focuses on identifying the underlying quality of the data itself.

The current implementation successfully demonstrates:

> **Data → Validation → Quality Metrics → Dashboard → Issue Identification**

The next stage of the project will expand this foundation into deeper analysis, trend monitoring, and remediation tracking.

---

# 🛠️ How to Run the Project

## 1. Clone the Repository

```bash
git clone <repository-url>
cd Material-Master-Data-Quality
```

## 2. Prepare SQL Server

Create the required database and execute the SQL scripts in the recommended order:

```text
01_create_database.sql
        ↓
02_create_tables.sql
        ↓
03_insert_data.sql
        ↓
04_data_quality_rules.sql
        ↓
05_material_quality_view.sql
        ↓
06_analysis_queries.sql
```

## 3. Verify the Data

Run validation queries to confirm:

* Records were loaded successfully
* Material IDs exist
* Duplicate records can be detected
* Missing values are identified
* Invalid values are flagged
* The quality view returns the expected results

## 4. Open Power BI

Open:

```text
Material_Master_Data_Quality.pbix
```

Update the SQL Server connection if required.

## 5. Refresh the Dataset

Refresh the Power BI model so that the latest SQL validation results are loaded.

## 6. Explore the Dashboard

Start with:

```text
Page 1 → Overall Data Quality
        ↓
Page 2 → Detailed Data Quality Issues
```

---

# 📚 Project Learning Outcomes

Through this project, the following concepts are demonstrated:

* How master data can be evaluated systematically
* How SQL can be used for automated data-quality validation
* How validation rules can be converted into measurable KPIs
* How Power BI can visualise data-quality problems
* How data-quality monitoring supports Master Data Management
* How technical data-quality results can be translated into business insights

---

# 👨‍💻 Author

**Hakim Has-Yun**

Data / Business Intelligence Portfolio Project

### Focus Areas

`Data Engineering` · `SQL` · `Power BI` · `Data Quality` · `Master Data Management` · `Business Intelligence`

---

## ⭐ Project Status

**Current progress: Page 2 completed**

This repository represents an ongoing portfolio project. Additional dashboard pages, advanced analytics, and remediation functionality may be added in future iterations.

---

> **Built to demonstrate practical data engineering, data quality, and business intelligence skills using SQL Server and Power BI.**

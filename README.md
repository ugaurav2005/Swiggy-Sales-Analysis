# 🍽️ Swiggy Data Analytics — End-to-End Microsoft Fabric Project

An end-to-end data analytics solution built for a simulated **Swiggy** business scenario — transforming raw operational data into business insights using **Microsoft Fabric**, **T-SQL**, **Power BI**, and **Excel**.

---

## 📌 1. Business Scenario

Swiggy is one of India's largest online food delivery platforms, connecting customers, restaurants, and delivery partners across multiple cities. Every day, thousands of orders generate large volumes of operational data related to customers, restaurants, orders, delivery partners, and payments.

Acting as a **Data Analyst**, this project designs an end-to-end analytics solution that transforms this raw data into meaningful business insights — helping the business analyze performance, improve delivery efficiency, and understand customer behavior.

---

## 🎯 2. Project Objective

Build a complete data analytics pipeline that:

1. Stores raw Swiggy datasets in a **Lakehouse**
2. Cleans and validates the data using **SQL**
3. Models the data inside a **Data Warehouse** (Star Schema)
4. Creates a **Power BI Semantic Model**
5. Builds interactive **Power BI dashboards**
6. Publishes the final report for business stakeholders
7. Delivers a parallel **self-service Excel analysis** (pivot tables, charts, slicers)

---

## 🧰 3. Technology Stack

| Layer | Technology |
|---|---|
| Data Storage | Microsoft Fabric **Lakehouse** |
| Data Modeling | Microsoft Fabric **Data Warehouse** (Star Schema) |
| Data Cleaning & Validation | **T-SQL** (SQL Analytics Endpoint / Warehouse SQL) |
| Data Processing / Orchestration | **Fabric Data Pipelines** (Data Factory-style Copy Data activities) |
| Semantic Layer | **Power BI Semantic Model** |
| Visualization | **Power BI Reports** + **Microsoft Excel** (Pivot Tables, Pivot Charts, Slicers) |
| File Format | **Delta / Parquet** (ACID-compliant storage) |
| Cloud Platform | **Microsoft Azure** (free-tier subscription used to provision Fabric capacity) |

---

## 📊 4. Dataset Overview

The project uses a rich, multi-table Swiggy order dataset, structured as one central fact table and four supporting dimension tables:

| Table | Type | Row Count | Description |
|---|---|---|---|
| `fact_orders` | **Fact** | **197,430** | Rich transactional dataset — order-level `price`, `rating`, and `rating_count` metrics with foreign keys into every dimension |
| `dim_date` | Dimension | 243 | Order date attributes (Jan–Aug 2025) |
| `dim_dish` | Dimension | 82,891 | Dish name and food category ordered |
| `dim_location` | Dimension | 995 | State, city, and locality of each order |
| `dim_restaurant` | Dimension | 993 | Restaurant name and details |

**~282,000+ total records** across the model, anchored by a **197K+ row fact table** — large enough to realistically exercise Fabric's Lakehouse-to-Warehouse pipeline performance, SQL validation, and Power BI aggregation at scale.

All dimension tables connect to `fact_orders` via **one-to-many relationships**, forming a clean **Star Schema**.

---

## 🏗️ 5. Data Architecture

```
Raw CSV Files → Lakehouse (Files, Bronze) → Delta Tables (Lakehouse, Silver)
      → Data Warehouse Star Schema (Gold) → Semantic Model
      → Power BI Dashboard + Excel Dashboard → Published to Fabric Workspace
```

**Medallion layers used:**
| Layer | Description |
|---|---|
| 🥉 Bronze | Raw CSVs loaded as-is into Lakehouse `Files` |
| 🥈 Silver | Cleaned & validated Delta tables in Lakehouse `Tables` |
| 🥇 Gold | Curated star-schema tables in the Data Warehouse, used for the semantic model |

**Workspace:** `Swiggy Analytics Project` — contains the Lakehouse, Data Warehouse, Data Pipelines, Power BI Semantic Model, and Power BI Report, all under one unified Fabric workspace.

---

## 🧹 6. Data Cleaning & Validation

Validation and cleaning were performed using T-SQL against the Lakehouse SQL Analytics Endpoint and the Warehouse, including:

- Row-count reconciliation between source CSVs, Lakehouse Delta tables, and Warehouse tables
- Removing duplicate records
- Handling missing values
- Correcting invalid date formats
- Standardizing city names
- Checking referential integrity across fact/dimension tables (e.g. orders without a valid restaurant or location)
- Flagging invalid values (e.g. negative order amounts)

---

## 🔄 7. Project Workflow

1. **Workspace Setup** — Created the `Swiggy Analytics Project` workspace in Fabric
2. **Lakehouse Creation** — Created `swiggy_lakehouse`, organized raw files under `Files/Raw_Data/`
3. **Data Ingestion** — Uploaded raw CSVs (fact + 4 dimension tables) into the Lakehouse
4. **Bronze → Silver** — Converted CSV files into Delta tables both via the UI (`Load to Tables`) and via **Data Factory Copy Data pipelines**
5. **Data Validation** — Used the SQL Analytics Endpoint to validate row counts and referential integrity
6. **Warehouse Setup** — Created `Swiggy Data Warehouse` with a dedicated schema (`swiggy_project`)
7. **Lakehouse → Warehouse** — Built Copy Data pipeline activities to move all 5 tables into the warehouse, forming the Gold layer
8. **Gold Layer Validation** — Re-validated row counts and referential integrity in the warehouse via T-SQL
9. **Semantic Modeling** *(Part 2)* — Defined relationships and DAX measures (Total Orders, Total Revenue, Average Order Value, Average Delivery Time, Orders per Restaurant) on top of the Data Warehouse
10. **Power BI Report** *(Part 2)* — Built and published an interactive dashboard to the Fabric workspace
11. **Excel Analysis** — Built a parallel self-service dashboard using Pivot Tables, Pivot Charts, and Slicers on the same full dataset

---

## Pipeline (Data Lakehouse to Warehouse)
<img width="1393" height="841" alt="Screenshot 2026-08-11 215615" src="https://github.com/user-attachments/assets/799c4ddf-3bac-4790-b5fc-ddfe62f7f757" />

## Data Warehouse
<img width="1812" height="852" alt="Screenshot 2026-08-11 215125" src="https://github.com/user-attachments/assets/f18d6d2c-6155-48eb-8c2f-85b64ad54f29" />

## 📁 8. Repository Structure

```
swiggy-fabric-analytics/
│
├── data/                      # Raw CSV source files (fact + dimension tables)
├── sql/                       # T-SQL scripts (schema creation, cleaning, validation queries)
├── pipelines/                 # Notes/screenshots of Data Factory pipeline configs
├── powerbi/                   # .pbix file (semantic model + report)
├── excel/                     # Excel pivot-table & slicer-based dashboard
├── docs/
│   ├── Business_Requirement.docx   # Original business requirement document
│   └── architecture.png            # Architecture diagram / screenshots
├── README.md
└── LICENSE
```
<img width="1137" height="587" alt="Screenshot 2026-08-11 215644" src="https://github.com/user-attachments/assets/ed979af7-501c-43d2-8429-64b2a71e1b87" />

---
## Star Schema 
<img width="1832" height="862" alt="Screenshot 2026-08-11 215818" src="https://github.com/user-attachments/assets/f8817d61-5937-4c6e-8315-dabaf1ca9a88" />



## 🚀 9. How to Reproduce

1. Create a free Microsoft Fabric-enabled Azure account (or use an existing Microsoft 365/Fabric license)
2. Create a new **Workspace** named `Swiggy Analytics Project`
3. Create a **Lakehouse** and upload the CSVs from `/data`
4. Load files into **Delta tables** (via UI or Data Factory pipeline)
5. Validate row counts and integrity using the SQL scripts in `/sql`
6. Create a **Data Warehouse**, create the `swiggy_project` schema, and run the pipeline activities to copy tables over
7. Re-validate with the warehouse validation queries
8. Build the **semantic model** and **Power BI report**, then publish to the workspace
9. Optionally, open `/excel` for the self-service pivot-table dashboard on the same dataset

---

## 📈 10. Power BI Dashboard

[Clickable Text](https://app.fabric.microsoft.com/groups/4a139114-0d80-4761-9e52-16e8c93201b3/reports/8185df62-aa77-4b7f-8ea9-69b7736a05e1/a80780ae693e69710345?redirectedFromSignup=1&experience=power-bi)
<img width="1781" height="832" alt="image" src="https://github.com/user-attachments/assets/b1a6b38b-2b3f-4870-b2e9-a45b16e2c059" />


**Planned KPIs:** Total Orders · Total Revenue · Average Order Value · Average Delivery Time · Orders per Restaurant

---

## 📊 11. Excel Analysis

The same full Swiggy dataset (fact + all dimension tables) was analyzed independently in **Microsoft Excel** to demonstrate self-service BI alongside the enterprise-grade Fabric pipeline:

- **Pivot Tables** across orders, restaurants, dishes, and locations to summarize order volume, ratings, and pricing trends
- **Pivot Charts** to visualize key trends (e.g. orders by city, top-rated dishes, monthly order trends)
- **Interactive Dashboard** using **slicers** (date, city, restaurant, dish category) for dynamic, self-service filtering

📁 File: `excel/swiggy_excel_dashboard.xlsx`

---

## 🧾 12. Key Learnings

- Microsoft Fabric unifies data engineering, warehousing, and BI into a single SaaS platform, replacing separate ADF/ADLS/SQL Server/Power BI tool chains
- Delta/Parquet format enables ACID-compliant, structured storage inside Lakehouses
- Fabric Data Pipelines provide a repeatable, schedulable way to move and validate data across Lakehouse and Warehouse layers
- Star schema design (fact + dimension tables) is essential for clean, performant Power BI semantic models
- Working with a 197K+ row fact table highlighted the importance of validation checkpoints at every stage of the pipeline (source → Lakehouse → Warehouse)

---

## 📄 13. License

This project is for educational/portfolio purposes. Sample data is illustrative and not affiliated with or sourced from the actual Swiggy platform.

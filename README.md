# Chinook Database Analysis with SQL & Jupyter

A comprehensive exploratory data analysis (EDA) and business intelligence project using SQL within a Jupyter Notebook to inspect schema structure, map relational dependencies, and evaluate global sales metrics on the **Chinook** database.

---

## 📌 Project Overview

This notebook connects directly to a local MySQL instance using `ipython-sql` and PyMySQL. It performs basic schema discovery, relational database inspection, row/column aggregations, and business-focused analytical queries.

### Key Analytical Objectives
* **Schema Inspection:** Discover table structures, column definitions, data types, and row count distributions across all 11 tables in the dataset.
* **Entity Relationship Mapping:** Query system metadata (`information_schema`) to isolate primary key–foreign key relationships.
* **Filtered & Grouped Analytics:** Answer business questions using `SELECT`, `WHERE`, `GROUP BY`, `HAVING`, and `ORDER BY`.
* **Market & Outlier Analysis:** Identify high-value global markets outside the US and perform outlier/fraud detection on customer purchasing behavior.

---

## 🛠️ Technology Stack & Dependencies

* **Environment:** Jupyter Notebook / Python 3
* **Database Management System:** MySQL
* **Database Driver & Extensions:** `ipython-sql`, `pymysql`
* **Target Schema:** `chinook` dataset (11 relational tables)

---

## 📂 Notebook Structure & Highlights

| Section | Focus | Description |
| :--- | :--- | :--- |
| **1. Database Setup** | Connection | Initializes SQL extensions and connects via PyMySQL connection string. |
| **2. Schema Understanding** | Metadata Analysis | Runs `SHOW TABLES;`, `DESCRIBE` statements, and inspects key constraints. |
| **3. FK Mapping** | Information Schema | Joins `KEY_COLUMN_USAGE` to list child tables and referenced primary keys. |
| **4. Table Metrics** | Row/Column Counts | Joins `TABLES` and `COLUMNS` in `information_schema` to calculate table scale. |
| **5. Query Practice** | Filtering & Aggregation | Analyzes orders, track duration, genre distributions, and country metrics. |
| **6. Business Scenarios** | Advanced Analysis | **Challenge 1:** Non-US high-value global market analysis.<br>**Challenge 2:** Outlier & fraud detection query identifying orders where max purchase exceeds $3 \times \text{AVG}$. |

---

## ⚠️ Challenges Encountered & Solutions

### Challenge 1: Retrieving Table Scale Metrics Simultaneously
* **Issue:** Obtaining row counts and column counts across all 11 tables in a single output required querying metadata dynamically rather than writing separate `COUNT(*)` queries for every table.
* **Solution:** Executed a SQL query joining `information_schema.TABLES` with `information_schema.COLUMNS` on `TABLE_SCHEMA` and `TABLE_NAME`, aggregating column names via `COUNT(c.COLUMN_NAME)` grouped by table.

### Challenge 2: Mapping Parent-Child Table Dependencies
* **Issue:** Understanding foreign key constraints across the 11 interconnected tables without external visual schema diagrams.
* **Solution:** Queried `information_schema.KEY_COLUMN_USAGE` filtering for `REFERENCED_TABLE_NAME IS NOT NULL` to extract child table foreign keys alongside parent primary key references.

### Challenge 3: Isolating Outlier Customer Purchases (Fraud Detection)
* **Issue:** Flagging customer profiles where a single large transaction heavily skews overall lifetime value statistics required combining aggregate functions with threshold mathematical logic on grouped data.
* **Solution:** Utilized a `HAVING` clause evaluating `MAX(Total) > 3 * AVG(Total)` on grouped `CustomerID` records to isolate single-order spikes.

---

## 🚀 How to Run

1. **Start MySQL Server** and import the `chinook` database.
2. **Install Required Libraries:**
   ```bash
   pip install ipython-sql pymysql

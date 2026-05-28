# 🛒 Walmart E-Commerce Sales ETL Pipeline

An end-to-end **ETL (Extract, Transform, Load) data pipeline** built on Walmart grocery sales data, designed to prepare clean, analysis-ready datasets and compute monthly average sales — with a focus on the impact of public holidays on weekly sales performance.

> **Context:** Real-world project completed on [DataCamp](https://www.datacamp.com). Data provided by DataCamp as part of the project environment.

---

## 📌 Table of Contents
- [Problem Statement](#problem-statement)
- [Pipeline Overview](#pipeline-overview)
- [Key Results](#key-results)
- [Dataset](#dataset)
- [Project Structure](#project-structure)
- [How to Run](#how-to-run)
- [Tech Stack](#tech-stack)

---

## Problem Statement

Walmart's e-commerce division reached **$80 billion in sales by end of 2022**, representing 13% of total revenue. Public holidays — Super Bowl, Labour Day, Thanksgiving, and Christmas — significantly influence weekly sales patterns.

This project builds a data pipeline that:
1. Merges raw grocery sales transaction data with external economic and store features
2. Cleans and filters the data for downstream analysis
3. Aggregates average weekly sales by month
4. Validates outputs by loading and inspecting the final CSV files

---

## Pipeline Overview

```
grocery_sales (SQL table) + extra_data.parquet
            │
            ▼
      extract()          Merge on index key → 231,522 rows, 17 columns
            │
            ▼
      transform()        Select key columns, extract Month, fill nulls,
                         filter Weekly_Sales > $10,000 → 106,193 rows
            │
            ▼
  avg_weekly_sales_per_month()    Group by Month → mean Weekly_Sales
            │
            ▼
        load()           Save clean_data.csv + agg_data.csv
            │
            ▼
      validation()       Reload and inspect both output files
```

---

## Key Results

### Monthly Average Weekly Sales

| Month | Avg Weekly Sales ($) |
|---|---|
| January | 33,174.18 |
| February | 34,342.44 |
| March | 33,227.31 |
| April | 33,414.78 |
| May | 33,339.89 |
| June | 34,582.47 |
| July | 33,930.77 |
| August | 33,644.79 |
| September | 33,266.59 |
| October | 32,736.99 |
| **November** | **36,594.03** |
| **December** | **39,248.98** |

**November and December are the strongest sales months** — driven by Thanksgiving and Christmas holiday demand. December average weekly sales ($39,248) are ~20% higher than October ($32,736), the weakest month.

### Cleaned Dataset Summary

| Metric | Weekly Sales ($) |
|---|---|
| Records after filtering | 106,193 |
| Mean | 34,213.69 |
| Median | 24,194.59 |
| Max | 693,099.36 |
| Std Dev | 28,495.09 |

The large gap between mean ($34K) and median ($24K) indicates a right-skewed distribution — a small number of high-performing departments during holiday weeks pull the average up significantly.

---

## Dataset

| File | Description | Rows | Included |
|---|---|---|---|
| `grocery_sales.csv` | Raw source — Store ID, Date, Dept, Weekly Sales | 231,522 | ✅ |
| `extra_data.parquet` | Economic features — Temperature, Fuel Price, CPI, Unemployment, Store Type/Size | 231,522 | ✅ |
| `clean_data.csv` | Pipeline output — transformed & filtered data | 106,193 | ✅ |
| `agg_data.csv` | Pipeline output — monthly avg weekly sales | 12 | ✅ |

**Columns in cleaned dataset:** `Store_ID`, `Dept`, `IsHoliday`, `Weekly_Sales`, `CPI`, `Unemployment`, `Month`

> The raw grocery sales data is sourced from Walmart's publicly referenced sales dataset, provided via the DataCamp project environment.

---

## Project Structure

```
walmart-etl-pipeline/
├── notebook.ipynb          # Full ETL pipeline with SQL + Python
├── grocery_sales.csv       # Raw input — store transactions (231,522 rows)
├── extra_data.parquet      # Economic features (input data)
├── clean_data.csv          # Transformed output — full cleaned dataset
├── agg_data.csv            # Aggregated output — monthly avg sales
├── walmartecomm.jpg        # Project header image
└── README.md
```

---

## How to Run

### Option A — DataCamp Workspace (original environment)

This project was built in DataCamp's integrated notebook environment with a pre-loaded SQL database. To run it as-is, open it within a DataCamp Workspace where `grocery_sales` is available as a SQL table.

### Option B — Local (Jupyter)

```bash
pip install pandas pyarrow jupyter
jupyter notebook notebook.ipynb
```

> For local runs, replace the SQL cell with:
> ```python
> import pandas as pd
> grocery_sales = pd.read_csv('grocery_sales.csv')
> ```

---

## Tech Stack

| Tool | Role |
|---|---|
| **Python 3** | Pipeline logic |
| **Pandas** | Data transformation, aggregation, I/O |
| **SQL** | Raw data extraction from `grocery_sales` table |
| **PyArrow / Parquet** | Reading economic feature data |
| **DataCamp Workspace** | Integrated notebook + SQL environment |

---

## Author

**Megha Kanojia**  
Completed via DataCamp Real-World Projects

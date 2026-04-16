# 🏭 RetailX — Microsoft Fabric Workshop

> **Build a complete data platform on Microsoft Fabric in 3 hours — from raw CSV to live Power BI dashboard — using only a browser and a free 60-day trial.**

[![GitHub Pages](https://img.shields.io/badge/Live%20Labs-GitHub%20Pages-blue?style=flat-square&logo=github)](https://amrgnnegm.github.io/retailx-fabric-workshop/)
[![License: MIT](https://img.shields.io/badge/License-MIT-green?style=flat-square)](LICENSE)
[![Microsoft Fabric](https://img.shields.io/badge/Microsoft-Fabric-0078D4?style=flat-square&logo=microsoft)](https://app.fabric.microsoft.com)

---

## 🎯 What you will build

| Layer | Tables | Tool |
|---|---|---|
| 🥉 Bronze | `raw_sales`, `raw_countries` | Dataflow Gen2 + PySpark Notebook |
| 🥈 Silver | `silver_sales`, `silver_countries` | PySpark Notebooks |
| 🥇 Gold | `fact_sales`, `dim_date`, `dim_customer`, `dim_product`, `dim_geography` | PySpark Notebook |
| 💎 Platinum | `SM_RetailX`, `RPT-RetailX-Overview` | Power BI DirectLake |

**Data sources used (both free, no authentication):**
- 📁 [Superstore Sales CSV](https://raw.githubusercontent.com/plotly/datasets/master/superstore.csv) — 9,994 rows × 21 columns (GitHub/Plotly)
- 🌐 [REST Countries API](https://restcountries.com/v3.1/all) — 250 countries worldwide

---

## 🚀 Quick Start

**Requirements:** A Microsoft account + free Fabric trial (60 days, no credit card)

1. Go to [app.fabric.microsoft.com](https://app.fabric.microsoft.com) → activate free trial
2. Open the workshop: **[Start here →](https://amrgnnegm.github.io/retailx-fabric-workshop/)**
3. Follow Labs 1–5 in order (3 hours total)

---

## 📚 Workshop Structure

| Section | Topic | Duration |
|---|---|---|
| [Section 1](section_01_prerequisites.html) | Prerequisites & References | Pre-session |
| [Section 2](section_02_architecture.html) | Platform Architecture | 15 min |
| [Lab 1](section_03_lab1.html) | Lakehouse & Dataflow Gen2 | 30 min |
| [Lab 2](section_04_lab2.html) | Bronze Layer: REST API Notebook | 30 min |
| [Lab 3](section_05_lab3.html) | Silver Layer & Data Pipeline | 25 min |
| [Lab 4](section_06_lab4.html) | Gold Layer: Star Schema | 25 min |
| [Lab 5](section_07_lab5.html) | Platinum: Power BI DirectLake | 20 min |
| [Cheat Sheet](section_08_cheatsheet.html) | Instructor Reference | Always open |

---

## 🧱 Fabric Features Covered

| Feature | Used in |
|---|---|
| Workspace & Trial Capacity | Lab 1 |
| Lakehouse + OneLake | Labs 1–5 |
| Dataflow Gen2 (Power Query M) | Lab 1 |
| PySpark Notebooks (4 notebooks) | Labs 2–4 |
| Data Pipeline + DAG orchestration | Lab 3 |
| Delta Table format | Labs 2–4 |
| SQL Analytics Endpoint | Lab 4 |
| DirectLake Semantic Model | Lab 5 |
| Power BI Report + 8 DAX measures | Lab 5 |
| DQ checks inside notebooks | Labs 2–4 |

---

## 🏗️ Architecture

```
GitHub CSV  ──DFG2──────────────►  🥉 Bronze  ──NB-02──►  🥈 Silver  ──NB-03──►  🥇 Gold  ──DirectLake──►  💎 Power BI
REST API    ──NB-01─────────────►  (raw_*)              (silver_*)            (dim_* + fact_*)          SM_RetailX
                                         └────────── PL-RetailX-Daily (Pipeline DAG) ─────────────┘
```

**Medallion layers:**
- **Bronze** — Exact copy of source. Raw JSON saved to `Files/bronze/`. Immutable.
- **Silver** — Cleaned, typed, snake_case columns, derived business metrics.
- **Gold** — Star schema with surrogate keys. Optimized for Power BI.
- **Platinum** — DirectLake semantic model + executive report. Zero-copy, zero-refresh-lag.

---

## 📓 Notebooks

| Notebook | Input | Output | Key concepts |
|---|---|---|---|
| `NB-01-BRONZE-Countries-API` | REST API | `raw_countries` | requests, mssparkutils, parse(), DQ assert |
| `NB-02-SILVER-Sales` | `raw_sales` | `silver_sales` | F.when(), datediff, derived columns |
| `NB-02-SILVER-Countries` | `raw_countries` | `silver_countries` | F.coalesce(), trim, dropDuplicates |
| `NB-03-GOLD-StarSchema` | silver_* | dim_* + fact_sales | Window, row_number(), SQL sequence, LEFT JOIN |

---

## 📐 DAX Measures (Lab 5)

```dax
Total Sales      = SUM(fact_sales[sales])
Total Profit     = SUM(fact_sales[profit])
Profit Margin %  = IF(ISBLANK([Total Sales]), BLANK(), ROUND(DIVIDE([Total Profit],[Total Sales])*100,1))
Total Orders     = DISTINCTCOUNT(fact_sales[order_id])
Avg Order Value  = DIVIDE([Total Sales],[Total Orders],BLANK())
Total Quantity   = SUM(fact_sales[quantity])
Sales YoY %      = CALCULATE([Total Sales], DATEADD(dim_date[date],-1,YEAR))  -- simplified
Avg Days to Ship = ROUND(AVERAGE(fact_sales[days_to_ship]),1)
```

---

## ✅ Expected Benchmark Values

After completing all labs, your Power BI report should show:

| Measure | Expected Value |
|---|---|
| Total Sales | ~$2.30M |
| Total Profit | ~$286K |
| Profit Margin % | ~12.5% |
| Total Orders | ~5,009 |
| Avg Days to Ship | ~3.9 days |
| Sales YoY % (2016) | +29.5% |

---

## 🗂️ Naming Conventions

| Item | Pattern | Example |
|---|---|---|
| Workspace | `{Project}-Analytics` | `RetailX-Analytics` |
| Lakehouse | `LH_{Project}` | `LH_RetailX` |
| Notebook | `NB-{seq}-{LAYER}-{Name}` | `NB-01-BRONZE-Countries-API` |
| Pipeline | `PL-{Project}-{Schedule}` | `PL-RetailX-Daily` |
| Bronze tables | `raw_{entity}` | `raw_sales` |
| Silver tables | `silver_{entity}` | `silver_sales` |
| Gold fact | `fact_{process}` | `fact_sales` |
| Gold dim | `dim_{entity}` | `dim_date` |
| Semantic model | `SM_{Project}` | `SM_RetailX` |
| Report | `RPT-{Project}-{Page}` | `RPT-RetailX-Overview` |

---

## 🙏 Credits & References

- **Dataset:** [Superstore Sales — Plotly Datasets](https://github.com/plotly/datasets)
- **API:** [REST Countries API](https://restcountries.com) — free, open, no auth required
- **Platform:** [Microsoft Fabric](https://learn.microsoft.com/en-us/fabric/) — free 60-day trial
- **Workshop author:** Amr Negm — Senior Expert Data Analytics Engineer, Bank Albilad

---

## 📄 License

MIT License — free to use, share, and modify with attribution.

---

*Built with ❤️ for the data engineering community. If this helped you, please ⭐ the repo.*

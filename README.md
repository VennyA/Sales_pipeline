## 🔷 Executive Summary

This project analyzes 8,800 B2B hardware sales transactions recorded between October 2016 and December 2017 to help sales leadership gain visibility into pipeline performance, rep productivity, and deal conversion.

The sales leadership team lacked a centralized view of which pipeline stages were underperforming, which reps needed coaching, and which products were driving wins. Using PostgreSQL for exploratory data analysis and Power BI for interactive dashboard development, a four-page dashboard was built to answer the key questions.


## 🔷 Project Overview

| Detail | Description |
|--------|-------------|
| Industry | B2B Computer Hardware |
| Domain | Sales & Revenue Operations |
| Tools | PostgreSQL, Power BI |
| Dataset | Maven Analytics — CRM Sales Pipeline |
| Timeline | October 2016 — December 2017 |

This project simulates a real-world RevOps analyst scenario where sales leadership needs a centralized view of pipeline health. The analysis covers four interconnected tables — accounts, products, sales teams, and pipeline 
deals. To show insights around deal velocity, stage conversion, rep performance, and revenue concentration.

Data Source
Source: Maven Analytics — CRM Sales Pipeline 
Dataset type: Transactional 
Time period: October 2016 — December 2017
Raw records: 541,909 rows and 8 columns
Cleaned records: ~396,470 rows (after removing nulls,cancellations, and invalid entries)
Unique accounts: 4,334 unique Customers
Columns: InvoiceNo, StockCode, Description, Quantity, InvoiceDate, UnitPrice, CustomerID, Country
Data Limitations: -Originally a B2C dataset, adapted here for B2B analysis -25% of records removed due to missing CustomerIDs -Only one year of data, so long-term behaviour is limited

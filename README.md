##  Executive Summary

This project analyzes 8,800 B2B hardware sales transactions recorded between October 2016 and December 2017 to help sales leadership gain visibility into pipeline performance, rep productivity, and deal conversion.

The sales leadership team lacked a centralized view of which pipeline stages were underperforming, which reps needed coaching, and which products were driving wins. Using PostgreSQL for exploratory data analysis and Power BI for interactive dashboard development, a four-page dashboard was built to answer the key questions.


##  Project Overview

| Detail | Description |
|--------|-------------|
| Industry | B2B Computer Hardware |
| Domain | Sales & Revenue Operations |
| Tools | PostgreSQL, Power BI |
| Dataset | Maven Analytics — CRM Sales Pipeline |
| Timeline | October 2016 — December 2017 |

**Business Context:** This project simulates a real-world RevOps analyst scenario where sales leadership needs a centralized view of pipeline health. The analysis covers four interconnected tables — accounts, products, sales teams, and pipeline 
deals. To show insights around deal velocity, stage conversion, rep performance, and revenue concentration.

**Why This Project Matters:**  
Sales pipeline visibility is essential for revenue growth and operational efficiency. Without a centralized reporting system, sales leadership struggles to identify stalled deals, underperforming regions, low-converting stages, and revenue-driving products.


##  Data Source

The dataset was sourced from **Maven Analytics** as a free B2B hardware sales dataset. It contains four related tables with a total of 8,800 pipeline deal records spanning October 2016 to December 2017.

| Table | Rows | Columns | Description |
|-------|------|---------|-------------|
| sales_pipeline | 8,800 | 8 | Core fact table — deals, stages, dates, values |
| accounts | 85 | 7 | Company accounts linked to deals |
| products | 7 | 3 | Hardware products with pricing |
| sales_team | 35 | 3 | Sales agents, managers, and regions |

**Data Type:** Transactional B2B sales pipeline data

**Limitations:**
- 1,425 deals (16%) had no linked account and were removed to preserve data integrity
- No external revenue targets were available — pipeline coverage analysis was excluded
- October 2016 to February 2017 recorded no Won deals, limiting MoM variance for those months


**Why This Project Matters:**  
Sales pipeline visibility is essential for revenue growth and operational efficiency. Without a centralized reporting system, sales leadership struggles to identify stalled deals, underperforming regions, low-converting stages, and revenue-driving products.




##  Problem Statement

The sales leadership team at a B2B computer hardware company lacks visibility into pipeline performance, rep productivity, and deal conversion across quarters. Without a centralized view, leadership cannot accurately forecast revenue or identify where deals are being lost.

This analysis was designed to answer the following business questions:

- Which sales reps are underperforming?
- Which pipeline stages have the highest deal drop-off?
- Which products are driving the most revenue and wins?
- How long does it take to close a deal on average, and who is slowest?
- Which regions and managers are leading revenue performance?


##  Tools & Methodology

**Tools Used**

| Tool | Purpose |
|------|---------|
| PostgreSQL | Exploratory data analysis, data cleaning, quality checks |
| Power BI | Data modeling, DAX measures, interactive dashboard |

**Methodology**

### Data Collection
The dataset was downloaded from Maven Analytics as four CSV files — accounts, products, sales_team, and sales_pipeline and loaded into both PostgreSQL and Power BI for parallel analysis.

### Data Cleaning & Preparation
The following steps were performed to ensure data quality:
- Removed 1,425 deals with no linked account (16% of total records)
- Corrected inconsistent product name — "GTXPro" standardised to "GTX Pro"
- Validated opportunity_id as unique primary key — no duplicates found
- Confirmed deal_stage, regional_office, and sector had no formatting inconsistencies

### Data Modeling
A star schema was designed in Power BI with sales_pipeline as the fact table linked to three dimension tables:
- sales_pipeline → accounts (via account)
- sales_pipeline → products (via product)
- sales_pipeline → sales_team (via sales_agent)

A DimDate calendar table was created with an active relationship on close_date and an inactive relationship on engage_date.

### Calculated Fields & DAX Measures
Key measures developed in Power BI include:

| Measure | Description |
|---------|-------------|
| Revenue | Total close value for Won deals only |
| Win Rate | Won deals / (Won + Lost deals) × 100 |
| AOV | Revenue / Won deals |
| Avg Deal Velocity | Average days from engage_date to close_date |
| Deals Exceeding 52 Days | Count of deals exceeding the 52 day benchmark |
| MoM Variance | Month-on-month change for all key KPIs |


##  Exploratory Data Analysis (EDA)

EDA was conducted in both PostgreSQL and Microsoft Power BI before dashboard development to ensure the analysis was driven by actual data patterns, trends, and business behaviour rather than assumptions.

### Structure Check
- sales_pipeline confirmed as the fact table with 8,800 rows and 8 columns
- Three dimension tables: accounts (85 rows), products (7 rows), sales_team (35 rows)
- Tables joined on text fields: account, product, and sales_agent

### Quality Check

**Null Values (sales_pipeline)**

| Column | Null Count | Action |
|--------|-----------|--------|
| account | 1,425 | Removed — could not be linked to account attributes |
| engage_date | 500 | Retained — open deals expected to have no engage date |
| close_date | 2,089 | Retained — open deals have no close date |
| close_value | 2,089 | Retained — open deals have no close value |


**Inconsistent Formatting**
- "GTXPro" in sales_pipeline did not match "GTX Pro" in products table — corrected before analysis

**Duplicates**
- opportunity_id confirmed unique across all 8,800 rows — no duplicates found

### Distribution Check

| Metric | Value |
|--------|-------|
| Close Value (Min) | $0 |
| Close Value (Max) | $30,288 |
| Close Value (Avg) | $1,490 |
| Employees (Median) | 2,769 |
| Employees (Avg) | 4,660 |

The employee distribution is right-skewed, a small number of large enterprise accounts pull the average above the median.

### Key Patterns

| Metric | Finding |
|--------|---------|
| Win Rate | 63.15% |
| Loss Rate | 36.85% |
| Avg Deal Velocity (Won) | 51.78 days |
| Top Rep by Revenue | Darcel Schlecht — $1.15M |
| Deal Stage Distribution | Won: 4,238 — Lost: 2,473 — Engaging: 1,589 — Prospecting: 500 |

##  Key Insights

### 1. Pipeline is healthy but revenue is heavily concentrated
The pipeline generated $10M in Won revenue at a 63.15% win rate. However, Darcel Schlecht alone accounts for $1.15M more than double the next highest rep at $478,396. This level of concentration creates risk if a single agent leaves or underperforms.

### 2. GTX Pro drives disproportionate revenue
GTX Pro generated $3.5M — 35% of total pipeline revenue despite being one of seven products.

### 3. Deal velocity varies significantly across reps
The average deal velocity is 52 days for Won deals. Bottom performing reps range from 56 to 65 days all above the benchmark. Moses Frase recorded the slowest velocity at 65 days. Faster closing reps like Cecily Lampkin at 42 days suggest coaching opportunities exist to compress the sales cycle.

### 4. West region leads but all regions are competitive
The West region leads revenue at $3.6M, followed closely by Central at $3.3M and East at $3.1M. The gap between regions is narrow, suggesting no region is severely underperforming but West has a replicable advantage worth investigating.

### 5. 37% of closed deals are lost — Engaging is the critical stage
With 2,473 deals lost, the pipeline loses more than one in three closed deals. Deals in the Engaging stage represent 1,589 records still in progress the largest open stage. If conversion at Engaging improves even marginally, win rate and revenue could increase significantly.

### 6. April deal velocity anomaly flagged
April recorded an unusually low average deal velocity of 6 days across 285 won deals. While shorter sales cycles are typically positive, a 6-day close period is unusually low for B2B hardware sales, which often involve procurement reviews, demos, and approval processes. This suggests a potential data quality issue where engage_date and close_date may have been recorded on the same or near-identical dates. The April velocity data should therefore be reviewed before being used for operational or strategic decision-making.


##  Recommendations

### 1. Investigate and coach bottom performing reps
Agents in the bottom 5 by deal velocity range from 56 to 65 days — all above the 52 day benchmark. Sales managers should review their pipeline activity, identify where deals are stalling, and provide targeted coaching to compress the sales cycle. Bringing bottom reps closer to the 42 day velocity of top performers could meaningfully improve overall pipeline throughput.

### 2. Reduce revenue concentration risk
Darcel Schlecht accounts for $1.15M which is more than double the next highest agent. Leadership should investigate what makes Darcel's approach successful and replicate it across the wider team. Over-reliance on a single rep creates significant revenue risk if that rep leaves or underperforms.

### 3. Prioritise GTX Pro in sales strategy
GTX Pro generates $3.5M — 35% of total revenue. Sales leadership should ensure reps are adequately trained on GTX Pro, that it features prominently in outreach, and that deal support resources are allocated to maximise conversion on this product.

### 4. MG Special low revenue
MG Special is the second highest product by Won deals at 729, yet generates only $43,768 in revenue due to its low unit price.

### 5. Focus coaching on the Engaging stage
With 1,589 deals currently in the Engaging stage and 37% of closed deals lost, the pipeline has a significant conversion gap. Sales managers should audit deals stuck in Engaging, identify common objections, and develop targeted playbooks to improve conversion at this critical stage.



## Visuals Preview

-**Dashboard Screenshots**-

<img width="601" height="342" alt="image" src="https://github.com/user-attachments/assets/180bbd37-0a51-481a-9726-5fde2fd917fc" />

<img width="597" height="344" alt="image" src="https://github.com/user-attachments/assets/52457099-964b-47a9-9929-35e575b2c9c5" />

<img width="601" height="340" alt="image" src="https://github.com/user-attachments/assets/d620a07b-75c1-4a4c-b0a8-ffb9e0b68a85" />

<img width="608" height="335" alt="image" src="https://github.com/user-attachments/assets/40a60ee7-1d61-4b27-942e-99abe212f97b" />

-**Before and After Cleaning**-

<img width="779" height="441" alt="Sales pipeline SQL select from sales_pipeline" src="https://github.com/user-attachments/assets/6371b4b8-7b0c-4106-b1db-de2deda76f17" />


<img width="783" height="416" alt="Screenshot 2026-05-09 161036" src="https://github.com/user-attachments/assets/50ead56d-7e59-47eb-a2e7-8bbeab63bfea" />


-**SQL Query**-

<img width="779" height="430" alt="Screenshot 2026-05-09 160740" src="https://github.com/user-attachments/assets/901e55db-efda-4e4a-ad75-339221624abc" />


-**Data Model Diagram**-
<img width="877" height="329" alt="Sales pipeline data modelling" src="https://github.com/user-attachments/assets/bc5e29b2-4f0a-4611-820e-a0b8dd5c752a" />




##  Limitations

While this analysis provides meaningful insights into pipeline performance, the following limitations must be acknowledged:

- **Missing account data** — 1,425 deals (16% of total records) had no linked account information and were removed from the analysis. As a result, account-level insights are based on 84% of total pipeline activity and may not fully represent overall revenue performance.

- **No revenue targets available** — The dataset does not include quarterly or annual revenue targets, making pipeline coverage analysis impossible. Recommendations around target attainment are therefore excluded.

- **October to February anomaly** — No Won deals were recorded from October 2016 till February 2017, which limits MoM variance analysis for those months and may indicate incomplete data for that period.

- **April velocity anomaly** — April 2017 recorded an average deal velocity of 6 days across 285 Won deals, which is unrealistically low for B2B hardware sales. This is flagged as a likely data quality issue and excluded from 
velocity-based coaching recommendations.

- **Static dataset** — The dataset covers a fixed period from October 2016 to December 2017 and does not reflect current market conditions or ongoing pipeline activity. Findings should be treated as historical analysis rather 
than live performance monitoring.

- **No external factors** — Variables such as competitor activity, economic conditions, or marketing campaigns are not captured in the dataset and may have influenced pipeline performance during the analysis period.



##  Conclusion

This analysis provides a structured evaluation of sales pipeline performance, rep productivity, deal velocity, and revenue concentration for a B2B computer hardware company covering October 2016 to December 2017.

The pipeline is fundamentally healthy — a 63.15% win rate and $10M in Won revenue demonstrate that the sales team is converting effectively. However, the analysis reveals three critical areas requiring leadership attention:

- **Revenue concentration risk** — Darcel Schlecht accounts for $1.15M, more than double any other rep, creating over-reliance on a single performer.

- **Product revenue imbalance** — GTX Pro drives 35% of total revenue while MG Special generates only $43,768 despite being the second highest product by deal volume, signalling a pricing strategy gap.

- **Deal velocity gap** — Bottom performing reps take up to 65 days to close deals against a 52 day benchmark, representing a coaching and process improvement opportunity.


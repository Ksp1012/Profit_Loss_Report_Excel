# Profit and Loss Report Analysis

This repository contains a detailed Profit and Loss (P&L) report analysis for AtliQ Hardware. The report is generated using data from various sources, processed and analyzed in Excel using Power Query, Power Pivot, and Pivot Tables. The analysis includes calculations of Net Sales, Cost of Goods Sold (COGS), Gross Margin, and other key financial metrics for fiscal years 2019, 2020, and 2021.

## Project Overview

The purpose of this project is to provide an in-depth understanding of the financial performance of AtliQ Hardware by analyzing key metrics on a country-wise and monthly basis. The project integrates and processes data from multiple datasets and generates insights into fiscal trends, including Gross Margin percentage, quarterly performance, and year-over-year growth.

## Datasets

### 1. **dim_customer**
   - Details about customers:
     - `customer_code`
     - `customer_name` (e.g., company name)
     - `market` (e.g., country)
     - `platform` (e.g., Brick & Mortar, E-commerce)
     - `channel` (e.g., Distributor, Direct, Retailer)

### 2. **dim_market**
   - Information about markets:
     - `market` (e.g., country)
     - `subzone` (e.g., NE, NAN, APAC)
     - `region` (e.g., North America, Europe, Asia-Pacific)

### 3. **dim_product**
   - Product-related details:
     - `product_code`
     - `division` (e.g., N&S, PC, P&A)
     - `segment` (e.g., Accessories, Networking, Desktop)
     - `category`, `product`, `variant`

### 4. **fact_sales_monthly_with_cost**
   - Enhanced sales data:
     - `date`
     - `product_code`
     - `customer_code`
     - `QTY`
     - `net_sales_amount`
     - **New Columns**: `freight_cost`, `manufacturing_cost`

### 5. **ns_target_2021**
   - Sales targets for 2021:
     - `market` (e.g., country)
     - `date`
     - `ns_target`

### 6. **dim_date**
   - Fiscal calendar details:
     - `date`
     - `month` (e.g., "MMM" format)
     - `month_FY` (Fiscal year month: e.g., September = 1, October = 2)
     - `quarter` (e.g., Q1, Q2)

## Data Preparation

### ETL Process
1. **Data Extraction**:
   - Imported all datasets into Power Query.
   - Updated the source of `fact_sales_monthly` to `fact_sales_monthly_with_cost`.

2. **Data Transformation**:
   - Created a `new_date_modified` column for standardizing date formats.
   - In `fact_sales_monthly`, added a column for **COGS**:
     ```
     COGS = freight_cost + manufacturing_cost
     ```
   - Enhanced `dim_date` with:
     - `month` (formatted as "MMM")
     - `month_FY` (fiscal year month calculation)
     - `quarter` (calculated as: `"Q" & ROUNDUP([month_FY] / 3, 0)`)

3. **Data Loading**:
   - Loaded the transformed data into Power Pivot for analysis.

### Relationships in Power Pivot
- **dim_customer** ➡ fact_sales_monthly (`customer_code`)
- **dim_product** ➡ fact_sales_monthly (`product_code`)
- **dim_date** ➡ fact_sales_monthly (`new_date_modified`)
- **dim_date** ➡ ns_target_2021 (`date`)
- **dim_market** ➡ ns_target_2021 (`market`)
- **dim_market** ➡ dim_customer (`market`)

## Measures and Calculations

- **Net Sales**: `SUM(fact_sales_monthly[net_sales_amount])`
- **COGS**: `SUM(fact_sales_monthly[COGS])`
- **Gross Margin**: `[Net Sales] - [COGS]`
- **Gross Margin Percentage (GM%)**: `DIVIDE([Gross Margin], [Net Sales], 0)`

### Key Concepts
- **Net Sales**: The total revenue generated from product sales.
- **Manufacturing Cost**: The cost incurred in producing goods.
- **Freight Cost**: The expense of transporting goods to customers.
- **Cost of Goods Sold (COGS)**: The combined cost of manufacturing and freight.
- **Gross Margin**: The difference between Net Sales and COGS, representing profitability.
- **GM%**: The percentage of sales retained after covering COGS.

## Reports

### 1. **Country-wise P&L Report**
   - Analyzes Net Sales, COGS, Gross Margin, and GM% for each country across fiscal years.
   - Includes year-over-year growth rates.

### 2. **Monthly P&L Report**
   - Breaks down financial performance by month and quarter for fiscal years 2019, 2020, and 2021.

### Sample Data
Key insights from the report (all values in USD):
- **India (2021)**:
  - Net Sales: 161.3M
  - COGS: 109.7M
  - Gross Margin: 51.6M
  - GM%: 32%
- **USA (2021)**:
  - Net Sales: 87.8M
  - COGS: 55.3M
  - Gross Margin: 32.5M
  - GM%: 37%

## Getting Started

1. Open Power Query to view the ETL process.
2. Review Power Pivot for relationships and calculated columns.
3. Explore the Pivot Tables for P&L insights.

## Prerequisites

- Microsoft Excel with Power Query and Power Pivot enabled.
- Basic understanding of ETL processes and Pivot Table analysis.

## Contributing

Feel free to fork this repository, submit issues, or propose improvements to the analysis or visualizations.



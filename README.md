# Profit and Loss Report Analysis

This repository contains a detailed analysis of AtliQ Hardware's Profit and Loss (P&L) report. The project uses data processing tools like Power Query, Power Pivot, and Pivot Tables in Excel to extract, transform, and load data for analysis. The report provides insights into financial performance across fiscal years (FY) 2019, 2020, and 2021, focusing on key metrics such as Net Sales, COGS, Gross Margin, and GM%.

## Project Overview

The objective of this project is to analyze AtliQ Hardware's financial performance by preparing a P&L report at the country, monthly, and quarterly levels. It includes detailed data transformations, relationships between datasets, and calculated measures for better decision-making.

## Datasets and ETL Process

### 1. **dim_customer**
- **Purpose**: Provides customer details.
- **Columns**: `customer_code`, `customer_name`, `market`, `platform`, `channel`.
- **ETL**: Cleaned inconsistencies in `customer_name`, loaded into Power Pivot.

### 2. **dim_market**
- **Purpose**: Contains market details.
- **Columns**: `market`, `subzone`, `region`.
- **ETL**: Replaced `nan` with `NA`, loaded into Power Pivot.

### 3. **dim_product**
- **Purpose**: Contains product-related details.
- **Columns**: `product_code`, `division`, `segment`, `category`, `product`, `variant`.
- **ETL**: Loaded directly into Power Pivot.

### 4. **fact_sales_monthly_with_cost**
- **Purpose**: Enhanced monthly sales data with cost details.
- **Columns**: `date`, `product_code`, `customer_code`, `QTY`, `net_sales_amount`, `freight_cost`, `manufacturing_cost`.
- **ETL**: 
  - Standardized dates using a `new_date_modified` column.
  - Added a calculated column for `COGS = freight_cost + manufacturing_cost`.
  - Replaced `fact_sales_monthly` with this dataset.

### 5. **ns_target_2021**
- **Purpose**: Annual sales targets for 2021.
- **Columns**: `market`, `date`, `ns_target`.
- **ETL**: Loaded directly into Power Pivot.

### 6. **dim_date**
- **Purpose**: Provides fiscal calendar details.
- **Columns**: `date`, `month`, `month_FY`, `quarter`.
- **ETL**: 
  - Added `month` (`FORMAT([date], "MMM")`).
  - Added `month_FY` (`MONTH(DATE(YEAR([date]), MONTH([date]) + 4, 1))`).
  - Added `quarter` (`"Q" & ROUNDUP([month_FY] / 3, 0)`).

## Relationships in Power Pivot

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

## Key Financial Concepts

- **Net Sales**: Revenue from product sales, excluding costs.
- **Freight Cost**: Expenses incurred for shipping goods.
- **Manufacturing Cost**: Costs associated with producing goods.
- **Cost of Goods Sold (COGS)**: Total cost incurred (freight + manufacturing) for sold products.
- **Gross Margin**: Profit remaining after deducting COGS from Net Sales.
- **GM%**: Percentage of Net Sales retained as profit after covering COGS.

## Reports

### 1. **Country-wise P&L Report**
   - Tracks Net Sales, COGS, Gross Margin, and GM% by country for fiscal years.
   - Highlights year-over-year growth rates.

### 2. **Monthly P&L Report**
   - Tracks Net Sales, COGS, and Gross Margin on a monthly and quarterly basis for FY 2019, 2020, and 2021.

## Getting Started

1. Open Power Query to review the ETL processes.
2. Use Power Pivot to explore relationships and calculated measures.
3. Explore Pivot Tables for insights into P&L performance.

## Prerequisites

- Microsoft Excel with Power Query and Power Pivot enabled.
- Basic understanding of ETL processes and financial metrics.



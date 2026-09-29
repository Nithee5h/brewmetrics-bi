# BrewMetrics BI

A version-controlled Power BI analytics solution for BrewMetrics Coffee Co.

## Project Overview
BrewMetrics Coffee Co. operates Flagship stores, Kiosks, and Drive-Thrus across four cities. This project analyzes revenue, units sold, month-over-month performance, cumulative sales, product performance, city-level performance and Cold Brew seasonality.

## Data Source
`data/brewmetrics_sales.csv`

Fields include:
- sale_id
- date
- city
- store_format
- category
- item
- quantity
- unit_price
- sales_amount

The final dashboard uses the April-June reporting period.

## Data Model
The solution uses a star schema with:
- `Fact_Sales`
- `Dim_Date`
- `Dim_City`
- `Dim_Product`

Relationships:
- `Dim_Date[Date]` → `Fact_Sales[date]`
- `Dim_City[City]` → `Fact_Sales[city]`
- `Dim_Product[Item]` → `Fact_Sales[item]`

## DAX Measures
- Total Sales
- Total Quantity
- Transaction Count
- Average Transaction Value
- Previous Month Sales
- Month-over-Month Growth %
- Running Total Sales
- City Rank

The four assignment measures are:
1. Month-over-Month Growth %
2. Running Total Sales
3. City Rank using `RANKX`
4. Average Transaction Value

## Dashboard
The dashboard contains:
- Total Revenue KPI
- Units Sold KPI
- Month-over-Month Growth KPI
- Average Transaction Value KPI
- Revenue by Product
- Revenue by City
- Monthly Sales Trend
- Running Total Sales
- Cold Brew Sales by Month
- Category slicer
- City slicer
- Month slicer
- City → Store Format drill-down

## Key Insights
1. Bengaluru is the strongest-performing city in the dataset.
2. Cold Brew performs strongly in April and May before declining in June.
3. Overall sales increase from April to May and decline in June.

## Repository Contents
- `BrewMetrics.pbix`
- `data/brewmetrics_sales.csv`
- `README.md`
- `NOTES.md`
- `REFLECTION.md`
- `DAX_Formulas.txt`
- `dashboard/BrewMetrics_Dashboard.pdf`

## Tools Used
Power BI Web, Power BI Service, Power Query, DAX, GitHub and GitHub Copilot.

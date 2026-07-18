# Shopify Sales Analytics — Power BI Project

An end-to-end Power BI dashboard analyzing 24 months of e-commerce sales data (Jan 2024–Dec 2025): revenue trends, customer retention, marketing channel ROI, and product performance. Built as a "senior executive lens" — every page answers a specific business question in a handful of numbers, no row-by-row digging required.

## Business questions answered

- Is revenue growing, and where is it concentrated? (customers, products, geography)
- Are we too dependent on a small set of customers?
- Which marketing channels actually drive revenue, and at what average order value?
- Is growth coming from new customers or repeat buyers?
- Where is operational risk — refunds, cancellations, pending orders?

## Key findings (from the data)

- **$89,858.68** total revenue across **420 orders** ($213.95 AOV), with a **6.9% refund rate**
- Top 20% of customers (32 people) generate **40.6%** of total revenue — moderate concentration
- **USA** leads by country (37.9% of revenue), followed by Canada and Australia
- **Organic traffic dominates** ($45,274.52) over paid; among paid channels, Instagram Ads leads revenue but Google Ads has the highest AOV ($235.05)
- Business is **heavily retention-driven**: returning customers drive $87,089.37 vs. $2,769.31 from new customers — a flag if new-customer acquisition is a growth priority
- **Apparel** is the top category (27.2% of revenue); Electronics Accessories is the weakest and a candidate for review

*(Full set of 10+ executive DAX queries with results is in [`Executive_Data_Dictionary.md.pdf`](./PowerBIShopifyProject.Report/Executive_Data_Dictionary.md.pdf).)*

## Report pages

| Page | Purpose |
|---|---|
| Navigation & Summary | Landing page / nav hub |
| Executive Overview | Top-line KPIs, revenue trend, targets |
| Orders | Order volume, fulfillment status, AOV |
| Product & Customer Insights | Category/product performance, customer segments |
| Top 5 Products Revenue Contribution | Pareto view of best sellers |
| Marketing / Ad Performance | Channel attribution, ad spend ROI |
| Tooltip / Ad_Tooltip / Organic_Tooltip | Custom hover tooltips for drill-down context |

## Data model

Star schema with one fact table and four dimensions:

- **Fact:** `OrderLines` (line-item grain: quantity, discount, line revenue)
- **Dimensions:** `Orders` (order-level attributes: status, channel, ad platform), `Customers`, `Products`, `Geography`, plus a dedicated `DateTable` for time intelligence

Relationships: `OrderLines → Orders → Customers → Geography`, `OrderLines → Products`, `Orders[order_date] → DateTable`.

## DAX highlights

A few measures that go beyond simple sums:

```dax
// Month-over-month revenue growth, using proper time intelligence
Revenue_MoM_Growth_Pct =
    VAR CurrentRevenue = [Total_Revenue]
    VAR PriorMonthRevenue = CALCULATE([Total_Revenue], DATEADD(DateTable[date], -1, MONTH))
    RETURN DIVIDE(CurrentRevenue - PriorMonthRevenue, PriorMonthRevenue)

// Revenue net of refunds/cancellations
Fulfilled_Revenue = CALCULATE([Total_Revenue], Orders[order_status] = "fulfilled")

// Each row's share of total revenue, ignoring active filters — for Pareto/contribution charts
Pct_of_Total_Revenue =
    DIVIDE([Total_Revenue], CALCULATE([Total_Revenue], ALL(Products), ALL(Orders), ALL(Customers)))

// Dynamic text box that updates based on slicer selection
Selected_Product_Comment = "Showing: " & [Selected_Product_Label] & " — " & FORMAT([Total_Revenue], "$#,##0") & " in revenue"
```

20+ measures total, organized into display folders (Marketing, Trend Measures, Product Insights, % Comparisons, Text Box Helpers) for a clean field list.

## Data

Synthetic Shopify-style order data ([`Shopify_Dummy_Data.xlsx`](./Shopify_Dummy_Data.xlsx)) generated to resemble a real DTC apparel store — used because production client data can't be shared publicly. Structure and analysis approach mirror real client engagements.

## Tools

Power BI Desktop (PBIP project format — text-based, git-friendly), DAX, Power Query.

## Opening this project

1. Requires Power BI Desktop with the **Power BI Project (.pbip)** preview feature enabled (Options → Preview features).
2. Open `PowerBIShopifyProject.pbip`.

## Screenshots

*(Add exported page images here — see note below.)*

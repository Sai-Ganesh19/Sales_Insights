# AtliQ Sales Insights Analysis (Power BI)

A Power BI solution for AtliQ Hardware to diagnose sales decline, understand profitability across markets and customer segments, and drive data-informed decisions.

---

## 📌 Problem Statement
The client, **AtliQ Hardware**, faced a continuous **sales decline** and lacked visibility into root causes. They needed a **data-driven** solution to:
- Analyze sales performance & trends
- Compare **profitability** across **markets** and **customer segments**
- Enable **drill-down** insights for faster, informed decisions

---

## ✅ Solution Overview
Built an **interactive Power BI Sales Insights dashboard** consolidating data from multiple sources with **three key pages**:
1. **Revenue Trend** — month-wise revenue & margin trends with slicers
2. **Sales by Market** — market-level performance and rankings
3. **Profitability (Market & Customer)** — margin %, contribution, and outliers

**Capabilities**
- Drill-down analysis by **market, region, and customer type** (e.g., Brick & Mortar vs E-Commerce)
- Consistent KPI definitions via **DAX measures**
- Clean star-schema data model for performance and scalability

---

## 🔍 Key Insights & Outcomes
- **Revenue decline identified:** from **₹26M+ (Feb 2020)** to **<₹14M (Jun 2020)**  
- **Top market:** **Delhi NCR** with **₹75.5M** revenue  
- **Segment highlight:** **Brick & Mortar ₹65.6M** vs **E-Commerce ₹29.1M**  
- **Profitability disparity:** a top customer drove **46.15% revenue** but only **0.4% profit margin**  
- **Regional strategy:** **Bhubaneswar** margin **10.5%** vs **Lucknow −2.7%** → focus on high-margin markets

## Outcomes
The project delivered a robust dashboard that improved reporting efficiency and enabled the client to:

Identify and address the root causes of the sales decline.

Shift focus from solely revenue-based sales to profitability-driven strategies.

Make informed decisions on resource allocation and pricing by understanding the true profitability of different markets and customer segments.

---

## 🧱 Data Model (Star Schema)
- **Fact Table**
  - `FactSales` (date_key, market_key, customer_key, product_key, qty, revenue, cost)
- **Dimensions**
  - `DimDate` (date, month, quarter, year, fiscal)
  - `DimMarket` (market, region, zone)
  - `DimCustomer` (customer, type: Brick & Mortar / E-Commerce)
  - `DimProduct` (category, subcategory, brand)

> Note: Rename columns to your dataset’s exact names as needed.

---

## 📐 Core DAX Measures (Examples)
> Adjust table/column names to match your model.

```DAX
Revenue := SUM(FactSales[revenue])

Cost := SUM(FactSales[cost])

Profit := [Revenue] - [Cost]

Profit Margin % := DIVIDE([Profit], [Revenue])

Sales Qty := SUM(FactSales[qty])

Revenue LY := CALCULATE([Revenue], DATEADD(DimDate[Date], -1, YEAR))

Profit Contribution % :=
DIVIDE([Profit], CALCULATE([Profit], ALL(DimMarket), ALL(DimCustomer)))

Top Customer Revenue % :=
VAR TotalRev = CALCULATE([Revenue], ALL(DimCustomer))
RETURN DIVIDE([Revenue], TotalRev)

Market Rank by Revenue :=
RANKX(ALL(DimMarket[Market]), [Revenue], , DESC)

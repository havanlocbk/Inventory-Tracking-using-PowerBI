# 📊 How to Optimize Inventory Levels – Supply Chain & Operations – Power BI

Author: Loc Ha

Date: 2025-06-18

Tools Used: SQL Server (AdventureWorks2022), Power Query, DAX, Power BI


---

## 📑 Table of Contents

1. [📌 Background & Overview](#-background--overview)
2. [📂 Dataset Description & Data Structure](#-dataset-description--data-structure)
3. [🧠 Design Thinking Process](#-design-thinking-process)
4. [📊 Key Insights & Visualizations](#-key-insights--visualizations)
5. [🔎 Final Conclusion & Recommendations](#-final-conclusion--recommendations)

---

## 📌 Background & Overview

### Objective

* Provide stakeholders with a comprehensive and easy-to-understand picture of warehouse inventory.
* Track and control inventory levels across multiple locations.
* Identify products that are out-of-stock, below reorder point, or at safety stock level.
* Support decision-making for warehouse managers, sales, and marketing teams.

### Stakeholders

✔️ Warehouse managers & staff – ensure inventory meets required thresholds.

✔️ Sales & marketing teams – align sales campaigns with available stock.

✔️ Decision-makers – optimize operations and reduce costs.

---

## 📂 Dataset Description & Data Structure

### Data Source

* **Source**: AdventureWorks2022 (imported from SQL Express).
* **Format**: Relational database (SQL Server).
* **Tables Used**: Product, ProductInventory, ProductCategory, ProductSubcategory, Sales, Calendar, Production tables.

### Data Model (ERD)

* The model connects product information with sales, inventory, and production data.
* Key tables: `Dim_Product`, `Dim_ProductInventory`, `Fact_Sales`, `Dim_Calendar`, `Production_WorkOrder`, `Production_Location`.
* Measures built in a dedicated `MeasuresTable` (Inventory KPIs).

<img width="2534" height="1555" alt="Screenshot 2025-09-06 094149" src="https://github.com/user-attachments/assets/05bfeaba-fd9e-4aa1-936b-37f83e2c5383" />


---

## 🧠 Design Thinking Process

1️⃣ **Empathize** – Understand warehouse managers’ and sales teams’ needs for monitoring stock levels.

2️⃣ **Define** – Problem: lack of visibility into stock status leads to overstocking or stockouts.

3️⃣ **Ideate** – Brainstorm KPIs: Total Inventory, Products Below Reorder Point, Out-of-Stock %, Turnover.

4️⃣ **Prototype & Review** – Build Power BI dashboards with key metrics, validate with stakeholders, refine.



---

## ⚒️ Main Process

1️⃣ **Data Cleaning & Preprocessing** – Use Power Query to transform AdventureWorks tables.

2️⃣ **Exploratory Data Analysis** – Review stock distribution by category, location, and time.

3️⃣ **DAX Measures** – Key examples:

```DAX
#Inventory Products = CALCULATE(DISTINCTCOUNT(Dim_ProductInventory[ProductID]), Dim_ProductInventory[Quantity] > 0)

Products Out-Of-Stock = CALCULATE(DISTINCTCOUNT(Dim_ProductInventory[ProductID]), Dim_ProductInventory[Inventory Status] = "Out of Stock")

Inventory Value = SUMX(Dim_ProductInventory, Dim_ProductInventory[Quantity] * RELATED(Dim_Product[ListPrice]))

Inventory Δ% by Month = VAR CurrentInventory = [Total Inventory Quantity]
VAR PrevInventory = CALCULATE([Total Inventory Quantity], DATEADD(Dim_Calendar[Date], -1, MONTH))
RETURN DIVIDE(CurrentInventory - PrevInventory, PrevInventory)
```
4️⃣ **Power BI Visualization** – Build 3 dashboards: Overview, Inventory vs Sales, Reorder Point Analysis.


---

## 📊 Key Insights & Visualizations

### 1️⃣ Inventory Overview Dashboard

* **Total Inventory Quantity**: 336K units across 428 unique products.
* **Products Below Reorder Point**: 343 SKUs (\~80%).
* **Out-of-Stock Products**: Only 4 SKUs (\~0.93%).
* **Inventory Turnover**: \~4.66 times.
* **Low Inventory Categories**: Bikes (279), Components (54).
* **Insight**: Most shortages are concentrated in Bikes, suggesting high demand but insufficient stock planning.

  <img width="2917" height="1771" alt="Screenshot 2025-09-06 094910" src="https://github.com/user-attachments/assets/e20d0158-a4a4-4683-ad20-082de40a34de" />


### 2️⃣ Inventory vs Sales Dashboard

* **Bikes**: Highest inventory but slow turnover, tying up capital.
* **Accessories & Clothing**: Very low inventory relative to sales → potential lost sales.
* **Trend Analysis**: Stock rose sharply in Q2 2012 but sales did not match, indicating forecasting issues.
* **Insight**: Misalignment between stock allocation and market demand.

  <img width="2766" height="1765" alt="Screenshot 2025-09-06 094918" src="https://github.com/user-attachments/assets/c82fb769-245f-4352-804d-77b721a1ded3" />


### 3️⃣ Reorder Point Analysis Dashboard

* **\~80% of products** are below reorder point, mainly in Bikes & Components.
* **Clothing**: 90% safe, but still the highest Out-of-Stock rate (\~10%).
* **Inventory by Location**: Finished Goods storage has safe levels, but many other warehouses are 100% below reorder point.
* **Insight**: Inefficient allocation across warehouses leads to stockouts in some while others are safe.

<img width="2748" height="1767" alt="Screenshot 2025-09-06 094924" src="https://github.com/user-attachments/assets/a4f1f362-c8b1-47de-8c32-ef0154180296" />


---

## 🔎 Final Conclusion & Recommendations

📌 **Key Takeaways**

✔️ Majority of SKUs (\~80%) are below reorder point → urgent restocking required.

✔️ Bikes consume the largest share of inventory but turn over slowly.

✔️ Accessories & Clothing are understocked despite demand, risking lost revenue.

✔️ Inventory turnover (4.66) needs benchmarking against industry standards.

✔️ Stock allocation across warehouses is unbalanced.

📌 **Recommendations**

1. **Adjust Reorder Points** – Recalibrate safety stock and reorder levels based on actual sales trends.
2. **Focus on Bikes & Components** – Prioritize replenishment for these categories while managing turnover.
3. **Boost Accessories & Clothing Stock** – Increase supply to capture unmet demand and avoid lost sales.
4. **Improve Forecasting** – Use time-series analysis to predict seasonal demand (e.g., Q2 2012 spike).
5. **Reallocate Inventory Across Warehouses** – Shift products from overstocked to understocked locations.
6. **Benchmark Turnover** – Compare with industry KPIs to set realistic performance targets.

---

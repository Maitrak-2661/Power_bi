# 📊 Superstore Sales & Profit Dashboard — Power BI

An interactive Power BI dashboard built on the classic **Sample Superstore** dataset, analyzing sales, profit, quantity, and discount performance across regions, categories, and sub-categories of a U.S. retail business.

![Power BI](https://img.shields.io/badge/Power%20BI-F2C811?style=flat&logo=powerbi&logoColor=black)
![Status](https://img.shields.io/badge/status-active-brightgreen)
![License](https://img.shields.io/badge/license-MIT-blue)

---

## 🖼️ Dashboard Preview

The report includes four KPI cards, two dynamic charts, and slicers for on-the-fly filtering by **Region** and **Category**:

| KPI | Value |
|---|---|
| 💰 Total Sales | ₹182.36K *(filtered view)* / **₹22,97,200.86** *(full dataset)* |
| 📈 Total Profit | ₹42.00K *(filtered view)* / **₹2,86,397.02** *(full dataset)* |
| 📦 Total Quantity | 6K *(filtered view)* / **37,873** *(full dataset)* |
| 🏷️ Average Discount | 0.09 (9%) |

> The dashboard is shown filtered to **Region = West** and **Category = Office Supplies** in the screenshot — use the slicers to explore other combinations.

---

## 📁 Project Contents

| File | Description |
|---|---|
| `Sample_-_Superstore.csv` | Raw transactional dataset (9,994 orders, 2014–2017) |
| `superstore_dashboard.pdf` | Exported snapshot of the Power BI report |
| `Superstore Dashboard.pbix` | *(Power BI source file — add this when you save your `.pbix`)* |
| `README.md` | This file |

---

## 🗃️ About the Dataset

The **Sample Superstore** dataset is a widely used retail analytics dataset containing order-level transactions for a fictional U.S. office/furniture/tech retailer.

| Attribute | Detail |
|---|---|
| **Rows** | 9,994 order line items |
| **Date Range** | Jan 3, 2014 – Dec 30, 2017 |
| **Regions** | Central, East, South, West |
| **Categories** | Furniture, Office Supplies, Technology |
| **Sub-Categories** | 17 (e.g., Storage, Binders, Phones, Chairs, Machines) |
| **Customer Segments** | Consumer, Corporate, Home Office |
| **Ship Modes** | Standard Class, Second Class, First Class, Same Day |
| **Geography** | 49 states, 531 cities |

**Key columns:**

| Column | Type | Description |
|---|---|---|
| `Order ID`, `Row ID` | Text/ID | Unique order and row identifiers |
| `Order Date`, `Ship Date` | Date | Order placement and fulfillment dates |
| `Customer ID`, `Customer Name`, `Segment` | Text | Customer details |
| `Region`, `State`, `City`, `Postal Code` | Geography | Location fields |
| `Category`, `Sub-Category`, `Product Name` | Text | Product hierarchy |
| `Sales`, `Profit` | Numeric (₹/$) | Revenue and profit per line item |
| `Quantity` | Numeric | Units sold |
| `Discount` | Numeric (%) | Discount applied |
| `Ship Mode` | Text | Shipping method |

---

## 📊 Report Pages & Visuals

### 1. KPI Summary Cards
Four headline metrics — **Total Sales**, **Total Profit**, **Total Quantity**, and **Average Discount** — update live as slicers are applied.

### 2. Total Revenue by Product Category
A horizontal bar chart showing **Sum of Sales by Sub-Category**, highlighting that **Storage (₹59K)** and **Binders (₹44K)** are the top-selling sub-categories within Office Supplies.

### 3. Total Profit by Respective Region
A horizontal bar chart showing **Sum of Profit by Region**, letting users compare regional profitability at a glance.

### 4. Interactive Slicers
- **Region** slicer (Central / East / South / West)
- **Category** slicer (Furniture / Office Supplies / Technology)

### 5. Detail Table (Drill-through / Data View)
A row-level table listing **Order ID, Customer Name, Product Name, Category, Sub-Category, Sales, Profit, Quantity, Region,** and **Ship Mode**, with running totals for Sales, Profit, and Quantity.

---

## 🧮 Suggested DAX Measures

```DAX
Total Sales = SUM('Superstore'[Sales])

Total Profit = SUM('Superstore'[Profit])

Total Quantity = SUM('Superstore'[Quantity])

Average Discount = AVERAGE('Superstore'[Discount])

Profit Margin % = DIVIDE([Total Profit], [Total Sales], 0)

Sales YoY % = 
VAR CurrentSales = [Total Sales]
VAR PriorSales = CALCULATE([Total Sales], SAMEPERIODLASTYEAR('Calendar'[Date]))
RETURN DIVIDE(CurrentSales - PriorSales, PriorSales, 0)
```

---

## 🛠️ How to Use This Project

1. **Clone or download** this repository.
2. Open **`Superstore Dashboard.pbix`** in Power BI Desktop (2021 or later recommended).
3. If prompted, update the data source path to point to `Sample_-_Superstore.csv` on your machine.
4. Click **Refresh** on the Home ribbon to reload the latest data.
5. Use the **Region** and **Category** slicers to filter the dashboard interactively.
6. To publish: **File → Publish → Publish to Power BI** (requires a Power BI account).

### Requirements
- Power BI Desktop (free download from [powerbi.microsoft.com](https://powerbi.microsoft.com/desktop/))
- No additional connectors required — data loads directly from CSV

---

## 💡 Key Insights

- **Storage and Binders** are the highest-revenue sub-categories within Office Supplies (₹59K and ₹44K respectively), together contributing more than the remaining three sub-categories combined.
- The **West region** is a strong profit contributor, reflected prominently in the regional profit breakdown.
- Across the full dataset, the business achieves an overall **profit margin of ~12.5%** (₹2,86,397 profit on ₹22,97,201 sales).
- **Average discount** across filtered views sits around **9%**, a useful benchmark for evaluating discount-driven profit erosion.

---

## 🚀 Possible Enhancements

- [ ] Add a **time-series trend** page (monthly/yearly Sales & Profit trend lines)
- [ ] Add **Top 10 Customers/Products** ranking visuals
- [ ] Add **Discount vs. Profit** scatter plot to flag loss-making discount bands
- [ ] Add **Shipping performance** analysis (Order-to-Ship lead time by Ship Mode)
- [ ] Add **geographic map** visual (Sales/Profit by State or City)
- [ ] Build a **What-If parameter** for simulating discount impact on profit

---

## 📄 License

This project uses the publicly available Sample Superstore dataset for educational and portfolio purposes. Feel free to reuse or adapt this dashboard with attribution.

---

## 🙋 Author / Maintainer

*Add your name, LinkedIn, and portfolio link here.*

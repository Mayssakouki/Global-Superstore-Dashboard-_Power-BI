# 📊 Global Superstore Business Intelligence Dashboard — Power BI Project

## 📋 Project Overview

This interactive dashboard provides a comprehensive analysis of global retail sales data using the **Global Superstore dataset**.

The project explores key business metrics and identifies the most significant performance drivers across **51,290 orders** spanning 4 years (2011–2014), covering sales, profitability, geography, and time trends through advanced visual analytics and interactive reporting.

---

# 🗄️ Dataset

* **Source:** Global Superstore CSV
* **Records:** 51,290 orders
* **Features:** 27 attributes
* **Target Variables:** Sales, Profit, Quantity, Shipping Cost

---

# 🧹 Data Preparation & Transformation

## 🔹 Data Cleaning

Performed extensive data cleaning and quality enhancement using **Power Query**:

* Fixed `Order_Date` and `Ship_Date` data types from **Date/Time** to **Date** *(Using Locale — English US)*
* Fixed `Profit`, `Discount`, and `Shipping_Cost` data types from **Text** to **Decimal Number** *(regional separator issue)*
* Fixed `Sales`, `Quantity`, and `Year` data types from **Text** to **Whole Number**
* Renamed all columns — replaced dots `.` with underscores `_` for clarity
* Removed 4 unnecessary columns

---

## 🔹 Columns Removed

| Column | Reason |
|---|---|
| `记录数` | Redundant Chinese counter |
| `Market2` | Duplicate of `Market` |
| `weeknum` | Recalculable via DAX |
| `Row.ID` | Simple useless index |

---

## 🔹 Data Quality Results

* ✅ Valid : 100%
* ✅ Empty : 0%
* ✅ Error : 0%

---

# ⭐ Data Modeling — Star Schema

## 🔹 Tables Created

| Table | Type | Key Columns |
|---|---|---|
| `Fact_Orders` | Fact | Order_ID, Customer_ID, Product_ID, City, Order_Date |
| `Dim_Client` | Dimension | Customer_ID, Customer_Name, Segment |
| `Dim_Produit` | Dimension | Product_ID, Product_Name, Category, Sub_Category |
| `Dim_Geo` | Dimension | City, State, Country, Region, Market |
| `Dim_Calendrier` | Dimension | Date, Année, Mois_Num, Mois_Nom, Trimestre, Semaine, Jour |
| `_Mesures` | DAX Measures | — |

---

## 🔹 Relationships

| From (Many *) | Column | To (One 1) | Column | Cardinality |
|---|---|---|---|---|
| `Fact_Orders` | `Customer_ID` | `Dim_Client` | `Customer_ID` | * → 1 |
| `Fact_Orders` | `Product_ID` | `Dim_Produit` | `Product_ID` | * → 1 |
| `Fact_Orders` | `City` | `Dim_Geo` | `City` | * → 1 |
| `Fact_Orders` | `Order_Date` | `Dim_Calendrier` | `Date` | * → 1 |

---

## 🔹 Calendar Table — M Language

The `Dim_Calendrier` table was created from scratch using **Power Query M Language**, generating all dates between 01/01/2011 and 31/12/2014 with the following attributes:

* Date
* Année *(Year)*
* Mois_Num *(Month Number)*
* Mois_Nom *(Month Name)*
* Trimestre *(Quarter)*
* Semaine *(Week)*
* Jour *(Day)*
* Jour_Nom *(Day Name)*

---

# 📐 DAX Measures

## 🔹 Base Measures

```dax
Total Sales = SUM(Fact_Orders[Sales])

Total Profit = SUM(Fact_Orders[Profit])

Total Quantity = SUM(Fact_Orders[Quantity])

Total Shipping Cost = SUM(Fact_Orders[Shipping_Cost])

Total Orders = DISTINCTCOUNT(Fact_Orders[Order_ID])
```

---

## 🔹 Calculated Measures

```dax
Profit Margin = DIVIDE([Total Profit], [Total Sales], 0)

Average Order Value = DIVIDE([Total Sales], [Total Orders], 0)

Average Discount = AVERAGE(Fact_Orders[Discount])
```

---

## 🔹 Year over Year (YoY) Measures

```dax
Total Sales LY = CALCULATE([Total Sales], SAMEPERIODLASTYEAR(Dim_Calendrier[Date]))

Sales YoY % = DIVIDE([Total Sales] - [Total Sales LY], [Total Sales LY], 0)

Total Profit LY = CALCULATE([Total Profit], SAMEPERIODLASTYEAR(Dim_Calendrier[Date]))

Profit YoY % = DIVIDE([Total Profit] - [Total Profit LY], [Total Profit LY], 0)
```

---

# ✨ Dashboard Features

* Fully interactive dashboard with dynamic filtering
* Professional and modern UI/UX design
* Real-time slicing and filtering capabilities
* Multi-page analytical structure
* Business insights highlighted through comparative visuals
* Synchronized Slicer across all pages

---

# 📊 Dashboard Pages & Visualizations

# 📄 Page 1 — Executive Dashboard

## 📌 KPI Cards

* Total Sales
* Total Profit
* Total Orders
* Profit Margin

## 📈 Charts & Visuals

* Clustered Bar Chart — Total Sales by Category
* Pie Chart — Total Sales by Segment
* Clustered Bar Chart — Total Sales by Market

## 🎛️ Interactive Filters

* Year Slicer *(synchronized across all pages)*

---

# 📄 Page 2 — Analyse des Ventes

## 📈 Charts & Visuals

* Line Chart — Total Sales by Year
* Clustered Bar Chart — Total Sales by Sub-Category *(17 bars)*
* Clustered Column Chart — Total Sales by Ship Mode
* Clustered Column Chart — Total Sales by Order Priority

---

# 📄 Page 3 — Analyse de la Rentabilité

## 📈 Charts & Visuals

* Clustered Bar Chart — Total Profit by Category
* Clustered Bar Chart — Total Profit by Sub-Category
* Clustered Bar Chart — Total Profit by Market
* Clustered Bar Chart — Profit Margin by Segment
* Line Chart — Total Profit by Year

---

# 📄 Page 4 — Performance Géographique

## 📈 Charts & Visuals

* Map — World Map *(bubble size = Total Sales)*
* Clustered Bar Chart — Total Sales by Country
* Clustered Bar Chart — Total Profit by Region

---

# 📄 Page 5 — Tendances Temporelles

## 📌 KPI Cards

* Total Sales
* Total Sales LY *(Last Year)*
* Sales YoY %
* Total Profit
* Total Profit LY *(Last Year)*
* Profit YoY %

## 📈 Charts & Visuals

* Line Chart — Total Sales by Month *(4 lines — one per year)*
* Line Chart — Total Profit by Month *(4 lines — one per year)*
* Clustered Column Chart — Total Sales by Quarter
* Clustered Column Chart — Total Profit by Quarter

---

# 🎛️ Filters & Interactivity

* **Year Slicer** *(Tile style)* created on Page 1
* **Sync Slicers** activated across all 5 pages *(Sync ✅ + Visible ✅)*
* Native cross-filtering between visuals on the same page

---

# 🔍 Key Insights

## ⚠️ Major Findings

* **APAC** is the highest performing market in terms of total sales

  > Asia Pacific leads global revenue generation

* **Technology** is the most profitable category

* **Standard Class** is the most used shipping mode across all markets

* Sales show **continuous growth** from 2011 to 2014

* Some regions display **negative profit** — a critical area to monitor

* **Q4** consistently generates the highest sales across all years

  > End-of-year demand drives peak performance

---

# 🛠️ Technologies & Skills Demonstrated

## 🔧 Tools & Technologies

* Power BI Desktop
* Power Query *(M Language)*
* DAX *(Data Analysis Expressions)*

## 💡 Skills Applied

* Data Cleaning & Transformation
* ETL Processes
* Star Schema Data Modeling
* Calendar Table Creation *(M Language)*
* DAX Measures & KPIs
* Year over Year Analysis
* Interactive Dashboard Development
* Data Visualization & Storytelling
* UI/UX Dashboard Design
* Business Intelligence Reporting

# Superstore Sales Analytics Dashboard — Power BI 📊

An interactive Power BI dashboard analyzing the Sample Superstore dataset with advanced DAX measures, dynamic filtering, drill-through capabilities, and executive-ready visualizations.

---

## 📈 Dashboard Preview

| Page | Focus |
|------|-------|
| **Executive Summary** | KPI cards, YoY growth, top-level trends |
| **Sales Analysis** | Regional maps, category/sub-category breakdown, shipping analysis |
| **Profitability** | Profit margins by segment, product, region; loss leaders |
| **Customer Insights** | RFM analysis, customer lifetime value, top customers |
| **Forecasting** | Time series forecasting with confidence intervals |

---

## 🗂️ Dataset

**Source:** Sample Superstore (Kaggle / Tableau community)  
**Records:** ~10,000 orders (2014–2017)  
**Fields:** Order ID, Order Date, Ship Date, Ship Mode, Customer ID, Customer Name, Segment, Country, City, State, Postal Code, Region, Product ID, Category, Sub-Category, Product Name, Sales, Quantity, Discount, Profit

---

## 🛠️ Tech Stack

| Tool | Version | Purpose |
|------|---------|---------|
| **Power BI Desktop** | Latest | Report authoring |
| **DAX** | — | Calculated measures, columns, tables |
| **Power Query (M)** | — | Data transformation, cleaning |
| **Data Model** | Star Schema | Fact table + dimension tables |

---

## 📐 Data Model

```
Fact_Sales (10K rows)
    │
    ├── Dim_Date (Date hierarchy: Year > Quarter > Month > Day)
    ├── Dim_Customer (Customer ID, Name, Segment, Region)
    ├── Dim_Product (Product ID, Name, Category, Sub-Category)
    ├── Dim_Geography (Country, State, City, Postal Code, Region)
    └── Dim_Shipping (Ship Mode)
```

**Relationships:** One-to-many from dimensions to fact table.

---

## 🧮 Key DAX Measures

```dax
-- Core KPIs
Total Sales = SUM(Fact_Sales[Sales])
Total Profit = SUM(Fact_Sales[Profit])
Total Orders = DISTINCTCOUNT(Fact_Sales[Order ID])
Avg Order Value = DIVIDE([Total Sales], [Total Orders])
Profit Margin = DIVIDE([Total Profit], [Total Sales])

-- Time Intelligence
Sales YTD = CALCULATE([Total Sales], DATESYTD(Dim_Date[Date]))
Sales PYTD = CALCULATE([Total Sales], DATESYTD(SAMEPERIODLASTYEAR(Dim_Date[Date])))
YoY Growth % = DIVIDE([Sales YTD] - [Sales PYTD], [Sales PYTD])

-- Dynamic Top N
Top N Products = 
VAR N = SELECTEDVALUE(TopN_Parameter[TopN Value], 10)
RETURN
    CALCULATETABLE(
        TOPN(N, VALUES(Dim_Product[Product Name]), [Total Sales]),
        ALLSELECTED(Dim_Product)
    )

-- RFM Segmentation
Recency Score = 
VAR LastOrder = CALCULATE(MAX(Fact_Sales[Order Date]), ALLEXCEPT(Fact_Sales, Fact_Sales[Customer ID]))
VAR MaxDate = CALCULATE(MAX(Fact_Sales[Order Date]), ALL(Fact_Sales))
RETURN DATEDIFF(LastOrder, MaxDate, DAY)
```

---

## 🎨 Visualizations Used

- **KPI Cards** with conditional formatting (green/red for YoY)
- **Choropleth Map** — Sales/Profit by State
- **Decomposition Tree** — Drill into Category → Sub-Category → Product
- **Scatter Plot** — Sales vs Profit by Product (quadrant analysis)
- **Ribbon Chart** — Category ranking over time
- **Waterfall** — Profit bridge by Segment/Region
- **Forecast Chart** — Built-in Power BI forecasting (ETS)
- **Slicer Panel** — Dynamic period, region, segment, category

---

## 🚀 Getting Started

### Prerequisites

- Power BI Desktop (latest version)
- Sample Superstore dataset (included in `data/` folder)

### Open the Report

```bash
git clone https://github.com/dineshreddypothula/Superstore-Sales-PowerBI-Dashboard.git
cd Superstore-Sales-PowerBI-Dashboard

# Open in Power BI Desktop
# File > Open > Superstore_Sales_Dashboard.pbix
```

### Refresh Data

1. Home > Transform Data (Power Query Editor)
2. Verify `Source` step points to local `data/Sample_Superstore.xlsx`
3. Close & Apply

---

## 📁 Project Structure

```
Superstore-Sales-PowerBI-Dashboard/
├── data/
│   └── Sample_Superstore.xlsx
├── Superstore_Sales_Dashboard.pbix
├── docs/
│   ├── DAX_Reference.md
│   ├── Data_Model.png
│   └── Dashboard_Screenshots/
└── README.md
```

---

## 🎯 Business Questions Answered

| Question | Page / Visual |
|----------|---------------|
| How are sales trending YoY? | Executive Summary > KPI + Line chart |
| Which regions are most profitable? | Sales Analysis > Map + Bar chart |
| What products lose money? | Profitability > Scatter (Sales vs Profit) |
| Who are our best customers? | Customer Insights > RFM Table |
| What's the forecast for next quarter? | Forecasting > Forecast chart |

---

## 📄 License

MIT License — free for learning and portfolio use.

---

## 👨‍💻 Author

**Dinesh Reddy Pothula**  
Data Analytics • Power BI • DAX • Data Visualization  

[GitHub](https://github.com/dineshreddypothula) • [LinkedIn](https://www.linkedin.com/in/dinesh-reddy-pothula/)
# 🚀 Executive Business Intelligence Dashboard

<p align="center">

# 📊 Retail Intelligence | Inventory Risk | Sales Analytics | Executive Decisions

### ✨ Turning Raw Retail Data into Actionable Business Intelligence ✨

</p>

---

## 🌟 Project at a Glance

<p align="center">

**RAW DATA**
⬇️
🧹 **DATA CLEANING**
⬇️
🗄️ **MYSQL DATABASE**
⬇️
📐 **STATISTICAL ANALYSIS**
⬇️
📊 **POWER BI**
⬇️
🎯 **EXECUTIVE DECISIONS**

</p>

> 💡 **The goal is simple:** Transform complex retail data into clear insights that help managers decide **what to sell, what to reorder, what to promote, what to reduce, and where the business is losing money.**

---

# 🧠 Business Problem

Retail companies generate large amounts of **sales, product, inventory, store, and category data**.

However, raw data alone does not answer the questions executives actually need:

```text
❓ What are our best-selling products?
❓ Which products generate the most profit?
❓ Which products have too much inventory?
❓ Which products may face stockouts?
❓ Which stores are underperforming?
❓ Which categories drive revenue?
❓ Where is inventory capital getting blocked?
❓ Where are we potentially losing revenue?
❓ What action should management take?
```

### 🎯 Proposed Solution

The **Executive Business Intelligence Dashboard** combines:

**Sales + Inventory + Products + Stores + Categories + Time**

into a centralized analytical solution.

---

# 🎯 Project Objectives

### 📈 Sales Intelligence

* Analyze revenue trends
* Identify top-selling products
* Compare store performance
* Analyze category contribution

### 📦 Inventory Intelligence

* Identify overstock products
* Detect stockout risks
* Analyze slow-moving inventory
* Measure inventory value
* Calculate inventory turnover

### 💰 Profitability Intelligence

* Identify high-profit products
* Analyze profit margins
* Compare revenue vs profit
* Detect low-profit products

### 🏢 Executive Intelligence

* Identify business risks
* Detect revenue opportunities
* Compare regions/stores
* Support reorder decisions
* Support promotion decisions
* Support inventory reduction decisions

---

# 🏗️ Complete Data Architecture

The project follows a structured relational data model.

```text
                         ┌──────────────────┐
                         │    DIM_DATE      │
                         │──────────────────│
                         │ Date_ID (PK)     │
                         │ Date             │
                         │ Month            │
                         │ Quarter          │
                         │ Year             │
                         └────────┬─────────┘
                                  │
                                  │ 1 : Many
                                  ▼
┌──────────────────┐       ┌──────────────────┐
│   DIM_PRODUCT    │       │    FACT_SALES    │
│──────────────────│       │──────────────────│
│ Product_ID (PK)  │──────<│ Product_ID (FK) │
│ Product_Name     │  1:M  │ Store_ID (FK)   │
│ Category_ID (FK) │       │ Date_ID (FK)    │
│ Unit_Cost        │       │ Quantity         │
│ Selling_Price    │       │ Revenue          │
└────────┬─────────┘       │ Profit           │
         │                 └────────┬─────────┘
         │ 1:M                      │
         ▼                          │
┌──────────────────┐                │
│   DIM_CATEGORY   │                │
│──────────────────│                │
│ Category_ID (PK) │                │
│ Category_Name    │                │
└──────────────────┘                │
                                    │
                                    │ M:1
                                    ▼
                           ┌──────────────────┐
                           │    DIM_STORE     │
                           │──────────────────│
                           │ Store_ID (PK)    │
                           │ Store_Name       │
                           │ Region_ID (FK)   │
                           │ Location         │
                           └────────┬─────────┘
                                    │
                                    │ M:1
                                    ▼
                           ┌──────────────────┐
                           │   DIM_REGION     │
                           │──────────────────│
                           │ Region_ID (PK)   │
                           │ Region_Name      │
                           └──────────────────┘


┌──────────────────┐
│ FACT_INVENTORY   │
│──────────────────│
│ Inventory_ID PK  │
│ Product_ID FK    │──────> DIM_PRODUCT
│ Store_ID FK      │──────> DIM_STORE
│ Date_ID FK       │──────> DIM_DATE
│ Stock_Quantity   │
│ Reorder_Level    │
│ Inventory_Value  │
└──────────────────┘
```

---

# 🔗 Table Relationships

The database follows a **Star Schema / Dimensional Modeling approach**.

| Parent Table   | Child Table      | Relationship | Key         |
| -------------- | ---------------- | ------------ | ----------- |
| `DIM_PRODUCT`  | `FACT_SALES`     | 1 : Many     | Product_ID  |
| `DIM_PRODUCT`  | `FACT_INVENTORY` | 1 : Many     | Product_ID  |
| `DIM_CATEGORY` | `DIM_PRODUCT`    | 1 : Many     | Category_ID |
| `DIM_STORE`    | `FACT_SALES`     | 1 : Many     | Store_ID    |
| `DIM_STORE`    | `FACT_INVENTORY` | 1 : Many     | Store_ID    |
| `DIM_REGION`   | `DIM_STORE`      | 1 : Many     | Region_ID   |
| `DIM_DATE`     | `FACT_SALES`     | 1 : Many     | Date_ID     |
| `DIM_DATE`     | `FACT_INVENTORY` | 1 : Many     | Date_ID     |

### 🔑 Primary Key

A **Primary Key (PK)** uniquely identifies each record.

Example:

```text
DIM_PRODUCT
Product_ID = P001
Product_ID = P002
Product_ID = P003
```

### 🔗 Foreign Key

A **Foreign Key (FK)** connects one table with another.

Example:

```text
FACT_SALES.Product_ID
            ↓
DIM_PRODUCT.Product_ID
```

This allows us to answer:

> "How much revenue did each product generate?"

---

# 🧩 Entity Relationship Model

The conceptual relationship can be represented as:

```text
                    CATEGORY
                       │
                       │ 1
                       │
                       ▼
                    PRODUCT
                 ┌─────┴─────┐
                 │           │
                 │ 1         │ 1
                 ▼           ▼
              SALES      INVENTORY
                 │           │
                 │ M         │ M
                 ▼           ▼
               STORE ◄───────┘
                 │
                 │ M
                 ▼
               REGION

DATE ───────────► SALES
DATE ───────────► INVENTORY
```

---

# ⭐ Why This Relationship Model?

The relational structure prevents unnecessary duplication and makes analysis easier.

### Example:

Instead of storing:

```text
Product Name
Category Name
Store Name
Region Name
Date
Revenue
Profit
```

repeatedly inside every transaction, the model separates the information into appropriate tables.

This provides:

✅ Better data organization
✅ Reduced redundancy
✅ Easier SQL analysis
✅ Better Power BI performance
✅ Cleaner relationships
✅ Easier filtering
✅ Scalable business intelligence

---

# 🔄 End-to-End Data Flow

```text
        📁 RAW RETAIL DATA
                 │
                 ▼
        🧹 DATA CLEANING
                 │
                 ▼
        📊 EXCEL VALIDATION
                 │
                 ▼
        🗄️ MYSQL DATABASE
                 │
        ┌────────┴────────┐
        ▼                 ▼
   SQL ANALYSIS      STATISTICS
        │                 │
        └────────┬────────┘
                 ▼
          🔄 POWER QUERY
                 │
                 ▼
        🧩 DATA MODELING
                 │
                 ▼
              DAX
                 │
                 ▼
       📊 POWER BI DASHBOARD
                 │
                 ▼
       🎯 BUSINESS INSIGHTS
                 │
                 ▼
       👔 EXECUTIVE DECISION
```

---

# 🛠️ Technology Stack

<p align="center">

📊 **Microsoft Excel**
  → Data Cleaning & Validation

🗄️ **MySQL**
  → Database & SQL Analysis

📐 **Statistics**
  → Business & Performance Analysis

📈 **Power BI**
  → Data Modeling & Visualization

</p>

### Core Technologies

```text
Excel
SQL
MySQL
Power Query
DAX
Power BI
Statistics
Data Visualization
Business Intelligence
```

---

# 📊 Power BI Dashboard Pages

## 1️⃣ Executive Overview

### Purpose

Give senior management a **30-second overview of business performance**.

### KPIs

```text
💰 Total Revenue
📈 Total Profit
🛒 Units Sold
📦 Inventory Value
🔄 Inventory Turnover
⚠️ Risk Products
🏪 Store Performance
```

### Key Visuals

* Revenue Trend
* Profit Trend
* Revenue by Category
* Revenue by Region
* Store Performance
* Inventory Health

---

# 2️⃣ 📈 Sales & Business Performance

### Questions Answered

* Which month generated the highest revenue?
* Which category contributes most revenue?
* Which store performs best?
* Which region is declining?
* What is the profit trend?

### Visuals

```text
📈 Monthly Revenue Trend
📊 Category Revenue
🏪 Store Comparison
🌍 Regional Performance
💰 Profit Analysis
```

---

# 3️⃣ 📦 Inventory Risk & Efficiency

### Questions Answered

* Which products are overstocked?
* Which products are close to stockout?
* Which products are slow-moving?
* How much capital is tied in inventory?
* Which products require immediate action?

### Risk Classification

| Status         | Meaning            | Recommended Action    |
| -------------- | ------------------ | --------------------- |
| 🔴 Critical    | Immediate risk     | Take immediate action |
| 🟠 Warning     | Potential risk     | Monitor closely       |
| 🟢 Healthy     | Good inventory     | Maintain              |
| 🔵 Slow Moving | Low sales velocity | Promote / Reduce      |

---

# 4️⃣ 🏷️ Product Performance

This page provides product-level intelligence.

### Analysis

```text
🏆 Top Revenue Products
💰 Top Profit Products
📦 High Inventory Products
🐌 Slow-Moving Products
⚠️ Stockout Risk Products
📊 Product Profit Margin
```

### Decision Matrix

```text
                     SALES
                LOW          HIGH
             ┌──────────┬──────────┐
        HIGH │  REDUCE  │ REORDER  │
             │    📦    │    🔄    │
INVENTORY    ├──────────┼──────────┤
        LOW  │ PROMOTE  │ HEALTHY  │
             │    📢    │    🟢    │
             └──────────┴──────────┘
```

---

# 💡 Executive Decision Engine

The dashboard does not stop at visualization.

It converts insights into **recommended business actions**.

```text
              PRODUCT ANALYSIS
                     │
          ┌──────────┼──────────┐
          ▼          ▼          ▼
       SALES      STOCK       PROFIT
          │          │          │
          └──────────┼──────────┘
                     ▼
              RISK ANALYSIS
                     │
                     ▼
            BUSINESS DECISION
                     │
       ┌─────────────┼─────────────┐
       ▼             ▼             ▼
    🔄 REORDER    📢 PROMOTE    📦 REDUCE
```

---

# 🧮 Key Business Metrics

### Total Revenue

```text
Revenue = Quantity Sold × Selling Price
```

### Profit

```text
Profit = Revenue − Total Cost
```

### Profit Margin

```text
Profit Margin % =
(Profit / Revenue) × 100
```

### Inventory Value

```text
Inventory Value =
Current Stock × Unit Cost
```

### Inventory Turnover

```text
Inventory Turnover =
Cost of Goods Sold / Average Inventory
```

---

# 📐 Statistical Analysis

Statistics is used to understand the behavior and performance of the business.

### Techniques

* Mean
* Median
* Standard Deviation
* Variance
* Trend Analysis
* Correlation
* Performance Distribution
* Comparative Analysis

### Example

Correlation analysis can help investigate relationships such as:

```text
Sales Volume
      ↕
Inventory Level

Sales Volume
      ↕
Revenue

Revenue
      ↕
Profit
```

---

# 🗄️ SQL Analysis

SQL is used to extract business insights from the relational database.

### Example Business Questions

```sql
-- Top revenue-generating products

SELECT
    Product_ID,
    SUM(Revenue) AS Total_Revenue
FROM FACT_SALES
GROUP BY Product_ID
ORDER BY Total_Revenue DESC;
```

```sql
-- Store performance

SELECT
    Store_ID,
    SUM(Revenue) AS Revenue,
    SUM(Profit) AS Profit
FROM FACT_SALES
GROUP BY Store_ID
ORDER BY Revenue DESC;
```

---

# 📊 Power BI Data Model

The Power BI model follows a **fact-and-dimension architecture**.

```text
                   DIM_DATE
                      │
                      │
                      ▼
DIM_CATEGORY ──► DIM_PRODUCT ◄── DIM_STORE ◄── DIM_REGION
                      │              │
                      │              │
              ┌───────┴───────┐      │
              ▼               ▼      │
         FACT_SALES      FACT_INVENTORY
```

### Fact Tables

📊 `FACT_SALES`

📦 `FACT_INVENTORY`

### Dimension Tables

📅 `DIM_DATE`

🏷️ `DIM_PRODUCT`

📂 `DIM_CATEGORY`

🏪 `DIM_STORE`

🌍 `DIM_REGION`

---

# 📁 Repository Structure

```text
Executive-Business-Intelligence-Dashboard/
│
├── 📄 README.md
│
├── 📁 data/
│   ├── raw/
│   └── cleaned/
│
├── 📁 excel/
│   └── Retail_Data_Cleaned.xlsx
│
├── 📁 sql/
│   ├── database_schema.sql
│   ├── table_creation.sql
│   └── analysis_queries.sql
│
├── 📁 powerbi/
│   └── Executive_BI_Dashboard.pbix
│
├── 📁 screenshots/
│   ├── dashboard-demo.gif
│   ├── executive-overview.png
│   ├── sales-analysis.png
│   ├── inventory-risk.png
│   └── product-performance.png
│
├── 📁 documentation/
│   ├── problem_statement.md
│   ├── data_dictionary.md
│   ├── data_model.md
│   └── project_documentation.md
│
└── 📁 assets/
    └── dashboard-banner.gif
```

---

# 🔐 Data & Security

Sensitive information should **never** be committed to the repository.

Do not upload:

```text
❌ Passwords
❌ API Keys
❌ Database Credentials
❌ .env files
❌ Private Business Data
```

Use:

```text
.gitignore
.env.example
```

for sensitive configuration.

---

# 🚀 Future Enhancements

The project can evolve from traditional BI into an **AI-powered Retail Intelligence Platform**.

### 🔮 Planned Features

```text
🤖 Demand Forecasting
        ↓
📦 Future Inventory Prediction
        ↓
⚠️ Stockout Prediction
        ↓
💰 Revenue Forecasting
        ↓
🧠 AI Recommendations
        ↓
🔔 Automated Alerts
        ↓
☁️ Cloud Deployment
```

Potential future additions:

* Machine Learning demand forecasting
* Stockout prediction
* Revenue forecasting
* AI-generated recommendations
* Automated inventory alerts
* Real-time dashboards
* Cloud database integration
* Natural-language business queries

---

# 🏆 Project Highlights

| Area                | Implementation                     |
| ------------------- | ---------------------------------- |
| 📊 Data Cleaning    | Excel / Power Query                |
| 🗄️ Database        | MySQL                              |
| 🔍 Data Analysis    | SQL                                |
| 📐 Statistics       | Descriptive & comparative analysis |
| 🧩 Data Modeling    | Star Schema                        |
| 📈 Visualization    | Power BI                           |
| 🧮 Calculations     | DAX                                |
| 📦 Inventory        | Risk & efficiency analysis         |
| 💰 Profitability    | Revenue & profit analysis          |
| 🎯 Decision Support | Executive recommendations          |

---

# 👔 Target Users

### 🧑‍💼 Business Managers

Monitor overall business performance.

### 📦 Inventory Managers

Identify overstock, slow-moving, and stockout-risk products.

### 🏪 Store Managers

Compare store performance and identify improvement areas.

### 📊 Business Analysts

Perform detailed analysis and identify trends.

### 👔 Executives

Get a high-level overview for strategic decision-making.

---

# 🌍 Real-World Business Impact

### Before

```text
📁 Multiple Data Sources
        ↓
📊 Manual Excel Reports
        ↓
⏳ Time-Consuming Analysis
        ↓
❓ Difficult Decisions
```

### After

```text
📊 Integrated Data
        ↓
🗄️ Structured Database
        ↓
📈 Automated Analytics
        ↓
🎯 Interactive Dashboard
        ↓
⚡ Faster Decisions
```

---

# 📌 Final Outcome

The **Executive Business Intelligence Dashboard** provides a centralized view of:

> **Sales Performance + Inventory Efficiency + Product Performance + Profitability + Business Risk**

It helps decision-makers move from:

### ❌ "What happened?"

to

### ✅ "Why did it happen?"

and finally to

### 🚀 "What should we do next?"

---

# 👨‍💻 About the Developer

## Mukesh N

🎓 **B.Sc Computer Science**

📍 Coimbatore, Tamil Nadu

💼 Aspiring **Data Analyst / Business Intelligence Analyst**

### Skills

```text
📊 Excel
🗄️ SQL
🐬 MySQL
📈 Power BI
🐍 Python
📐 Statistics
📊 Data Analytics
💡 Business Intelligence
```

---

# ⭐ Project

If you find this project useful or interesting, consider giving the repository a ⭐.

<p align="center">

### 🚀 DATA → INSIGHTS → DECISIONS → BUSINESS GROWTH

**Built with 📊 Data Analytics + 🧠 Business Intelligence + 🎯 Executive Thinking**

</p>

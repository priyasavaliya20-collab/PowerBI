# 📊 Sales Performance Dashboard — Power BI End-to-End Project

A complete Power BI project covering **Data Modeling**, **DAX Measures & Calculated Columns**, **Time Intelligence**, and a fully interactive **Multi-Page Dashboard** with drillthrough, tooltips, and parameter pages.

---

## 🚀 Project Overview

This project demonstrates a real-world Power BI reporting solution built on a **Star Schema** data model. The report includes four analysis pages, a drillthrough page, and a tooltip page — all styled with a dark navy theme for a polished, professional look.

### 🗂️ Project Files

| File Name | Description |
|-----------|-------------|
| 📄 `Sales_Dashboard.pbix` | Main Power BI file with all pages, DAX, and relationships |
| 📘 `README.md` | Project documentation (this file) |
| 📁 `File used sale` | file used in project  |
| 🖼️ Dashboard Preview | Preview images for each report page |

---

## 📁 Data Sources & Tables

| Table | Type | Key Columns |
|-------|------|-------------|
| `Sales_Fact` | Fact | SaleID, CustomerID, ProductID, DateID, TotalAmount, UnitsSold |
| `Customer_Dim` | Dimension | CustomerID, FirstName, LastName, Region |
| `Product_Dim` | Dimension | ProductID, ProductName, Category, SubCategory, Price |
| `Date_Dim` | Dimension | DateID, Date, Month, Quarter, Year |
| `Region_Dim` | Dimension | RegionID, RegionName, ManagerName |
| `Returns_Fact` | Fact | ReturnID, SaleID, ReturnDate, ReturnReason |

---

## 🧩 Task Breakdown

---

### 🔹 Task 1 — Data Modeling

Built a clean **Star Schema** with `Sales_Fact` at the center connected to all dimension tables.

**Relationships created:**

| From Table | To Table | Join Key | Cardinality |
|------------|----------|----------|-------------|
| Sales_Fact | Customer_Dim | CustomerID | Many → 1 |
| Sales_Fact | Product_Dim | ProductID | Many → 1 |
| Sales_Fact | Date_Dim | DateID | Many → 1 |
| Sales_Fact | Region_Dim | RegionID | Many → 1 |
| Returns_Fact | Sales_Fact | SaleID | Many → 1 |

**Additional steps:**
- ✅ Applied single cross-filter direction on all relationships
- ✅ Hidden unnecessary foreign key columns from Report View (e.g., CustomerID, ProductID in Fact tables)
- ✅ Marked `Date_Dim[Date]` as the primary date table

---

### 🔹 Task 2 — DAX Measures & Calculated Columns

#### 📐 Measures

**CALCULATE + FILTER** — Filter rows where TotalAmount > 1000:
```dax
HIGH SALES = 
CALCULATE(
    [TOTAL SALES],
    FILTER(Sales_Fact, Sales_Fact[TotalAmount] > 1000)
)
```

**ALL** — Find the top-selling category ignoring all filters:
```dax
Top Category = 
CALCULATE(
    FIRSTNONBLANK(Product_Dim[Category], 1),
    TOPN(1, ALL(Product_Dim[Category]), [TOTAL SALES], DESC)
)
```

**SUMX + RELATED** — Calculate revenue by multiplying units sold by product price:
```dax
REVENUE = 
SUMX(
    Sales_Fact,
    Sales_Fact[UnitsSold] * RELATED(Product_Dim[Price])
)
```

**COUNTX** — Count unique customer transactions:
```dax
CUSTOMER COUNT = COUNTX(Sales_Fact, Sales_Fact[CustomerID])
```

**AVERAGEX** — Average sales amount per transaction:
```dax
AVG SALES = AVERAGEX(Sales_Fact, Sales_Fact[TotalAmount])
```

**SWITCH** — Classify sales into High / Medium / Low buckets:
```dax
SALES CATEGORY = 
SWITCH(
    TRUE(),
    [TOTAL SALES] > 5000, "High",
    [TOTAL SALES] > 2000, "Medium",
    "Low"
)
```

---

#### 🧮 Calculated Columns

**Full Name** — Concatenate first and last name:
```dax
Full Name = 
Customer_Dim[FirstName] & " " & Customer_Dim[LastName]
```

**Year Month Formatting:**
```dax
Year Month = FORMAT(Date_Dim[Date], "YYYY-MM")
```

**Margin Category** — Classify products by profit margin:
```dax
Margin Category = 
SWITCH(
    TRUE(),
    [Profit Margin %] < 0.20, "Low Margin",
    [Profit Margin %] < 0.40, "Medium Margin",
    [Profit Margin %] < 0.60, "High Margin",
    "Super Margin"
)
```

---

### 🔹 Task 3 — Time Intelligence

**Year-over-Year % Change:**
```dax
TotalAmount YoY% = 
VAR __PREV_YEAR =
    CALCULATE(
        SUM('Sales_Fact'[TotalAmount]),
        DATEADD('Date_Dim'[Date], -1, YEAR)
    )
RETURN
    DIVIDE(SUM('Sales_Fact'[TotalAmount]) - __PREV_YEAR, __PREV_YEAR)
```

**Month-over-Month % Change:**
```dax
TotalAmount MoM% = 
VAR __PREV_MONTH =
    CALCULATE(
        SUM('Sales_Fact'[TotalAmount]),
        DATEADD('Date_Dim'[Date], -1, MONTH)
    )
RETURN
    DIVIDE(SUM('Sales_Fact'[TotalAmount]) - __PREV_MONTH, __PREV_MONTH)
```

**Year-to-Date Sales:**
```dax
TotalAmount YTD = 
TOTALYTD(SUM('Sales_Fact'[TotalAmount]), 'Date_Dim'[Date])
```

---

### 🔹 Task 4 — Dashboard Layout 

#### 📄 Home Page  — Sales Performance Dashboard

<img width="1288" height="862" alt="Screenshot 2026-04-30 102014" src="https://github.com/user-attachments/assets/3c8fd790-da31-483e-8f6f-d28e8508e469" />

---

#### 📄 Page 1 — Sales Performance Dashboard
---

<img width="1326" height="746" alt="Screenshot 2026-04-30 101223" src="https://github.com/user-attachments/assets/ea0d3c2f-e25b-447a-9175-064630168e51" />

---

The main overview page showing company-wide KPIs and trends.

**KPI Cards:**
- Total Sales: **844.02K**
- Revenue: **1.39M**
- Total Orders: **1,000**
- Total Quantity: **5,502**

**Visuals:**
- 📊 Bar Chart — Sales by Region 
- 🍩 Donut Chart — Sales by Category 
- 📈 Area Chart — HIGH SALES vs TOTAL SALES by ProductID
- 📉 Line Chart — Total Sales by Year, Quarter, Month, Day 

**Filters (Right Panel):**
- Date range slicer 
- Full Name slicer
- RegionName slicer 
- Category slicer 

---

#### 📄 Page 2 — Customer Analysis
---

<img width="1277" height="718" alt="Screenshot 2026-04-30 113627" src="https://github.com/user-attachments/assets/4c0a03c3-88b4-460c-87f3-99a16a2d05a9" />

---

Deep-dive into customer-level performance.

**KPI Cards:**
- Total Customers: **198**
- Return Rate %: **0.05**
- Avg Sales per Customer: **4.26K**

**Visuals:**
- 📊 Horizontal Bar Chart — Total Sales by Full Name 
- 📋 Table — Full Name, Total Sales, Total Returns 
- 🍩 Donut Chart — Total Customers by Segment 
- 🎯 Gauge Chart — Return Rate % 

**Filters (Right Panel):**
- Region slicer
- Segment slicer (Consumer, Corporate, Home Office)
- Year slicer (2024, 2025)

---

#### 📄 Page 3 — Product Analysis

---

<img width="1277" height="719" alt="Screenshot 2026-04-30 113643" src="https://github.com/user-attachments/assets/8506dae5-5185-4e08-895b-99c5ec619e19" />

--

Product-level breakdown across categories.

**KPI Cards:**
- Total Products: **100**
- Sales Category: **High**
- Top Category: **Office Supplies**

**Category Summary Table:**

| Category | Total Sales | Total Products | Total Quantity | Revenue |
|----------|-------------|----------------|----------------|---------|
| Furniture | 2,74,569.59 | 35 | 1,805 | 4,87,334.73 |
| Office Supplies | 3,03,150.69 | 34 | 1,943 | 5,27,766.96 |
| Technology | 2,66,296.67 | 31 | 1,754 | 3,74,700.74 |
| **Total** | **8,44,016.95** | **100** | **5,502** | **13,89,802.43** |

**Visuals:**
- 📊 Horizontal Bar Chart — Total Sales by ProductName
- 📋 Summary Matrix Table — Category × Sales, Products, Quantity, Revenue

**Filters (Right Panel):**
- Category slicer (Furniture, Office Supplies, Technology)
- Year slicer (2024, 2025)

---

#### 📄 Page 4 — Parameter Page

---

<img width="1312" height="736" alt="Screenshot 2026-04-30 101324" src="https://github.com/user-attachments/assets/36727d42-cd6a-4d4f-adf2-c655179d639b" />

---

A dynamic analysis page powered by **Field Parameters** and **Numeric Range Parameters**.

**Controls:**
- **Year Filter** — 2024 / 2025 (checkbox slicer)
- **Chosen Measure** — Toggle between: Total Sales / Total Returns / Return Rate %
- **Period Choice** — Switch between: Year / Quarter / Month (radio button slicer)

**Output:**
- Dynamic matrix showing returns by customer name and time period
- For 2025 filtered to "Total Returns" by Quarter: 20 total returns across Q1 and Q2 from 12 customers (David Chambers led with 2 returns)

---

#### 📄 Page 5 — Drillthrough Page

---

<img width="1276" height="717" alt="Screenshot 2026-04-30 113655" src="https://github.com/user-attachments/assets/a20c9784-1d1b-48ff-8f12-fc5277dedcb5" />

---

<img width="654" height="622" alt="Screenshot 2026-04-30 114402" src="https://github.com/user-attachments/assets/6355b74a-f22d-4bce-8335-fbe41529184e" />

---


----

Context-sensitive drillthrough for customer-level return analysis.

**Triggered by:** Right-clicking a customer in another page → Drillthrough → Drillthrough Page

**KPI Cards:**
- Total Returns: **1**
- Return Rate %: **0.17**
- YTD Returns: **1**

**Visuals:**
- 📊 Bar Chart — Total Returns by ReturnReason (e.g., "Customer Changed Mind")
- 📋 Detail Table — ReturnID, Full Name, ProductName, ReturnReason

**Example (Anthony Ramos drillthrough):**

| ReturnID | Full Name | Product Name | Return Reason |
|----------|-----------|--------------|---------------|
| — | Anthony Ramos | Empower Rich Mindshare | — |
| — | Anthony Ramos | Harness Dynamic Vortals | — |
| — | Anthony Ramos | Maximize Innovative Interfaces | — |
| 46 | Anthony Ramos | Implement World-Class Technologies | Customer Changed Mind |

---

#### 📄 Page 6 — Tooltip Page

---

<img width="637" height="400" alt="Screenshot 2026-04-30 113718" src="https://github.com/user-attachments/assets/5c7e9389-db81-4737-9299-fb9cf158c1ae" />


---

<img width="674" height="428" alt="Screenshot 2026-04-30 114206" src="https://github.com/user-attachments/assets/89be8606-d0d3-44bd-9200-f3d0ddb5a5cf" />

---



A compact tooltip card that appears on hover across visuals.

**Displays:**
- Total Sales: **844.02K**
- Total Returns: **50**

Used as a custom tooltip across the Sales Performance Dashboard page visuals.



---

---

📌 Bookmarks

---

<img width="955" height="86" alt="Screenshot 2026-04-30 114649" src="https://github.com/user-attachments/assets/26fc78b2-a91c-455b-be95-be8273d1e77a" />


---
<img width="959" height="93" alt="Screenshot 2026-04-30 114603" src="https://github.com/user-attachments/assets/1dd06abb-94fa-4f1f-83ff-a72d51d29710" />


---

 Conditional Formatting :-
---

<img width="266" height="237" alt="Screenshot 2026-04-30 121038" src="https://github.com/user-attachments/assets/f6b033b1-a335-4284-a272-19796d049dee" />

---

## 📊 Data Model — Star Schema

---

<img width="1162" height="738" alt="Screenshot 2026-04-30 101516" src="https://github.com/user-attachments/assets/ca0d0f17-e6bc-4c13-9acc-6a2e696494ae" />

---

## Mobile Layout :-

---

 <img width="824" height="597" alt="image" src="https://github.com/user-attachments/assets/136428a0-c45a-4c45-a340-5cc0c5958bc8" />

 
---

## 📌 Key Insights from the Dashboard

- ✅ **South region** leads in sales at **0.23M**, followed closely by North (0.22M)
- ✅ **Office Supplies** is the top-performing category with **₹3,03,150** in total sales
- ✅ **Michael Rich** and **Brent Green** are the top two customers by total sales
- ✅ Return rate is very low at **0.05%** — indicating high customer satisfaction
- ✅ **"Customer Changed Mind"** is the most common return reason (visible via drillthrough)
- ✅ Sales peaked around **Nov 2024 – Jan 2025** in the time series trend

---

## ⚠️ Challenges & Solutions

| Challenge | Solution |
|-----------|----------|
| Building YoY/MoM without an active date table on Returns | Used `DATEADD` on `Date_Dim[Date]` with the active relationship |
| Drillthrough not showing context | Set drillthrough filter field to `Full Name` from `Customer_Dim` |
| Tooltip page appearing too large | Set page type to "Tooltip" in Page Information settings |
| Parameter slicer not syncing across pages | Used Page-level filters and synced slicer panels |
| SWITCH classification not updating dynamically | Moved SWITCH logic from calculated column to a DAX measure |

---

## 🛠️ Tools & Features Used

| Tool / Feature | Usage |
|----------------|-------|
| Power BI Desktop | Report authoring, model view, DAX editor |
| Power Query | Data type cleanup, column renaming, null removal |
| DAX | Measures, Calculated Columns, Time Intelligence |
| Field Parameters | Dynamic measure switching on Page 4 |
| Drillthrough | Customer-level return detail on Page 5 |
| Custom Tooltip | KPI hover card on Page 6 |
| Star Schema | Central Sales_Fact with 4 surrounding dimensions |

---

## 📈 How to Use

1. Download and open `Sales_Dashboard.pbix` in **Microsoft Power BI Desktop**
2. Navigate between the 6 pages using the tabs at the bottom
3. Use the **right-side slicers** on Pages 1–3 to filter by Region, Category, Year, or Customer
4. On **Page 4**, toggle the Chosen Measure and Period Choice to explore dynamic analysis
5. On **Page 2**, right-click any customer bar → **Drillthrough → Drillthrough Page** to see return details
6. Hover over any bar or line on **Page 1** to see the custom tooltip card

---

## 🌟 Future Enhancements

- [ ] Add Row-Level Security (RLS) by Region or Manager
- [ ] Connect to a live SQL Server or Azure database
- [ ] Add forecasting visuals using Power BI's built-in analytics pane
- [ ] Publish to Power BI Service and schedule automatic refresh
- [ ] Build a mobile layout version for the Sales Performance page

---

👩‍💻 **Priya Savaliya**  
📍 Ahmedabad, Gujarat

> ⭐ If you found this helpful, give the repository a star and feel free to fork or contribute!

🗂️ *Clean Models · Sharp DAX · Confident Insights*

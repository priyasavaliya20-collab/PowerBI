# 📊 DAX Depo – Advanced Calculations Using DAX in Power BI

An advanced Power BI DAX project focused on building calculated columns, measures, filter context behavior, time intelligence, quick measures, and iterator functions — all organized in a structured, multi-tab report layout.

---

## 🎬 Project Demo Video

▶ **[WATCH DEMO – GOOGLE DRIVE](#)**

---

## 🚀 Project Overview

This project demonstrates end-to-end **DAX (Data Analysis Expressions)** inside Microsoft Power BI Desktop. The focus is purely on DAX calculations across multiple categories — no complex visuals, just clean logic verified through Matrix tables and Card visuals.

---

## 📁 Data Sources

| File | Key Columns |
|------|-------------|
| 📊 `Sales_Fact` | SalesID, CustomerID, ProductID, RegionID, DateKey, SalesAmount, Cost, Quantity |
| 👥 `Customer_Dim` | CustomerID, FirstName, LastName, Segment |
| 📦 `Product_Dim` | ProductID, ProductName, Category, Subcategory |
| 🌍 `Region_Dim` | RegionID, RegionName |
| 📅 `Date_Dim` | DateKey, Date, Month, Quarter, Year |
| 🔄 `Returns_Fact` | ReturnID, SalesID, ReturnDateKey, Reason |

---

## 🔗 Relationships & Data Model

![Relationships](PR_3_images/Relationships.png)

![Joining and Relationships](PR_3_images/Joining%20and%20relationship.png)

---

## 🧩 Project Tasks Breakdown

---

### 🔹 1️⃣ Calculated Columns

#### Profit & ReturnFlag — `Sales_Fact`

```dax
Profit = 'Sales_Fact'[SalesAmount] - Sales_Fact[Cost]
ReturnFlag =
IF(
    COUNTROWS(
        FILTER(
            Returns_Fact,
            Returns_Fact[SalesID] = Sales_Fact[SalesID]
        )
    ) > 0,
    "Returned",
    "Not Returned"
)

Customer Full Name — Customer_Dim
Customer Full Name =
Customer_Dim[FirstName] & " " & Customer_Dim[LastName]

UPPER & LEFT

IF, AND, OR, SWITCH

YEAR, EOMONTH

🔹 2️⃣ Measures
Total Sales = SUM(Sales_Fact[SalesAmount])

🔹 3️⃣ Measure Management

All DAX measures are stored in a dedicated _Measures table for clean organization.

🔹 4️⃣ Filter Context & Behavior
North Region Sales =
CALCULATE(
    [Total Sales],
    FILTER(Region_Dim, Region_Dim[RegionName] = "North")
)
Sales % of Total =
DIVIDE(
    [Total Sales],
    CALCULATE([Total Sales], ALL(Region_Dim)),
    0
) * 100
Total Sales All Regions =
CALCULATE(
    [Total Sales],
    ALL(Region_Dim)
)

🔹 5️⃣ DAX Operators

🔹 6️⃣ Quick Measures

🔹 7️⃣ Time Intelligence
Sales YTD =
TOTALYTD(
    [Total Sales],
    Date_Dim[Date]
)
Sales Last Year =
CALCULATE(
    [Total Sales],
    SAMEPERIODLASTYEAR(Date_Dim[Date])
)
Running Total Sales =
CALCULATE(
    [Total Sales],
    DATESBETWEEN(
        Date_Dim[Date],
        MIN(Date_Dim[Date]),
        MAX(Date_Dim[Date])
    )
)
Sales Last 3 Months =
CALCULATE(
    [Total Sales],
    DATESINPERIOD(
        Date_Dim[Date],
        MAX(Date_Dim[Date]),
        -3,
        MONTH
    )
)

🔹 8️⃣ Additional Scenarios — Iterator Functions
TotalRevenue =
SUMX(
    Sales_Fact,
    Sales_Fact[Quantity] * Sales_Fact[Cost]
)
AvgOrderValue =
AVERAGEX(
    Sales_Fact,
    Sales_Fact[Quantity] * Sales_Fact[Cost]
)
Margin Category =
SWITCH(
    TRUE(),
    (Sales_Fact[SalesAmount] - Sales_Fact[Cost]) < 100, "Low Margin",
    (Sales_Fact[SalesAmount] - Sales_Fact[Cost]) < 300, "Medium Margin",
    (Sales_Fact[SalesAmount] - Sales_Fact[Cost]) < 800, "High Margin",
    "Super Margin"
)

🔹 9️⃣ Output Requirement

🛠️ Tools & Technologies Used
Tool	Features Used
Power BI Desktop	Report View, Table View, Model View
DAX	Calculated Columns, Measures, Iterator Functions, Time Intelligence
Power Query	Data import, type casting, blank row removal
Quick Measures	YoY Growth %, MoM% auto-generation
Fields Pane	Measure table organization

📈 How to Use
Download the repository
Open DAX_DEPO.pbix in Power BI Desktop
Navigate across report tabs
Use Table View for row-level validation
Check _Measures table
👩‍💻 Shruti Bhawsar

📍 Ahmedabad

⭐ If you like this project, give it a star!

🗂️ Clean DAX. Clear Context. Confident Insights.

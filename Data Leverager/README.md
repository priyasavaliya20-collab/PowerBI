# 📊 Data Leverager – Power Query Transformation Project

> An end-to-end ETL simulation project built using Power BI's Power Query Editor to extract, clean, reshape, and combine data from multiple sources using essential data engineering techniques.

---

## 🎬 Project Demo Video

[![Watch the Project Walkthrough](https://img.shields.io/badge/▶%20Watch%20Demo-Google%20Drive-blue?style=for-the-badge&logo=google-drive)](https://drive.google.com/file/d/1x6efKuSzRKSjEv3kJi2alaJJ4lJl6E7Q/view?usp=sharing)

> 📺 Click the badge above to watch the full project walkthrough video on Google Drive.

---

## 🖼️ Project Preview

### Overview — Hero & Module Cards
![Data Leverager Preview 1](Project%20preview/Preview%20(1).png)

### Modules 04–08 — Numeric, Date/Time, Logic, Reshape, Combine
![Data Leverager Preview 2](Project%20preview/Preview%20(2).png)

### Modules 09–12 — Aggregate, Quality, Config, Simulate & Submission Checklist
![Data Leverager Preview 3](Project%20preview/Preview%20(3).png)

---

## 🚀 Project Overview

This project presents a complete **Power Query ETL Pipeline** built inside **Microsoft Power BI Desktop**. It demonstrates real-world data engineering skills including:

| Feature | Description |
|---|---|
| 📂 Multi-Source Extraction | Sales files (Jan, Feb, Mar) loaded via Folder Source + Append |
| 🧹 Data Cleaning | Removed blanks, duplicates, renamed columns, fixed data types |
| 🔤 Text Transformations | UPPER, LOWER, TRIM, REPLACE, Split Column |
| 🔢 Numeric Transformations | Rounding, Custom Profit Column |
| 📅 Date & Time Tools | Day, Month, Year, Quarter, Fiscal Month, Age |
| 🔀 Pivot & Unpivot | Region-wise reshaping of sales data |
| 🔗 Merge & Append | Fuzzy Nested Join of employees + sales on Region |
| 📊 Group By & Aggregation | Total Sales, Avg Order Value, Transaction Count |
| 🔍 Data Profiling | Column Quality, Distribution, and Profile views |
| ⚙️ Dynamic Parameters | FolderPath parameter for dynamic data source |
| 🔄 Refresh Simulation | Auto-loaded April data via folder source |

---

## 🗂️ Project Files

| File Name | Description |
|---|---|
| 📄 `PR_1_DATA_LEVERAGER.pbix` | Main Power BI file with all 13 Power Query transformations |
| 📘 `README.md` | Project documentation (this file) |
| 🎬 Video | [Project walkthrough video](https://drive.google.com/file/d/1x6efKuSzRKSjEv3kJi2alaJJ4lJl6E7Q/view?usp=sharing) |
| 📷 Project preview | Explanation of project in images |
| 📁 Files Used | Files used in project |

---

## 📁 Data Sources

| Source | Details |
|---|---|
| 📂 Folder Source | `Sales_Jan.xlsx`, `Sales_Feb.xlsx`, `Sales_Mar.xlsx` — loaded via Append Queries from Folder |
| 📄 Excel File | `Employees.xlsx` — EmployeeID, Name, Department, Region, BirthDate, Joining Date |
| 🌐 Web Table | Worldometers HTML table loaded using the Web Connector |

---

## 🧩 Transformation Tasks Breakdown

### 🔹 Task 1 — Data Extraction
- Loaded 3 monthly sales files using **Folder Source → Append Queries from Folder**
- Loaded `Employees.xlsx` as a separate Excel connection
- Loaded an HTML table from Worldometers using the **Web Connector**

---

### 🔹 Task 2 — Basic Transformations
- Removed blank rows and columns from sales data
- Promoted first row to headers
- Renamed columns — `Product` → `Product Name`, `JoinDate` → `Joining Date`
- Changed data types of `Revenue` and `Cost` using **Change Type with Locale**
- Removed duplicate rows

---

### 🔹 Task 3 — Text Tools

Applied on `CustomerName`, `City`, `Revenue`, `Region` in sales data:

| Function | Column Applied |
|---|---|
| `UPPER()` | CustomerName |
| `LOWER()` | City |
| `TRIM()` | Region, CustomerName |
| `REPLACE()` | Revenue |
| Split Column by Delimiter | CustomerName → `.1` & `.2` |

---

### 🔹 Task 4 — Numeric Tools
- Rounded `Revenue` column to **2 decimal places**
- Created custom column: `Profit = Revenue - Cost`

---

### 🔹 Task 5 — Date & Time Tools
- Extracted **Day, Month, Year, Quarter** from `Order Date`
- Created **Fiscal Month** column via Add Column → Date → Month
- Added calculated **Age** column from `BirthDate` in employees table

---

### 🔹 Task 6 — Conditional Columns & Indexing

| Condition | Category |
|---|---|
| Revenue >= 10,000 | 🟢 High |
| Revenue >= 5,000 | 🟡 Medium |
| Otherwise | 🔴 Low |

- Added **Index Column from 0** (0-based)
- Added **Index Column from 1** (1-based)

---

### 🔹 Task 7 — Pivot & Unpivot
- **Pivot:** Region column → Values: Revenue (Region rows → columns)
- **Unpivot:** Unpivoted columns back to normalized form

---

### 🔹 Task 8 — Merging & Appending
- **Append:** Jan + Feb + Mar already combined via Folder Source ✅
- **Merge:** Fuzzy Nested Join on `Region` column

---

### 🔹 Task 9 — Grouping & Aggregation

Grouped by `Source.Name` (month/file):

| File | Total Sales | Avg Order Value | Transaction Count |
|---|---|---|---|
| Sales_Jan.xlsx | ₹9,24,878.91 | ₹23,714.84 | 39 |
| Sales_Feb.xlsx | ₹9,37,628.91 | ₹24,041.76 | 39 |
| Sales_Mar.xlsx | ₹9,62,280.00 | ₹24,057.00 | 40 |

---

### 🔹 Task 10 — Data Profiling & Quality

Enabled **Column Quality**, **Column Distribution**, **Column Profile** from the View tab.

| Column | Distinct Values | Valid | Errors | Empty |
|---|---|---|---|---|
| CustomerName | 85 | 100% | 0% | 0% |
| Region | 5 | 100% | 0% | 0% |
| Product | 8 | 100% | 0% | 0% |

---

### 🔹 Task 11 — Source Settings & Parameters
- Created **Parameter `FolderPath`** (Type: Text) with current folder path as default
- Used parameter to make folder source **dynamic**
- Changed data source credentials for `Employees.xlsx` via **Data Source Settings** ✅

---

### 🔹 Task 12 — Refresh Simulation
- Copied an existing file → renamed `Sales_Apr.xlsx` → placed in same folder
- Clicked **Refresh** in Power BI → April data loaded automatically ✅
- All previous transformations carried over successfully ✅

---

## 🗃️ Query Summary

**Total Queries: 13**

| Query Name | Description |
|---|---|
| `sales data` | Main cleaned & transformed sales table (Tasks 2–6) |
| `employees` | Employees dataset with age, date parts, renamed columns |
| `web table` | Worldometers HTML table via Web Connector |
| `Merge1` | Fuzzy join of employees + sales data on Region |
| `Group by` | Sales aggregated by source file (Total, Avg, Count) |
| `Data Profiling` | Column quality and distribution profiling view |
| `Pivot & unpivot` | Region-pivoted and unpivoted sales data |
| `april_sales` | Refresh simulation with `Sales_Apr.xlsx` |
| `folder path` | Parameter-driven dynamic folder path query |
| `Parameter1` | FolderPath parameter (Helper Queries folder) |
| `Sample File` | Helper query for folder source |
| `Transform File` | Custom function for file transformation |
| `Transform Sa...` | Transform Sample file query |

---

## ⚠️ Challenges Faced & How I Solved Them

| Challenge | How It Was Solved |
|---|---|
| Web connector not loading table | Used a stable Worldometers URL and selected the correct table from Navigator |
| Change Type with Locale errors | Selected **English (India)** as locale for Revenue and Cost columns |
| Data Source Settings path error after moving files | Re-entered correct path in Data Source Settings and refreshed |

---

## 🛠️ Tools & Technologies Used

| Tool | Features Used |
|---|---|
| **Power BI Desktop** | Power Query Editor, M Language, Data Source Settings |
| **Power Query** | Folder Source, Web Connector, Append, Merge, Group By, Pivot/Unpivot |
| **M Query Language** | Custom columns, Split, FuzzyNestedJoin, Change Type with Locale |
| **Excel** | Source files — `Sales_Jan/Feb/Mar.xlsx`, `Employees.xlsx` |

---

## 📌 Key Insights Derived

- ✔️ **March had the highest total sales** at ₹9,65,290 across 40 transactions
- ✔️ All 3 months had **nearly equal average order values** (~₹24,000)
- ✔️ **Fuzzy join** successfully matched Region values despite case/space inconsistencies
- ✔️ Data profiling confirmed **100% data quality** — no errors, nulls, or duplicates
- ✔️ **Dynamic parameter setup** makes the pipeline reusable across any folder path
- ✔️ **Refresh simulation** proved the ETL pipeline is fully automated for new monthly data

---

## 📈 How to Use

1. Download the repository
2. Open `PR_1_DATA_LEVERAGER.pbix` in **Microsoft Power BI Desktop**
3. Go to **Transform Data** to open Power Query Editor
4. Navigate through the 13 queries in the **Queries pane** on the left
5. Update the **FolderPath parameter** if your sales files are in a different location
6. Click **Close & Apply** to load transformations into the data model

---

## 🌟 Future Enhancements

- [ ] Add DAX Measures & KPI Cards in Power BI Report View
- [ ] Connect to a live SQL Server or SharePoint data source
- [ ] Automate refresh via Power BI Service + Scheduled Refresh
- [ ] Build an interactive Sales Dashboard on top of this ETL layer
- [ ] Add incremental refresh for large monthly datasets

---

## 👩‍💻 Author

**Priya Savaliya**  
📍 Ahmedabad

---

> ⭐ If you found this project useful, give it a **star** and feel free to fork or contribute!

*📊 Turning Raw Data into Clean, Structured, Actionable Pipelines*

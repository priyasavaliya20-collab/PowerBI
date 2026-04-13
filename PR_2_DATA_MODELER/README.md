# 🗂️ Data Modeler – Building a Normalized Star Schema Data Model

An advanced Power BI data modeling project focused on building a structured, normalized relational data model with Star and Snowflake schemas, relationship cardinality, data categories, hierarchies, and verification using Matrix tables.

---

## 🎬 Project Demo Video

[![▶ WATCH DEMO – GOOGLE DRIVE](https://img.shields.io/badge/▶%20WATCH%20DEMO-GOOGLE%20DRIVE-4285F4?style=for-the-badge&logo=googledrive&logoColor=white)](https://drive.google.com/file/d/1Ax2q81JJfqHjOYBV_VfqNsYL90CqvuoO/view?usp=sharing)

> 🖥️ Click the badge above to watch the full project walkthrough video on Google Drive.

---

## 🚀 Project Overview

This project demonstrates end-to-end **Data Modeling** inside Microsoft Power BI Desktop. No calculated columns, DAX measures, or visual charts are used — the focus is purely on:

- 🏗️ **Schema Design** — Star Schema (primary) + Snowflake Schema (secondary layout)
- 🔗 **Relationships** — Defined cardinality (1:Many, Many:1, 1:1) between all tables
- 🔁 **Inactive Relationships** — Handled for `ReturnDateKey` in `Return_fact`
- 🧭 **Hierarchies** — Built for Date, Region, and Product dimensions
- 🏷️ **Data Categories** — Assigned City, Country/Region, State, Postal Code, etc.
- 📐 **Data Formats** — Currency for Revenue, Whole Numbers for IDs, Date type for date fields
- ✅ **Verification** — Matrix Table used to verify relationship flows

---

## 🗂️ Project Files

| File Name | Description |
|-----------|-------------|
| 📄 PR_2_DATA_MODELER.pbix | Main Power BI file with all tables, relationships, hierarchies |
| 📘 README.md | Project documentation (this file) |
| 📁 Files Used | Source Excel files used in the project |
| 🖼️ project_preview.png | explanation of project through image |

---

## 📁 Data Sources

| File | Columns |
|------|---------|
| 📊 Sales_Fact.xlsx | SalesID (PK), CustomerID (FK), ProductID (FK), RegionID (FK), DateKey (FK), Quantity, Revenue, Discount |
| 👥 Customer_Dim.xlsx | CustomerID (PK), FullName, Age, Gender, Segment |
| 📦 Product_Dim.xlsx | ProductID (PK), ProductName, Category, Subcategory, Brand |
| 🌍 Region_Dim.xlsx | RegionID (PK), Country, State, City |
| 📅 Date_Dim.xlsx | DateKey (PK), Date, Month, Quarter, Year, Fiscal Year |
| 🔄 Returns_Fact.xlsx | ReturnID (PK), SalesID (FK → Sales_Fact), ReturnDateKey (FK), Reason |

---


## 📸 Project Preview

![Project Preview](project%20preview.png)


---

## 🧩 Project Tasks Breakdown

### 🔹 1️⃣ Data Extraction & Cleaning (Power Query)
- Imported all 6 source Excel files via Power Query
- Applied proper data types to each column
- Removed blank rows and null values
- Promoted headers where needed
- Loaded all cleaned tables into the Power BI Data Model

### 🔹 2️⃣ Model Construction & Relationships (Model View)

Defined the following relationships with cardinality and single cross-filter direction:

| From Table | To Table | Join Key | Cardinality | Status |
|------------|----------|----------|-------------|--------|
| Sales_fact | Customer_dim | CustomerID | Many → 1 | Active |
| Sales_fact | Product_dim | ProductID | Many → 1 | Active |
| Sales_fact | Reigon_dim | RegionID | Many → 1 | Active |
| Sales_fact | Date_dim | DateKey | Many → 1 | Active |
| Return_fact | Sales_fact | SalesID | Many → 1 | Active |
| Return_fact | Date_dim | ReturnDateKey | Many → 1 | **Inactive** |

> ⚠️ The `Return_fact → Date_dim` relationship on `ReturnDateKey` is set as **inactive** to avoid filter ambiguity since `Date_dim` is already connected via `Sales_fact → DateKey`.

### 🔹 3️⃣ Schema Design

**Star Schema** (primary model layout):
- `Sales_fact` is the central fact table
- All 4 dimension tables (`Customer_dim`, `Product_dim`, `Reigon_dim`, `Date_dim`) connect directly to `Sales_fact`
- `Return_fact` connects to `Sales_fact` as a second fact table

**Snowflake Schema** (alternate layout tab):
- Same tables arranged to visually represent the extended hierarchy structure
- `Return_fact` connects to `Date_dim` with the inactive relationship visible

### 🔹 4️⃣ Advanced Model Settings

- Set **single cross-filter direction** on all relationships (no bidirectional unless justified)
- Simulated **inactive relationship** using `Return_fact → Date_dim` on `ReturnDateKey`
- Identified and resolved **filter ambiguity** between `Return_fact` and `Date_dim`

### 🔹 5️⃣ Data Model Enhancements

**Data Formats set:**

| Column | Format |
|--------|--------|
| Revenue | Currency (₹), 2 decimal places, Sum |
| Discount | Currency |
| Quantity | Whole Number |
| DateKey, ReturnDateKey | Whole Number |
| Date (Date_dim) | Date |
| Age, IDs | Whole Number |

**Data Categories assigned:**

| Column | Category |
|--------|----------|
| City | City |
| Country | Country/Region |
| State | State or Province |
| ProductName | Postal Code (as label category) |

### 🔹 6️⃣ Hierarchies Built

**Date_dim → Date Hierarchy:**
```
Year → Quarter → Month → Date
```

**Reigon_dim → Region Hierarchy:**
```
Country → State → City
```

**Product_dim → Product Hierarchy:**
```
Category → Subcategory → ProductName
```

### 🔹 7️⃣ Verification – Matrix Tables

Used Matrix visuals to verify all relationship flows:

| Matrix | Rows | Columns | Values |
|--------|------|---------|--------|
| Matrix 1 | Product Category | Region (Germany / India / USA) | Sum of Revenue |
| Matrix 2 | Fiscal Year | Return Reason (Damaged / Not Needed / Wrong Item) | Return Count |
| Matrix 3 | Customer Segment | — | Sum of Revenue |

**Matrix Results:**

### 🔹 1️⃣ Sales Group by Product Category and Region

| Category | Germany | India | USA | Total |
|----------|---------|-------|-----|-------|
| Clothing | ₹26,521 | ₹27,725 | ₹38,015 | ₹92,261 |
| Electronics | ₹26,489 | ₹31,692 | ₹44,712 | ₹1,02,893 |
| Furniture | ₹23,990 | ₹26,347 | ₹41,435 | ₹91,772 |
| **Total** | **₹77,000** | **₹85,764** | **₹1,24,162** | **₹2,86,926** |

### 🔹 2️⃣ Return Reason by Fiscal Year

| Fiscal Year | Damaged | Not Needed | Wrong Item | Total |
|-------------|---------|------------|------------|-------|
| FY2022 | 15 | 17 | 15 | 47 |
| FY2023 | 19 | 18 | 16 | 53 |
| **Total** | **34** | **35** | **31** | **100** |

### 🔹3️⃣ Revenue By Customer Segment 

| Segment | Sum of Revenue |
|---------|---------------|
| Gold | ₹95,597 |
| Platinum | ₹93,893 |
| Silver | ₹97,436 |
| **Total** | **₹2,86,926** |

---

## 📌 Key Insights from the Model

✔️ Electronics was the highest revenue category at ₹1,02,893 across all regions

✔️ USA contributed the most revenue (₹1,24,162) among all regions

✔️ Return volume increased from FY2022 (47) to FY2023 (53), with "Not Needed" being the top reason

✔️ All 3 customer segments (Gold, Platinum, Silver) had nearly equal revenue distribution

✔️ The inactive relationship on `ReturnDateKey` successfully resolved filter ambiguity

✔️ Hierarchies enabled drill-down from Country → State → City and Year → Quarter → Month → Date

---

## ⚠️ Challenges Faced & How I Solved Them

| Challenge | How It Was Solved |
|-----------|------------------|
| Filter ambiguity between `Return_fact` and `Date_dim` | Set `ReturnDateKey` relationship as inactive to eliminate ambiguity |
| Incorrect data category on `ProductName` | Manually reassigned category in Column Tools tab |
| Blank rows appearing after Power Query load | Added `FilterNullAndWhitespace` step + `Removed Blank Rows` in Applied Steps |
| Relationship direction confusion | Set all relationships to single cross-filter; enabled bidirectional only where verified |
| Snowflake layout looked same as Star | Reorganized tables in a separate Model View tab named "snowflake Schema" |

---

## 🛠️ Tools & Technologies Used

| Tool | Features Used |
|------|--------------|
| Power BI Desktop | Model View, Power Query Editor, Matrix Visual |
| Power Query | Data type changes, blank row removal, header promotion |
| Power BI Model View | Relationships, cardinality settings, inactive relationships |
| Column Tools | Data categories, data formats, summarization settings |
| Data Pane (Fields) | Hierarchies, column renaming, sort by column |

---

## 📈 How to Use

1. Download the repository
2. Open `PR_2_DATA_MODELER.pbix` in Microsoft Power BI Desktop
3. Go to **Model View** to explore the Star Schema and Snowflake Schema layouts
4. Navigate to **Table View** to inspect individual tables and their data types
5. Open the **Report View** to see the Matrix verification tables
6. Use the **Fields Pane** to explore hierarchies and data categories

---

## 🌟 Future Enhancements

- [ ] Add DAX Measures (Total Revenue, Return Rate %, Avg Revenue per Customer)
- [ ] Build an interactive Sales & Returns Dashboard on top of this data model
- [ ] Add Row-Level Security (RLS) by Region or Segment
- [ ] Connect to a live SQL Server data source
- [ ] Enable incremental refresh for large datasets via Power BI Service

---

## 👩‍💻Priya Savaliya
📍 Ahmedabad

---

⭐ **If You Like This Project**

Give this repository a ⭐ and feel free to fork or contribute!

> 🗂️ *Clean Models, Clear Relationships, Confident Insights*

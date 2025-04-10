# 📦 SUPPLIER QUALITY AND PERFORMANCE ANALYSIS

![Dashboard Screenshot](https://github.com/user-attachments/assets/b8c93a76-ddc1-4120-a092-70a9734c3434)

---

## 📑 Table of Contents
- [Overview](#overview)
- [Objective](#objective)
- [Data Source](#data-source)
- [Tools Used](#tools-used)
- [Process Workflow](#process-workflow)
- [Key Findings](#key-findings)
- [Recommendations](#recommendations)

---

## 🔍 Overview

Enterprise Manufacturers Ltd. was experiencing operational inefficiencies stemming from inconsistent supplier quality and a fragmented procurement system across its plants. With the recent consolidation of supplier data, the organization aimed to uncover root causes of raw material defects and their financial impact—particularly downtime caused by these issues.

---

## 🎯 Objective

1. **Identify Key Issues:** Pinpoint vendors and plant locations contributing most to defects and downtime.  
2. **Analyze Combinations:** Examine material–vendor–plant combinations to reveal recurring quality issues.  
3. **Support Procurement Decisions:** Build interactive Power BI dashboards to guide procurement strategy and vendor performance management.

---

## 🗂️ Data Source

- **Primary Dataset:** [Supplier_data.xlsx](https://docs.google.com/spreadsheets/d/1nwO4VG5U2cklj5OoDpx6U1j0voGm-9zW/edit?usp=drive_link&ouid=106373350318822195700&rtpof=true&sd=true)  
  This dataset includes details such as material types, vendors, defect types, downtime duration, and plant locations across the enterprise.

---

## 🛠 Tools Used

- **Microsoft Excel** – Initial data exploration  
- **Power Query** – Data transformation and cleaning  
- **Power BI** – Data modeling and dashboard creation

---

## 🔄 Process Workflow

### 1. Data Consolidation & Cleaning
- Merged raw data from multiple plants.
- Standardized vendor naming conventions.
- Addressed inconsistencies across plant entries.
- Cleaned and prepped dataset using Excel and Power Query.

---

### 2. Data Modeling in Power BI

- **Relational Model:** Designed schema linking materials, vendors, defect types, and plants.
- **Custom Date Table:** Generated for time-based analysis using DAX.

```DAX
Date = ADDCOLUMNS(
    CALENDARAUTO(),
    "Year", YEAR([Date]),
    "Quarter", QUARTER([Date]),
    "Month", MONTH([Date]),
    "Month Name", FORMAT([Date], "mmmm"),
    "Is Weekend", IF(WEEKDAY([Date]) IN {1, 7}, TRUE, FALSE)
)
```

- **Sample DAX Measures:**

```DAX
Impact Defect Ratio = 
VAR Impact = CALCULATE(COUNT('Supplier fact'[Defect Type ID]), KEEPFILTERS('Defect Type'[Defect Type] = "Impact"))
VAR TotalDefects = COUNT('Supplier fact'[Defect Type ID])
RETURN DIVIDE(Impact, TotalDefects)
```

```DAX
YOY Downtime Trend Icon = 
VAR Icon = IF([YOYGrowthDowntimeCost] > 0, UNICHAR(9650), UNICHAR(9660))
RETURN Icon & " " & FORMAT([YOYGrowthDowntimeCost], "0.00%")
```

---

### 3. Dashboard & Visualization

Dashboards were built in Power BI to uncover:

- **Top Offending Suppliers & Plants**
- **Downtime Trend Analysis (Monthly & YoY)**
- **Vendor & Material Performance Comparisons Across Plants**
- **Recurring Defect Types by Category**

Key charts include heatmaps, bar charts, matrix tables, and dynamic filtering for drill-down analysis.

---

## 📊 Key Findings

### 💸 **Cost of Downtime**
- Over **$2 million** in downtime costs, with major spikes in **September** and **December**.

![Downtime Cost Chart](https://github.com/user-attachments/assets/a712adc4-1c22-4687-b5e7-0e0675ed77dc)

---

### ⚠️ **Underperforming Vendors**
- Vendors **Avamm**, **Meejo**, and **Yombu** were the most frequent contributors to defects and downtime.

![Vendor Issues](https://github.com/user-attachments/assets/cc388b4b-5c80-4639-8157-475b8436c5a5)

---

### 🏭 **Problematic Plants**
- Plants like **Hingham**, **Charles City**, and **Twin Rocks** recorded defect quantities close to **100 million units**.

![Defect Volumes](https://github.com/user-attachments/assets/ef97ed13-592c-4d14-988d-bcda24dee037)

---

### 🔁 **Recurring Defects**
- Defects such as **bad seams** were especially common, pointing to gaps in quality control protocols.

---

### 🧱 **Material-Level Concerns**
- **Raw materials** were the primary contributors to defect-related downtime across all sites.

![Material Issues](https://github.com/user-attachments/assets/45c5feb3-1c0a-4558-b4ad-fc690a558ff8)

---

## ✅ Recommendations

1. **Centralized Supplier System**  
   Implement a unified supplier management system to maintain consistent records and performance tracking across all locations.

2. **Monetize Defect Impact**  
   Always translate defects and downtime into financial terms to better communicate business impact and prioritize solutions.

3. **Continuous Improvement Programs**  
   Create data-driven initiatives to address recurring issues—e.g., vendor scorecards, QC enhancements, and employee training.

4. **Predictive Analytics Integration**  
   Develop predictive models using historical defect and downtime trends to proactively flag at-risk vendors or materials.

---

### 🙏 Thank You!

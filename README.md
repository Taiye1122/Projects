# SUPPLIER QUALITY AND PERFROMANCE ANALYSIS

![Uploading Screenshot 2025-03-27 173537.png…]()

## Table of Contents
- [Project Overview](#project-overview)
- [Project Objective](#project-objective)
- [Data Source](#data-sources)
- [Tools](#tools)
- [Walk Through Process](#walk-through-process)
- [Results/Findings](#resultsfindings)

### Project Overview
Enterprise Manufacturers Ltd. faced significant challenges due to the lack of a centralized procurement system and inconsistent supplier quality across various plants. The company had recently consolidated data from multiple plants that included information on raw material defects, the vendors supplying these materials, and the downtime incurred when defective goods were used.

### Project Objective
1. Identify Key Issues:
Determine which vendors or plants are the primary sources of defects and downtime.
2. Analyze Combinations:
Examine specific combinations of material, vendor, and plant to identify poor performance clusters.
3. Support Decision-Making:
Develop interactive dashboards in Power BI to provide clear insights and support procurement and vendor management decisions.

### Data Sources
Supplier Data: The primary dataset used for this analysis is the [Supplier_data.xlsx](https://docs.google.com/spreadsheets/d/1nwO4VG5U2cklj5OoDpx6U1j0voGm-9zW/edit?usp=drive_link&ouid=106373350318822195700&rtpof=true&sd=true) file, containing detailed information information around the material, defect and vendor across all the plants.

### Tools
- Excel - Data Exploration
- Power Query - Data Cleaning
- Power BI - Data Visualization

### Walk Through Process
1. **Data Consolidation & Cleaning:**

- Gathering Data:
Collected raw data from all relevant plants, including material details, defect counts, and downtime minutes.

- Data Cleaning:
Standardized vendor names, resolved inconsistencies between plants, and cleaned the dataset for accuracy using Excel and Power Query.

2. **Data Modeling in Power BI:**

- Schema Design:
Created a relational data model that links materials, vendors, and plant locations.

- DAX Measures:
Here are some of the DAX measures used 
```DAX
Date = ADDCOLUMNS(
                 CALENDARAUTO(),
                 "Year", YEAR([Date]),
                 "Quarter", QUARTER([Date]),
                 "Month", MONTH([Date]),
                 "Month Name", FORMAT([Date], "mmmm"),
                 "Month Name Short", FORMAT([Date],"mmm"),
                 "Day", DAY([Date]),
                 "Day of the week", WEEKDAY([Date]),
                 "day of the week name", FORMAT([Date],"dddd"),
                 "Week of Year", WEEKNUM([Date]),
                 "Is Weekend", IF(WEEKDAY([Date]) IN {1, 7}, True,false))
``` 
``` DAX
Impact Defect Type = 
                 VAR Impact = CALCULATE(COUNT('Supplier fact'[Defect Type ID]), KEEPFILTERS('Defect Type'[Defect Type] = "Impact"))
                 var Defects = COUNT('Supplier fact'[Defect Type ID])
                 RETURN DIVIDE(Impact, Defects)
```
``` DAX
Rank Plant by Defect Qty = 
           var topplantdefectquantity = IF(ISINSCOPE(Plant[Plant Location]), (RANKX(ALL(Plant[Plant Location]), [Total Defect Qty], , desc)))
           var bottomplantdefectquantity = IF(ISINSCOPE(Plant[Plant Location]), (RANKX(ALL(Plant[Plant Location]), [Total Defect Qty], ,ASC)))
           var ranking = IF(SELECTEDVALUE(TopBottom[Value]) = "Top" , topplantdefectquantity, bottomplantdefectquantity)
           RETURN IF(ranking <= 'Top N Parameter'[Top N Parameter Value], [Total Defect Qty])
```
``` DAX
YOY Downtime Cost Trend colour = IF([YOYGrowthDowntimeCost] > 0, "Red", "Green")
```
``` DAX
YOY Downtime Cost trend Icon = 
var positiveicon = unichar(9650)
var negativeicon = UNICHAR(9660)
var choice = if([YOYGrowthDowntimeCost] > 0, positiveicon,negativeicon)
var display = choice & " " & FORMAT([YOYGrowthDowntimeCost], "0.00%")
RETURN display
```



3. **Interactive Dashboard Development:**

- Visualizations:
Designed key dashboards that include bar charts, heatmaps, and matrices which display:

- Top Offenders: Which vendors or plants are causing the most defects.

- Downtime Analysis: Breakdown of downtime across different suppliers and plants.

- Performance Comparisons: How the same vendor/material combination performs across various plants.

### Results/findings
In analyzing supplier quality and performance, key insights include:

- **Financial Impact of Defects:** Defective materials led to over $2 million in downtime costs, with significant spikes in September and December.

- **Underperforming Vendors:** Suppliers such as Avamm, Meejo, and Yombu were identified as high-risk, contributing the most to defects and downtime.

- **Problematic Plants:** Facilities like Hingham, Charles City, and Twin Rocks reported defect quantities nearing 100 million, indicating significant operational issues.

- **Recurring Defect Types:** Issues such as bad seams were prevalent, suggesting the need for focused quality control measures.

- **Material-Specific Problems:** Raw materials were the most problematic, causing substantial downtime and highlighting areas for supply chain improvements.

These insights underscore the necessity for targeted supplier management strategies and process enhancements to mitigate defects and associated costs.




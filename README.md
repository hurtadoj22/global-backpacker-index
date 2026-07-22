# 🎒 Global Backpacker Index

**Interactive Destination Benchmarking Dashboard & Data Pipeline**

<img width="610" height="365" alt="image" src="https://github.com/user-attachments/assets/71aadc56-6dcc-4e72-8a31-b42056a85285" /><img width="611" height="365" alt="image" src="https://github.com/user-attachments/assets/32af5f02-20ff-462d-9cee-afba09d5b5f9" />

---

## 📌 Project Overview

Choosing a travel or backpacking destination involves complex trade-offs between affordability and safety/quality of life. The **Global Backpacker Index** is an end-to-end data analytics project that cleans, transforms, and visualizes global socio-economic data across 83 countries based on data from 2024.

The goal was to build an interactive Power BI dashboard that enables budget travelers to make data-driven destination choices by comparing country metrics directly against fixed global benchmark averages.

---

## 🛠️ Tools & Technologies Used

* **Python (Pandas, NumPy):** Data ingestion, cleaning, handling missing values, filtering by recency, and master dataset feature engineering.
* **Power BI Desktop:** Dynamic data modeling, custom theme styling, user experience (UX/UI) design, and report layout.
* **DAX (Data Analysis Expressions):** Context modification (`CALCULATE`, `ALL`), conditional logic (`SWITCH`), and global baseline aggregation.

---

## 🔄 Data Pipeline & ETL (Python)

1. **Data Sourcing & Ingestion**
   * Sourced primary datasets from Kaggle covering global Cost of Living Index and Quality of Life Index metrics (Numbeo baseline data).
   * **Key metrics extracted include:** Safety Index, Groceries Index, Restaurant Price Index, Rent Index, Health Care Index, Climate Index, and Local Purchasing Power Index.

2. **Data Cleaning & Transformation Highlights**
   * **Entity Standardization:** Cleaned country names across disparate datasets to eliminate join mismatches.
   * **Missing Value Handling & Outlier Control:** Addressed missing entries in regional classifications and standardized numerical indices.
   * **Master Dataset Export:** Merged clean metrics into a single master file (`clean_backpacker_master.csv`) to serve as the unified star-schema model in Power BI.

---

## 📐 Data Modeling & DAX Engineering

To allow country-specific drillthroughs while maintaining global comparisons, DAX measures were written to calculate baseline benchmarks that ignore page-level country filters:

### Fixed Global Average Measures

```dax
Global Avg Rent = 
CALCULATE(
    AVERAGE('clean_backpacker_master'[Rent Index]), 
    ALL('clean_backpacker_master')
)
```

(Similar CALCULATE(..., ALL(...)) logic was implemented for Groceries, Restaurants, Health Care, Climate, Purchasing Power, and Safety indices to ensure dynamic paired comparisons on detailed visual breakdowns).

Custom Region Categorization

```dax
Region = 
SWITCH(
    TRUE(),
    'clean_backpacker_master'[Country] IN {"Thailand", "Vietnam", "Indonesia", "Japan", "India"}, "Asia",
    'clean_backpacker_master'[Country] IN {"Spain", "Portugal", "France", "Germany", "Italy"}, "Europe",
    'clean_backpacker_master'[Country] IN {"Mexico", "Costa Rica", "Colombia", "Brazil"}, "Latin America",
    'clean_backpacker_master'[Country] IN {"Australia", "New Zealand"}, "Oceania",
    "Other"
)
```

## 📊 Dashboard Architecture & UX Strategy

The Power BI report was designed using a strict information hierarchy spread across two dedicated pages:

### Page 1: Global Overview
* **Quadrant Scatter Plot (Safety vs. Cost Trade-off):** Plotted Cost of Living Index vs. Safety Index with reference lines at X = 45 (Avg Cost) and Y = 60 (Avg Safety) to segment countries into target budget/safety quadrants.
* **Dynamic World Map (Affordability vs. Safety):** Utilized bubble size for Safety Index and a custom color gradient for Cost of Living Index.
* **Top KPI Header Row:** Displays baseline counts, average cost, and safety scores with rounded containers, soft drop shadows (12px border radius), and crisp typography.

### Page 2: Country Breakdown (Drillthrough Experience)
* **Targeted Deep-Dive:** Activated right-click Drillthrough functionality bound to the Country field.
* **Paired Benchmark Bar Charts:** Placed country-specific metrics directly alongside global average companion bars (Forest Green vs. Muted Gray) so users instantly recognize if a country is priced above or below the world baseline.
* **Seamless Navigation:** Integrated an in-banner Back Button (`<--`) returning the user to Page 1.

---

## 🎯 Key Analytical Insights

* **The "Sweet Spot" Quadrant:** Countries in Southeast Asia (e.g., Thailand, Vietnam) consistently fall into Quadrant 2—offering significantly lower daily expenses (Cost Index < 35) while maintaining above-average safety rankings.
* **Expense Discrepancy Patterns:** In several European destinations, the Restaurant Price Index drops below the Groceries Index, indicating street dining and eating out can be more cost-effective for travelers than traditional grocery shopping.

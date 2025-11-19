# Optical Business Analytics Dashboard

## Snapshot of Dashboard 1 - Business Performance (Power BI)

<img width="1135" height="652" alt="dashboard 1" src="https://github.com/user-attachments/assets/24a186ed-fa84-4124-8fd5-3208cce73d59" />

## Snapshot of Dashboard 2 - Doctor & Category Analysis (Power BI)

<img width="1137" height="651" alt="dashboard 2" src="https://github.com/user-attachments/assets/eb386b6c-d48c-4d12-af6b-7fbff2c96a43" />

---

## Project Overview

This project showcases Power BI dashboards for an optical retail data model (dummy data). The dashboards provide a detailed look into key business metrics for optical retail, such as sales, conversion rates, profitability, product mix, operational metrics, and customer loyalty. All data used is synthetic and created for demonstration purposes only.

---

## Technology Used

- **Power BI Desktop** - For dashboard creation and data modeling
- **DAX (Data Analysis Expressions)** - For calculated measures and custom KPIs
- **Excel/CSV** - For data management
- **Power Query** - For data transformation and cleaning

---

## Approach

1. Defined the business metrics relevant for optical retail, including conversion analysis, revenue trends, profit/margin analysis, product mix, and customer loyalty.
2. Generated a mock dataset spanning 12 months, with fields representing transactions, doctors, stores, products, and operational data.
3. Designed two Power BI dashboards—one focused on overall business performance; the other on in-depth doctor, product, and location analysis.
4. Developed DAX measures for KPIs such as conversion rate, MoM growth, formatted revenue/cost/profit, repeat patient count, and operational performance.
5. Used interactive visualizations: KPI cards, line and bar charts, pie/segment charts, and data tables for detailed analysis.

---

## Key Insights & Project Motivation

This section covers what we discovered from the dashboards and the core purpose of this project:

**What We Found:**

- **Conversion Performance Varies by Doctor**: The dashboards reveal that conversion rates range from 41% to 57% across doctors. This variation highlights opportunities to identify best practices from high performers and implement improvements across the team.

- **Product Mix Drives Revenue**: Spectacles dominate with the highest revenue (₹1.158M), followed by contact lenses. This insight helps prioritize inventory and marketing efforts on high-revenue categories.

- **Profitability is Not Always About Volume**: Dr. Desai generates the highest profit (₹515K) despite not seeing the most patients, indicating that profit depends on product mix, pricing, and efficiency—not just patient count.

- **Customer Loyalty Exists but Can Be Optimized**: Repeat customer rate stands at ~45%, showing that loyalty programs and follow-up engagement can be strengthened.

- **Operational Efficiency is Solid**: Average turnaround time of 7 days and 80.9% order completion rate are reasonable benchmarks for optical retail operations.

- **Cash Payment Dominance**: 40.63% of transactions are cash-based, suggesting room to promote digital and insurance-based payments.

**Project Motivation:**

The primary goal of this project is to demonstrate how Power BI can be used to build business intelligence dashboards for retail operations. Specifically, it shows:

1. How to structure retail data for analysis (transactions, products, locations, performance metrics)
2. How to use DAX to calculate meaningful KPIs (conversion rates, profitability, trends, loyalty metrics)
3. How to design dashboards that serve different audiences—executive KPIs for high-level decisions, and detailed operational views for drill-down analysis
4. How to use visualizations to tell a data story and surface actionable insights

By working through this optical retail scenario, the project provides a reusable template for analyzing any retail business: tracking sales, profitability, conversion funnels, customer behavior, and operational performance.

---

## Dashboard 1: Business Performance Overview

**KPI Cards:**
- Count: 800 transactions
- Total Sales: ₹54.87L
- Cost: ₹28.97L
- Profit: ₹25.90L
- Avg. TAT: 7.0 Days
- Overall Conversion Rate: 53.9%
- Avg. Margin: 44.4%
- MoM Growth: -45.7%

**Key Visualizations:**
1. Month-wise Patient Trend Line - Shows patient visit patterns over 12 months
2. Patient Conversion Rate (100% Stacked Bar) - Doctor-wise conversion performance
3. Product Type Distribution (Pie Chart) - Branded vs Semi-Branded vs Budget mix
4. Conversion Status Split (Donut) - Overall conversion split
5. Transaction Detail Table - Drill-down view of individual transactions

**Interactive Filters:**
- Date range selector
- Month filter
- Order Status filter
- Product Category filter
- Product Type filter

---

## Dashboard 2: Doctor & Category Analysis

**Key Metrics:**
1. Doctor-Wise Profit - Shows profit contribution by each doctor
2. Product-Wise Profit - Breakdown of revenue by product category
3. Repeated Patient Count - Customer loyalty by doctor (Dr. Verma leads with 71)
4. Store-Wise Profit - Performance across store locations
5. Payment Mode Distribution - Customer payment preferences (Cash: 40.63%)
6. Product Type Mix - Distribution across product segments

**Interactive Filters:**
- Month selector
- Doctor name filter
- Order Status filter
- Product Type filter

---

## Main DAX Measures

**Conversion Rate Percentage by Doctor**
```dax
Converted_Percentage = 
VAR TotalByDoctor = 
    CALCULATE(
        COUNTROWS('DUMMY DATA'),
        ALLEXCEPT('DUMMY DATA', 'DUMMY DATA'[DoctorName])
    )
VAR ConvertedByDoctor = 
    CALCULATE(
        COUNTROWS('DUMMY DATA'),
        'DUMMY DATA'[ConversionStatus] = "Converted",
        ALLEXCEPT('DUMMY DATA', 'DUMMY DATA'[DoctorName])
    )
RETURN
    DIVIDE(ConvertedByDoctor, TotalByDoctor, 0) * 100
```

**Month-on-Month Revenue Growth**
```dax
MoM_Growth = 
VAR CurrentMonth = MONTH(TODAY())
VAR CurrentYear = YEAR(TODAY())
VAR PrevMonth = IF(CurrentMonth = 1, 12, CurrentMonth - 1)
VAR PrevYear = IF(CurrentMonth = 1, CurrentYear - 1, CurrentYear)
VAR CurrentMonthRev = 
    CALCULATE(
        SUM('DUMMY DATA'[NetRevenue]),
        MONTH('DUMMY DATA'[TransactionDate]) = CurrentMonth,
        YEAR('DUMMY DATA'[TransactionDate]) = CurrentYear
    )
VAR PreviousMonthRev = 
    CALCULATE(
        SUM('DUMMY DATA'[NetRevenue]),
        MONTH('DUMMY DATA'[TransactionDate]) = PrevMonth,
        YEAR('DUMMY DATA'[TransactionDate]) = PrevYear
    )
VAR Growth = DIVIDE(CurrentMonthRev - PreviousMonthRev, PreviousMonthRev, 0)
RETURN
    FORMAT(Growth, "0.0%")
```

**Formatted Revenue (with Cr/L/K scaling)**
```dax
Total_Revenue_Formatted = 
VAR Revenue = SUM('DUMMY DATA'[NetRevenue])
VAR Result = 
    SWITCH(
        TRUE(),
        Revenue >= 10000000, FORMAT(Revenue / 10000000, "₹0.00") & "Cr",
        Revenue >= 100000, FORMAT(Revenue / 100000, "₹0.00") & "L",
        Revenue >= 1000, FORMAT(Revenue / 1000, "₹0.0") & "K",
        FORMAT(Revenue, "₹#,##0")
    )
RETURN Result
```

**Repeat Patient Count**
```dax
Repeat_Patient_Count = 
CALCULATE(
    COUNTROWS('DUMMY DATA'),
    'DUMMY DATA'[RepeatCustomer] = "Yes"
)
```

**Order Completion Rate**
```dax
Order_Completion_Rate = 
VAR TotalOrders = COUNTROWS('DUMMY DATA')
VAR CompletedOrders = 
    CALCULATE(
        COUNTROWS('DUMMY DATA'),
        'DUMMY DATA'[OrderStatus] = "Completed"
    )
RETURN
    FORMAT(DIVIDE(CompletedOrders, TotalOrders, 0), "0.0%")
```

---

## Dataset Schema

| Column Name | Data Type | Description |
|---|---|---|
| TransactionID | String | Unique transaction identifier |
| PatientID | String | Unique patient identifier |
| TransactionDate | Date | Date of transaction |
| DoctorName | String | Consulting doctor name |
| RefractionDone | Boolean | Whether refraction test was performed |
| PrescriptionIssued | Boolean | Whether prescription was issued |
| PurchaseCompleted | Boolean | Whether purchase was completed |
| StoreName | String | Store location |
| ProductCategory | String | Product category (Spectacles, Contact Lens, etc.) |
| ProductType | String | Product type (Branded, Semi-Branded, Budget) |
| SellingPrice | Currency | Selling price in INR |
| CostPrice | Currency | Cost price in INR |
| GrossMargin_Percentage | Number | Gross margin percentage |
| InventoryStatus | String | Stock status |
| StockAgeing_Days | Number | Days in inventory |
| OrderDate | Date | Order placement date |
| DeliveryDate | Date | Delivery completion date |
| TAT_Days | Number | Turnaround time in days |
| OrderStatus | String | Order status |
| PaymentMode | String | Payment method |
| InsuranceCovered | Boolean | Insurance coverage status |
| PatientFeedback | String | Customer feedback |
| RatingOutOf5 | Number | Rating (1-5) |
| FollowUpScheduled | Boolean | Follow-up appointment status |
| RepeatCustomer | Boolean | Repeat customer indicator |
| NetRevenue | Currency | Net revenue |
| GrossProfit | Currency | Gross profit |
| ConversionStatus | String | Conversion status |

**Dataset Stats:**
- 800 records
- 28 columns
- 12 months of data
- 6 doctors, 5 store locations, 5 product categories

---

## How to Use

1. Download or clone the repository
2. Open the `.pbix` file in Power BI Desktop
3. Explore both dashboard tabs
4. Use filters to drill into specific time periods, doctors, products, or locations
5. Hover over visuals to see tooltips and detailed values

---

## Repository Structure

```
optical-analytics-dashboard/
│
├── README.md
├── data/
│   └── optical_data.csv
├── dashboards/
│   └── Optical_Analytics.pbix
└── screenshots/
    ├── dashboard-1.png
    └── dashboard-2.png
```

---

## Skills & Techniques Demonstrated

- Power BI dashboard design and layout
- DAX measure creation for KPIs and calculations
- Data modeling and relationships
- Time intelligence functions (MONTH, YEAR, TODAY)
- Conditional formatting and dynamic scaling
- Interactive filtering and drill-down capabilities
- Data visualization best practices
- Business metrics and analysis techniques

---

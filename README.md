# E-Commerce Analytics Pipeline: Customer Segmentation, Cohort Retention & Revenue Forecasting

A comprehensive end-to-end Excel analytical project built on the **UCI Online Retail Dataset**. This workbook processes raw transactional data (~541k records) to deliver customer segmentation (RFM), customer retention matrices, time-series exponential smoothing revenue forecasts, and an executive dashboard.

---

## Executive Summary

- **Total Clean Revenue**: $8,761,066.65 ($8,247,836.22 across the 12-month baseline Dec 2010 – Nov 2011)
- **Active Customer Base**: 4,335 unique buyers
- **Repeat Customer Rate**: 65.26%
- **3-Month Projected Sales**: $3,204,879.69 (Dec 2011 – Feb 2012 ETS Forecast)

---

## Project Structure & Analytical Pipeline

### Phase 1: Data Cleaning & ETL (`Transactions_Clean`)
- **Source**: UCI Machine Learning Repository — Online Retail Dataset.
- **Transformation Pipeline** (Power Query):
  - Stripped non-customer records (`CustomerID` is null).
  - Filtered out product returns and cancellations (`Quantity <= 0`).
  - Generated computed column `Revenue = Quantity * UnitPrice`.
  - Set baseline snapshot evaluation date to `2011-12-10`.

### Phase 2: RFM Customer Segmentation (`RFM_Analysis`)
- Computed **Recency** (days since last purchase), **Frequency** (distinct invoice count), and **Monetary** (total spend) per customer.
- Assigned quintile scores (1–5) via explicit interval logic.
- Rule-mapped scores into **7 actionable customer segments**:
  1. **Champions**: High spenders, frequent, recent buyers.
  2. **Loyal Customers**: Consistent buyers with steady order frequency.
  3. **Potential Loyalists**: Recent buyers with moderate frequency.
  4. **Promising / Need Attention**: Recent buyers with average spend requiring engagement.
  5. **New Customers**: First-time buyers within the recent snapshot window.
  6. **At Risk**: High historical spenders who haven't purchased recently.
  7. **Hibernating / Lost**: Low frequency and long time elapsed since last order.

### Phase 3: Cohort Retention Heatmap (`Cohort_Retention`)
- Aggregated distinct monthly purchasing cohorts from join date (`2010-12` through `2011-12`).
- Constructed a percentage decay matrix utilizing blanking logic:
  `=IF(OR(ISBLANK(B5), B5="", B5=0), "", B5 / $B5)`
- Derived the **Average Retention Curve** to isolate initial onboarding drop-off vs. long-term recurring stabilization.

### Phase 4: Time-Series Revenue Forecasting (`Forecast`)
- Built a 12-month clean historical monthly baseline (Dec 2010 – Nov 2011), excluding partial Dec 2011 data.
- Configured date serials (`YYYY-MM-DD`) to satisfy `FORECAST.ETS` timeline requirements.
- Implemented models:
  - **ETS Exponential Smoothing**: `=FORECAST.ETS(target_date, values, timeline, 1, 1)`
  - **95% Confidence Interval**: `=FORECAST.ETS.CONFINT(target_date, values, timeline, 0.95, 1, 1)`
  - **Linear Trend Baseline**: `=FORECAST.LINEAR(target_date, values, timeline)`

### Phase 5: Executive Dashboard (`Executive_Dashboard`)
- High-level KPI summary cards for Revenue, Unique Customers, Repeat Rate, and Forecasted Sales.
- Combo time-series chart showing historical sales alongside ETS, Linear, and Confidence Bounds.
- Donut & Bar charts highlighting the RFM Pareto distribution (Champions generating 62% of revenue).

---

## Core Findings & Strategic Recommendations

1. **Protect the VIP Base**:
   - *Champions* (18.96% of buyers) contribute **62% of total revenue**. Retaining this group is the single highest-leverage operational priority.
2. **Mitigate Month 1 Onboarding Churn**:
   - Retention drops from 100% to ~20% in Month 1. Implementing post-purchase email sequences and 21-day re-engagement incentives targets an increase in initial buyer retention.
3. **Capitalize on Q4 Seasonality**:
   - Revenue accelerates dramatically in Q4 ($1.14M in Nov '11). Inventory depth must be maintained through Q1 to satisfy projected $3.20M+ quarterly demand.

---

Access Link : https://docs.google.com/spreadsheets/d/15hyUEMJlnX4Sl062-5AKzhPtd_fK0F4R/edit?usp=sharing&ouid=101924346634065868107&rtpof=true&sd=true

## Technical Requirements & Excel Functions

- **Excel Version**: Microsoft 365 or Excel 2016+ (requires `FORECAST.ETS` support).
- **Core Functions Used**: `FORECAST.ETS`, `FORECAST.ETS.CONFINT`, `FORECAST.LINEAR`, `AVERAGE`, `COUNTIFS`, `SUMIFS`, `DISTINCTCOUNT`, `IF`, `OR`, `ISBLANK`, `MAX`.
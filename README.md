# BrightHaus Electronics: Sales, Profitability and Returns Analytics

**An Excel project built from raw, multi-table retail data: data quality investigation, cleaning with Power Query, data modelling, DAX measures and a five-page dashboard.**

![Excel](https://img.shields.io/badge/Excel-217346?style=flat&logo=microsoftexcel&logoColor=white)
![Power Query](https://img.shields.io/badge/Power_Query-Data_Cleaning-217346?style=flat)
![DAX](https://img.shields.io/badge/DAX-Measures-0F6BB8?style=flat)
![Data Quality](https://img.shields.io/badge/Issue_Log-49_issues_closed-12202E?style=flat)

I took seven raw tables from five source systems (about 33,700 order lines) and turned them into a clean, validated data model and an interactive dashboard for a fictional Nigerian electronics retailer. Along the way I investigated 49 data problems. For each one I worked out the cause, applied a fix and checked the result against the data. Where the data could not recover a value, I labelled the gap instead of inventing one.

![Dashboard overview](images/brighthaus-overview.png)

---

## At a glance

| | |
|---|---|
| **Business** | BrightHaus Electronics Nigeria Ltd (fictional), January 2022 to December 2025 |
| **Tools** | Excel, Power Query, Excel Data Model (Power Pivot), DAX, PivotTables, PivotCharts, Slicers |
| **Raw data** | 33,708 order lines (31,974 in Orders plus 1,734 from a separate marketplace feed), 5,945 customer records, 198 products, 3,709 return and refund records, 200 employee records, 31 location records |
| **Data model** | 6 tables, 6 relationships |
| **Issue Log** | 49 issues, all closed |
| **Currency** | Nigerian Naira (₦) |

### Headline results (all years)

| Measure | Value |
|---|---:|
| Net Sales (before refunds) | ₦10,544,599,508 |
| Net Sales after refunds | ₦10.05bn |
| Total Profit (after refunds) | ₦1.33bn |
| Profit Margin (after refunds) | 13.23% |
| Gross Margin (before refunds) | 17.31% |
| Valid Orders | 19,032 |
| Average Order Value | ₦554,046 |
| Return Rate | 7.91% |
| Active Customers | 4,943 of 5,900 |

**Main metric:** **Net Sales** = Gross Sales − Total Discounts, counted on valid sales only (`Valid_Sale = Yes`). Gross Sales is quantity × unit price. A line is a valid sale only if its quantity is 1 or more and its status is not Cancelled or Payment Failed. Net Sales is measured before refunds. **Net Sales after refunds** is the same figure after refunds are taken off.

---

## Business context and problem

**The company.** BrightHaus Electronics Nigeria Ltd is a fictional omnichannel retailer of consumer electronics, home appliances and power solutions, founded in Lagos in 2016. At the end of 2025 it operates 26 retail locations (flagships, standard stores and kiosks) across 18 Nigerian cities, plus three fulfilment hubs that serve online orders. It sells roughly 200 active and legacy products in eight categories. Customers are individual shoppers (Retail), small businesses (SME), companies (Corporate) and public-sector or non-profit buyers (Government & NGO). Orders arrive through six channels: In-Store, Website, Mobile App, WhatsApp, a Corporate Sales desk and a third-party Marketplace partner, whose orders arrive as a separate export file.

**The problem.** After four turbulent years (currency devaluation, rapid inflation, a shift to online selling, store openings and closures, and heavy promotional activity), the Chief Commercial Officer and CFO want a clear, trustworthy picture of how the business performed. Nobody had reconciled the five source systems (POS, e-commerce platform, CRM, HR system and the marketplace export), and the data was exported straight from them. Management has concerns including margins, returns and regional performance.

### Objectives

| Objective | Status |
|---|---|
| Profile and document the data quality problems in each source table | Complete (Issue Log) |
| Clean and transform the data with repeatable Power Query steps, and validate relationships between tables | Complete |
| Build an Excel Data Model and calculate core KPIs: Net Sales, profit, margin, average order value, return rate and cancellation rate | Complete |
| Analyse products, customers, channels, regions and stores, employees and returns, and trends over time | Complete on the dashboard |
| Deliver an executive dashboard, data-driven recommendations and an honest list of limitations | Dashboard built and limitations documented; recommendations planned |

---

## Key findings

### What the business data shows

Each finding comes from the dashboard. The Validation section shows the cross-checks behind the figures.

1. **Net Sales after refunds grew 2.25 times**, from ₦1.76bn in 2022 to ₦3.96bn in 2025. Year-on-year growth was 7.87% (2023), 28.14% (2024) and 62.90% (2025).
2. **Higher order value drove most of the growth.** Average order value (calculated on Net Sales before refunds) rose 77%, from ₦400.7K to ₦708.5K, while valid orders rose 27%, from 4,590 to 5,828.
3. **Profit margin was uneven:** 14.8% (2022), 9.4% (2023), 11.7% (2024) and 15.3% (2025). The lowest quarterly margin was 4.8% in Q4 2023. Q4 has the highest Net Sales after refunds in every year.
4. **Laptops & Computing is the largest category** (26.6% of Net Sales, ₦2.80bn) **but has the lowest gross margin** (11.4%). The South West region contributes 43% of Net Sales.
5. **The highest-margin categories are small.** Accessories & Wearables (37.8% margin, ₦489.8m) and Kitchen Appliances (30.8%, ₦533.0m) are each under 6% of Net Sales. Accessories & Wearables also has the highest return rate (13.7%).
6. **Online channels more than doubled their share.** Sales with no sales rep (Website, Mobile App and Marketplace) grew from 16.1% of Net Sales in 2022 to 34.9% in 2025. Across all years they bring ₦3.08bn (29.2%), against ₦7.47bn (70.8%) for assisted channels.
7. **Promotions account for 11.5% of Net Sales, and every campaign has a lower gross margin than the no-promotion figure of 17.98%.** Black Friday is the lowest at 6.2%.
8. **A small segment carries a large share.** Corporate customers are 3% of active customers but 21.1% of Net Sales (₦14.8M per customer, against ₦1.2M for Retail). The top 10 customers account for only 4.7% of Net Sales, so no single customer dominates.
9. **Standard stores bring the most Net Sales (53.3%),** ahead of Flagships (21.1%), Fulfilment Hubs (20.1%) and Kiosks (5.5%). The top 3 stores bring 30.7%. 29 of the 31 location records have sales.

### What the data investigation found

1. **281 order lines were double entries** (257 in Orders, 24 in the marketplace feed). Left in, they would have double-counted Net Sales. My own check on the raw data found exactly 281, matching the supplied Data Quality Report.
2. **45 customers were registered twice.** I merged them (43 matched on cleaned phone numbers, 2 manually), taking the customer table from 5,945 to 5,900.
3. **957 of 5,900 customers (16.2%) have no valid sale:** 862 never ordered and 95 have no valid sale.
4. **About 0.65% of Net Sales (₦68.1M) has no payment method.** I kept those rows and labelled them "Payment Method Not Recorded".
5. **A discount pattern for management review.** 13 heavily discounted, price-matched in-store order lines (29 to 42% discount, no promo code) were credited to one employee between 6 April and 16 June 2024. Sold below cost, they lost ₦576,294.
6. **A staff discount code used on customer orders.** `STAFF50` appears on 28 retail orders, all at 50% off and all in-store.

Findings 5 and 6 are possible control issues. I kept the rows and reported them instead of correcting them.

---

## Data quality: what I found, how I investigated and how I verified

Every issue followed the same path: **found** (what the problem is), **investigated** (why it happened and what a fix would change), **corrected** (a documented rule applied in Power Query), and **verified** (the refreshed data checked against the source). The table shows the most instructive cases. The full record is in the Issue Log.

| Problem | How I investigated | Resolution | How I verified |
|---|---|---|---|
| **Two prices for the same product** (PRD-0001, PRD-0033) | Compared both prices with each product's recent sales in NGN, newest first. The repeat came from an older Products table. | Pointed the Products query at the current price list, which has one row per product. | 198 unique IDs. PRD-0001 appears once, at ₦423,500; its 110 order lines run from about ₦217,000 (2022) to ₦420,000 (2025). PRD-0033 appears once, at ₦892,500; none of its 125 lines is near the old ₦324,000. |
| **Duplicate sales** | The same sale appeared under different line IDs and sometimes a different Order_ID | Removed duplicates on seven fields and left Order_ID out of the key on purpose | The same seven-field check on raw data finds 281 repeats, matching the raw report. The risk (two real identical orders on one day would also merge) is noted in the log. |
| **Duplicate customers** | Joined the customer table to itself on cleaned phone numbers | Mapped each duplicate ID to a master ID, then re-pointed Orders and Returns | 43 phone numbers are each shared by exactly two customers, so the join adds no extra rows. The 2 manual pairs share a name or email and city. 5,945 − 45 = 5,900. |
| **Duplicate employee** (EMP-088 and EMP-200) | Compared email, store, hire date, role, department, manager, salary and commission rate. Only the name's capitalisation differed. | Removed EMP-200 | EMP-200 had 0 order lines and 0 return records, so no sales needed re-pointing. Refreshed table: 200 rows (199 employees plus the online row), EMP-088 once. |
| **Dates stored as text in mixed layouts, placeholder dates and year typos** | Counted the affected lines (210 order dates, 174 ship dates, 165 delivery dates and 66 return dates) and tested day-first against month-first reading on the returns data | Converted with the day-first (en-GB) locale. In Orders, set the 3 placeholder dates (1900-01-01) to blank and corrected the 3 year typos (2052) by subtracting 27 years. | Read month-first, 9 return resolution times would be negative. Read day-first, resolution times run from 1 to 39 days and none is negative. |
| **Blank order dates** | Found 18 lines with no Order_Date (17 in Orders, 1 in the feed). A blank date shows as a (blank) row under Year in PivotTables. | Recovered dates from other lines of the same order. For 2 single-line orders (ORD-0012733, ORD-0017667), used the ship date and flagged them `Order_Date_Imputed`. | Net Sales on those 2 lines (₦50,033) is the amount that sat in the blank Year row. After refresh the blank row is gone, and the flag returns exactly 2 lines. |
| **"Delivery delay" and "Late delivery" return reasons** | Checked whether they were the same cause | Kept both, and added `Return_Reason_Group` to combine them for charts | All 142 "Delivery delay" lines are cancellation refunds and all 124 "Late delivery" lines are return refunds, so they are different processes. Group total 266 = 142 + 124, and group counts add up to 3,709. |
| **Returns linked to orders** | Checked every return against Orders | Added link-check fields | Every Order_Line_ID in Returns exists in Orders, none points to a removed duplicate line, and Order_ID, Product_ID and Customer_ID agree. |
| **Discounts typed as 10 instead of 0.10** (63 lines) | A value of 10 would read as a 1,000% discount and create large negative Net Sales | Divided by 100 where the value is above 1 | Discount_Pct is now a fraction between 0 and 1 on all lines. |
| **Online orders with no employee** | The raw report confirms a blank Employee_ID is legitimate on online channels | Labelled "Online Channels (No Sales Rep)" and added a matching row to Employees so the relationship still works. Rejected "Unassigned" because it suggests bad data. | Online sales show under that label, not as "(blank)". |

### How the whole log was checked

- **Coverage:** every line of the supplied Data Quality Report (98 retained issues and 2 suspicious patterns) is mapped to the log entry that covers it, in the `Raw Issue Coverage` sheet.
- **Counts:** where possible, I recomputed the problem on raw data with the same logic as the fix and compared it with the report.
- **After refresh:** I profiled the cleaned table after each fix (row counts, distinct IDs, allowed values).
- **Reconciliation:** the `Validation Checks` sheet reconciles customers, duplicates, returns and the Net Sales measures.

---

## Cleaning approach

All cleaning is done in Power Query, so each step is visible and repeatable on refresh.

| Principle | Example |
|---|---|
| **Keep rows and flag problems** | `DQ_Flag` marks zero or negative quantities and missing dates. `Valid_Sale` decides what counts as a sale. |
| **Label gaps clearly** | Labels name the field that is missing: "Payment Method Not Recorded", "Sales Channel Not Recorded", "Gender Not Recorded". Plain "Unknown" (too vague) and "Not Recorded" (does not say what is missing) were rejected. |
| **Never invent values** | Blank emails and phone numbers stay blank. Blank sales channels are recovered from other lines of the same order, and the 4 lines that cannot be recovered read "Sales Channel Not Recorded". |
| **Standardise with mapping lists** | 6 sales channels, 11 payment methods, 7 order statuses, 14 return reasons, 6 refund methods, 4 customer segments, 8 product categories |
| **Fix once, apply everywhere** | The marketplace feed is appended to Orders so each cleaning step runs once |
| **Document every decision** | Each issue records what was found, why it matters, what was done and the result |

---

## Data model

Six tables are loaded into the Excel Data Model: **Products, Orders, ReturnsPayments, Customers, Employees and Stores**, connected by **six relationships**. The model also holds two added rows: "No Sales Rep" in Employees, so online orders still match, and an "Unlisted product" placeholder in Products.

![Data model](images/brighthaus-data-model.png)

### Core measures (DAX)

```dax
Gross Sales =
CALCULATE(SUM(Orders[Gross_Revenue]), Orders[Valid_Sale] = "Yes")

Total Discounts =
CALCULATE(SUM(Orders[Discount_Amount]), Orders[Valid_Sale] = "Yes")

Net Sales = [Gross Sales] - [Total Discounts]
```

`Gross_Revenue` is the order-line column for quantity × unit price. It feeds Gross Sales and is not a reported metric. The KPI definitions for each dashboard page are in the Dashboard section.

### Validation

Each key figure was cross-checked against another figure from the model, the log or the dashboard.

| Check | Calculation | Result |
|---|---|---|
| Assisted plus online Net Sales | ₦7,467,317,447 + ₦3,077,282,061 | ₦10,544,599,508 = Net Sales |
| Net Sales in the Payment Method pivot | Grand total | ₦10,544,599,508 = Net Sales |
| Order lines after cleaning | 33,708 − 281 | 33,427 = the order lines by year on the Sales page (8,060 + 7,784 + 7,367 + 10,216) |
| Valid orders | 4,590 + 4,411 + 4,203 + 5,828 | 19,032 = the Overview card |
| Customers after removing duplicates | 5,945 − 45 | 5,900 |
| Active customers plus customers with no valid sale | 4,943 + 957 | 5,900 |
| Customers with no valid sale | 862 never ordered + 95 with no valid sale | 957 |
| Duplicate order lines | 257 (Orders) + 24 (feed) | 281, matching the raw report |
| Employees table | 199 employees plus the online row | 200 rows |
| Average order value | ₦10,544,599,508 ÷ 19,032 | ₦554,046 |
| Net Sales per day | ₦10,544,599,508 ÷ 1,461 days (1 Jan 2022 to 31 Dec 2025) | ₦7,217,385 |
| Net Sales per customer | ₦10,544,599,508 ÷ 4,943 | ₦2,133,239 |
| Orders per customer | 19,032 ÷ 4,943 | 3.85 |
| Online Channels share | ₦3,077,282,061 ÷ ₦10,544,599,508 | 29.18% |
| Average selling price | ₦10,544,599,508 ÷ 43,411 units | ₦242,902 |
| Gross margin | (₦242,902 − ₦200,844) ÷ ₦242,902 | 17.31% |
| Profit margin | ₦1.33bn ÷ ₦10.05bn | 13.23% |
| Growth chain | ₦1.76bn × 1.0787 × 1.2814 × 1.6290 | ₦3.96bn, the 2025 figure |
| Commission cost | ₦74.4M ÷ ₦7.47bn | about 1.0% of assisted Net Sales |
| Top 10 customers | The ten values add to ₦497.5M | 4.7% of Net Sales |
| Top 10 products | The ten values add to ₦3.48bn | 33% of Net Sales |
| Store type shares | 53.3% + 21.1% + 20.1% + 5.5% | 100.0% |

---

## Dashboard

An interactive Excel dashboard built from PivotTables and PivotCharts, with KPI cards, slicers and a Key Insight panel on every page. A small VBA macro changes the colour of the active page's navigation button as you switch pages. The colour still works after the file is saved back to a normal Excel workbook.

### 1. Overview

![Sales Overview](images/brighthaus-overview.png)

*How much did sales grow from 2022 to 2025, was growth driven by more orders or higher value per order, and did margin hold?*

**Slicers:** Year, Region, Category

| KPI | Value | Definition on the card |
|---|---:|---|
| Net Sales after refunds | ₦10.05bn | after discount and refunds |
| Total Profit | ₦1.33bn | Net Sales after refunds minus cost of goods |
| Profit Margin % | 13.23% | profit ÷ Net Sales after refunds |
| Valid Orders | 19,032 | distinct qualifying orders |
| AOV | ₦554,046 | Net Sales (before refunds) ÷ orders |
| Return Rate | 7.91% | lines with a completed refund |

**Charts:** Net Sales after refunds and profit margin by quarter; Net Sales by region; valid orders and AOV by year; year-on-year growth; Net Sales by category.

### 2. Sales

![Sales Performance](images/brighthaus-sales.png)

*How do sales move by year, payment method, promotion and channel, and how fast do we deliver?*

**Slicers:** Year, Sales Region, Sales Channel

| KPI | Value | Definition on the card |
|---|---:|---|
| Net Sales Per Day | ₦7,217,385 | Net Sales ÷ days in period |
| Digital Channel Share % | 39.72% | share of Net Sales |
| Promo Sales Share % | 11.48% | share of Net Sales |
| Cancellation Rate | 3.87% | cancelled share of lines |
| Avg Delivery Days | 2.61 | days, all channels |

**Charts:** Net Sales and order lines by year (₦1.8bn, ₦2.0bn, ₦2.6bn and ₦4.1bn); Net Sales by payment method; average daily Net Sales on weekdays (₦7.8m) and weekends (₦5.7m); promotion campaigns by gross margin and average discount; average delivery days by channel.

**Observed:**
- Bank Transfer is the largest payment method (₦4.2bn, 39.6% of Net Sales), ahead of POS (₦1.6bn, 15.4%) and Card (₦1.4bn, 13.5%).
- Promotions account for 11.5% of Net Sales. Black Friday has the lowest gross margin (6.19%) and the highest average discount (16.64%). With no promotion, the margin is 17.98% and the average discount is 2.29%.
- Corporate Sales has the longest average delivery time (6.30 days). In-Store averages 0.24 days.

### 3. Products

![Products](images/brighthaus-products.png)

*Which products, brands and categories drive sales and margin, and where do returns concentrate?*

**Slicers:** Year, Sales Channel, Category

| KPI | Value | Definition on the card |
|---|---:|---|
| Units Sold | 43,411 | all valid lines |
| Avg Selling Price | ₦242,902 | per unit |
| Avg Unit Cost | ₦200,844 | per unit |
| Gross Margin % | 17.31% | before refunds |
| Average Discount % | 3.16% | on list price |
| Top Category Share % | 26.56% | Laptops & Computing |

**Charts:** top 10 products by Net Sales; gross margin by category; return rate by category; Net Sales and gross margin by brand; average selling price against average unit cost by quarter; return reasons.

**Observed:**
- Gross margin by category runs from 11.43% (Laptops & Computing) to 37.80% (Accessories & Wearables).
- Return rate by category runs from 3.58% (Power & Solar) to 13.67% (Accessories & Wearables).
- Among the return reasons charted, Defective / Not working is the most common (1,058 refunds), followed by Changed mind (576) and Damaged in transit (285).
- The top 10 products contribute 33% of Net Sales. The largest is a washing machine at ₦653.7m.
- Average selling price and average unit cost both rise across the four years.

### 4. Customers

![Customers](images/brighthaus-customers.png)

*Which customer segments, tiers and channels generate sales, and how loyal are customers?*

**Slicers:** Year, Segment, Customer Region

| KPI | Value | Definition on the card |
|---|---:|---|
| Active Customers | 4,943 | customers with a purchase |
| Net Sales Per Customer | ₦2,133,239 | Net Sales ÷ active customers |
| Orders Per Customer | 3.85 | valid orders ÷ customers |
| Repeat Customer Rate % | 57.70% | customers with 2+ orders |
| Customers With No Valid Sale | 957 | never ordered or no valid sale |

**Charts:** Net Sales by segment; Net Sales per customer by segment; Net Sales by loyalty tier; top 10 customers; Net Sales by customer region; Net Sales by acquisition channel.

**Observed:**
- Retail is 85% of active customers but 46.5% of Net Sales. Corporate is 3% of customers and 21.1% of Net Sales.
- Net Sales per customer: Corporate ₦14.8m, Government & NGO ₦8.2m, SME ₦5.3m, Retail ₦1.2m.
- Repeat rate by segment: Corporate 92%, Government & NGO 82%, SME 76%.
- Corporate Sales (30.6%) and Walk-In (24.8%) are the largest acquisition channels. South West customers contribute 39.6% of Net Sales.

### 5. Employees and Stores

![Employees and Stores](images/brighthaus-employees-stores.png)

*Who and where do sales come from, and how productive are our sellers and stores?*

**Slicers:** Year, Store Region, Store Type

| KPI | Value | Definition on the card |
|---|---:|---|
| Assisted Net Sales | ₦7.47bn | sales with a sales rep |
| Selling Employees | 180 | employees with sales |
| Assisted Net Sales per Selling Employee | ₦41.5M | assisted sales ÷ employees |
| Commission Cost (Est.) | ₦74.4M | estimate, not actual payout |
| Active Stores | 29 | stores with sales |
| Online Channels Share | 29.18% | sales with no sales rep |

**Charts:** top 10 sellers by Net Sales; Net Sales by role; assisted against online sales by year; top 10 stores; Net Sales by store type.

**Observed:**
- Assisted and online Net Sales by year: 2022 ₦1.5bn and ₦296.0m; 2023 ₦1.6bn and ₦435.7m; 2024 ₦1.7bn and ₦903.2m; 2025 ₦2.7bn and ₦1.4bn.
- The top 10 employees generated 48.8% of assisted sales.
- Estimated commission cost is about 1.0% of assisted Net Sales.
- Net Sales by role: Corporate Account Manager ₦2.7bn, Sales Associate ₦2.3bn, Customer Support Agent ₦1.1bn, Senior Sales Rep ₦1.1bn, Store Manager ₦244.5m.

---

## Business questions

| Theme | Question | Where on the dashboard |
|---|---|---|
| **Performance** | What are Net Sales, profit and profit margin, and how have they changed over time? | Overview (by quarter and year; month-on-month is not shown) |
| **Products** | Which products and categories earn the most Net Sales and profit? Which underperform? | Products, Overview |
| **Returns** | Which products have the highest return rates, and why are items returned? | Products (return rate and reasons), Overview (return rate). Refund cost is not shown as a separate figure. |
| **Customers** | Which segments and customers contribute the most Net Sales? | Customers |
| **Regions and stores** | Which regions, cities and stores perform best? | Overview (region), Employees and Stores (stores), Customers (customer region). Store trends over time are not shown. |
| **Channels and payments** | Which channels and payment methods perform best, and how has the mix shifted? | Sales, Employees and Stores (assisted against online by year) |
| **Orders** | What is the average order value, and what share of orders is cancelled? | Overview (AOV), Sales (cancellation rate) |
| **Promotions** | How effective were promotions: volume uplift versus margin erosion? | Sales (campaign margin and discount) |
| **Employees** | Who delivers the most Net Sales, and is performance consistent with discounting behaviour? | Employees and Stores. Discounting behaviour is covered by the two suspicious patterns in the Issue Log. |
| **Delivery** | How reliable are shipping and delivery times? | Sales (delivery days) |
| **Economy** | How did currency devaluation and inflation affect prices, costs and margins? | Products (selling price against unit cost by quarter), Overview (margin by quarter). No explicit inflation or exchange-rate analysis. |
| **Data reliability** | Are there suspicious patterns that threaten reported numbers? | Issue Log |
| **Action** | What should management do based on the findings? | Planned findings document |

---

## Assumptions and limitations

Each assumption below is recorded in the Issue Log with its reasoning.

- **The data is fictional.** Results describe a made-up business and do not generalise.
- **Net Sales is before refunds.** Lines with status Returned count as valid sales. Net Sales after refunds is shown separately on the Overview page.
- **Two margins are reported.** Gross margin (17.31%) is before refunds. Profit margin (13.23%) is after refunds.
- **Commission cost is an estimate,** not an actual payout.
- **Dates are read day-first (en-GB).** This is supported by the return-resolution check above.
- **Two order dates are estimates.** Their ship date stands in for the missing order date.
- **Discounts above 1 are divided by 100.** This assumes no real discount exceeds 100%.
- **Negative shipping fees and negative refunds** are treated as typing errors or a recording style, not reversals.
- **Duplicate detection** uses seven fields. Two genuine identical orders on one day would also be merged.
- **Two products** have a discontinued date before their launch date. Both are kept and flagged. 71 sales lines on these products fall outside their launch-to-discontinued dates.
- **Product prices and costs** in the product table are the latest price-list values, not historical ones. Historical values come from the order lines.
- **Invalid rows are flagged, not deleted,** and are excluded from valid sales.
- **The staff discount and heavy discount patterns** are reported as findings. The data cannot say whether they were authorised.

---

## Repository structure

Add files gradually. The status column shows what exists now.

```text
brighthaus-electronics-analysis/
├── README.md
├── data/
│   ├── raw/
│   └── cleaned/
├── documentation/
│   ├── BrightHaus_Issue_Log.xlsx
│   ├── data_cleaning_methodology.md
│   ├── business_rules_and_assumptions.md
│   └── data_model.md
├── power_query/
├── dashboard/
│   └── BrightHaus_Electronics_Pivot_Chart.xlsx
├── images/
└── findings/
    └── insights_and_recommendations.md
```

| Path | Purpose | Status |
|---|---|---|
| `README.md` | Central project documentation | This file |
| `data/raw/` | Original dataset, unchanged | To add |
| `data/cleaned/` | Optional exports of the cleaned tables | To add |
| `documentation/BrightHaus_Issue_Log.xlsx` | The completed Issue Log | Complete, to upload |
| `documentation/data_cleaning_methodology.md` | Cleaning approach, table by table | To add |
| `documentation/business_rules_and_assumptions.md` | Definitions such as Net Sales and Valid_Sale | To add |
| `documentation/data_model.md` | Tables, relationships and measures, with diagram | To add |
| `power_query/` | M code for each cleaned table | To add (optional) |
| `dashboard/` | The Excel dashboard workbook | To add |
| `images/` | Dashboard and data model screenshots | To add |
| `findings/` | Insights and recommendations | To add |

---

## Data quality documentation

The Issue Log is the full record of the data quality work. It is not repeated here.

| Sheet | Contents |
|---|---|
| **About** | Purpose, tools used and the status key |
| **Issue Log** | 49 issues: ID, stage, table, column, what I found, why it matters, what I did, the result and status |
| **Validation Checks** | Key figures and reconciliations that show the numbers add up |
| **Raw Issue Coverage** | Every line of the supplied Data Quality Report mapped to the log entry that covers it |

**Status.** All 49 issues are closed. In the log, a closed issue is marked **Resolved** (fixed and checked) or **Decided** (approach chosen and applied in the data). None are open.

**ID numbering.** IDs run from ISS-001 to ISS-050. ISS-010 is not used, so the log holds 49 entries.

---

## What this project demonstrates

- Investigating a data problem to find its cause before fixing it, then proving the fix worked (an instinct from my QA background)
- Choosing fixes that protect the numbers: flagging instead of deleting, and labelling gaps instead of guessing
- Combining two sources into one clean table (the marketplace feed) without duplicating the work
- Building a relational model with validated relationships and reconciling its results to the source
- Turning the model into a five-page dashboard where every KPI has a stated definition and reconciles to the others
- Writing down every decision and assumption so someone else can review it
- Reporting suspicious patterns instead of quietly correcting them

---

## Project status

**Built and documented:** data quality investigation, cleaning of all tables, data model, measures, validation checks, the Issue Log (49 of 49 closed) and the five-page dashboard.

**In progress:** final dashboard review, screenshots and repository files.

**Planned:** findings and recommendations document, supporting documentation files.

---

## Data source

The data is fictional, generated for portfolio practice. Any resemblance to real businesses, people or transactions is coincidental.

## Author

**Kolawole Opeyemi Oscar**, Quality Assurance Engineer moving into data analytics

- Portfolio: [oscarcodelab.github.io](https://oscarcodelab.github.io)
- LinkedIn: [kolawole-opeyemi](https://www.linkedin.com/in/kolawole-opeyemi-3063231b2)
- GitHub: [OscarCodeLab](https://github.com/OscarCodeLab)
- Email: kolawoleopeyemioscar@gmail.com

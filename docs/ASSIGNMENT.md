# Power BI Assignment: Nordkompass Outdoor Gear — Sales Dashboard

A hands-on, self-paced Power BI assignment built around a fictional Stockholm-based
outdoor-gear retailer. Use it with **Nordkompass_SalesData.xlsx**. Everything here uses
only Power BI Desktop, which is free.

## The scenario

You are the Data Analyst for Nordkompass, an outdoor-gear retailer with online, retail
store, and marketplace sales channels. Leadership wants a dashboard to understand sales
performance, customer behaviour, and progress against 2025 targets.

## The data (5 sheets in the workbook)

| Sheet | Grain | Notes |
|---|---|---|
| `Orders` | one row per order | Status (Completed/Cancelled/Returned), Channel, OrderDate |
| `OrderLines` | one row per product per order | Quantity, UnitPrice, Discount, LineAmount |
| `Customers` | one row per customer | City, Segment, SignupDate |
| `Products` | one row per product | Category, SubCategory, cost and price |
| `SalesTargets_2025` | one row per category per month | TargetRevenue for 2025 |

**The data is deliberately messy**, on purpose — real exports look like this, and cleaning
it is part of the exercise:
- Some dates in `Orders` are formatted `DD/MM/YYYY` instead of `YYYY-MM-DD`
- Some `City` values have extra spaces, inconsistent capitalisation, or are blank
- Some `Category` values are lower-case instead of title-case
- A handful of exact duplicate rows exist in `Customers` and `Orders`
- Some `Email` values are missing

---

## Part 1 — Import and clean (Power Query)

1. Open Power BI Desktop → **Get Data > Excel** → select `Nordkompass_SalesData.xlsx` →
   load all 5 sheets into **Power Query** (click Transform Data, not Load, so you can
   clean first).
2. In Power Query, for each table:
   - **Orders**: fix the mixed date formats in `OrderDate` so the whole column parses as a
     proper date type. (Hint: you may need to split the column into two cleanup paths
     based on which format a row uses, or use `Date.FromText` with error handling.)
   - **Orders**: remove exact duplicate rows.
   - **Customers**: trim whitespace from `CustomerName` and `City`, and fix inconsistent
     capitalisation. Remove exact duplicate rows.
   - **Customers**: decide how to handle blank `City` values (replace with "Unknown" is a
     reasonable choice — document your decision).
   - **Products**: fix inconsistent capitalisation in `Category`.
3. Rename each query to a clean name if needed, then **Close & Apply**.

**Checkpoint:** row counts after cleaning should be close to (but slightly less than)
the raw counts, since you removed duplicates. Note your before/after counts somewhere
(a text box on a dashboard page, or your own notes) — you'll want this for an interview
story about data cleaning.

## Part 2 — Build the data model

1. Go to the **Model** view.
2. Create relationships:
   - `Orders[CustomerID]` → `Customers[CustomerID]`
   - `OrderLines[OrderID]` → `Orders[OrderID]`
   - `OrderLines[ProductID]` → `Products[ProductID]`
3. Create a proper **Date table** (Power BI has a "New Table" DAX option, or build one
   manually with `CALENDAR()`), and relate it to `Orders[OrderDate]`. This is a core
   analytics-engineering habit: always model dates with a dedicated date table rather
   than relying on the raw date column directly.
4. Check your model is a clean star schema: `OrderLines` and `Orders` as facts, `Customers`,
   `Products`, and your new `Date` table as dimensions.

## Part 3 — DAX measures

Create these as explicit measures (not calculated columns) in a dedicated "Measures" table:

1. `Total Revenue` = sum of `LineAmount` for orders where Status = "Completed"
2. `Total Orders` = distinct count of completed OrderIDs
3. `Average Order Value` = Total Revenue ÷ Total Orders
4. `Cancelled Order Rate` = % of all orders with Status = "Cancelled"
5. `Returned Order Rate` = % of all orders with Status = "Returned"
6. `YTD Revenue` = year-to-date Total Revenue, using time intelligence (`TOTALYTD`)
7. `Revenue vs Target %` = Total Revenue ÷ sum of `TargetRevenue` from `SalesTargets_2025`,
   for 2025 only (this one requires relating the targets table by Category and Month —
   think about whether you need a relationship or a different DAX approach, since
   `SalesTargets_2025` won't relate cleanly to your Date table by default)
8. `Revenue MoM %` = month-over-month revenue change, using `DATEADD` or similar

## Part 4 — Build the report (3 pages)

**Page 1 — Executive Overview**
- Card visuals: Total Revenue, Total Orders, Average Order Value, Revenue vs Target %
- Line chart: monthly revenue trend, 2024–2025
- Bar chart: revenue by Category

**Page 2 — Customers**
- Map or bar chart: revenue by City
- Bar chart: revenue by Segment
- Table: top 10 customers by lifetime revenue
- A slicer for Channel

**Page 3 — Operations**
- Cancelled Order Rate and Returned Order Rate as cards
- Bar chart: orders by Channel
- Table: revenue by Category vs Target (2025), conditionally formatted to flag categories
  below target

Add a consistent title, a date slicer, and apply a simple consistent theme (View > Themes).

## Part 5 — Publish

1. **File > Publish > Publish to Power BI** (needs a free Power BI account).
2. In the Power BI Service, open the report → **File > Publish to web** to get a public
   link.
3. Save that link — it's what you'll put in your GitHub README and LinkedIn Featured
   section.

---

## What "done" looks like

- [ ] All 5 tables imported and cleaned in Power Query, with duplicates and format issues
      fixed
- [ ] A proper star schema with a dedicated Date table
- [ ] All 8 DAX measures working correctly
- [ ] 3 report pages built, each answering a clear business question
- [ ] Report published, with a public link saved

## Stretch goals (optional, if you want to go further)

- Add a `Customer Lifetime Value` measure and a top/bottom customer analysis
- Add a cohort-style "new vs returning customer" measure using `SignupDate` vs `OrderDate`
- Try a simple what-if parameter (e.g., a discount-sensitivity slider)
- Recreate one of your SQL practice queries (from the retail SQL repo) as a DAX measure,
  and compare the two approaches — this is a great interview talking point

## Interview angle

When you talk about this project, mention explicitly: *"I deliberately built the dataset
with realistic data-quality issues — inconsistent dates, duplicates, mixed casing — and
cleaned them in Power Query before modelling."* That's a stronger, more specific answer
than "I made a dashboard," and it's exactly what Analyst and Analytics Engineer interviews
probe for.

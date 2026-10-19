# Nordkompass Sales Dashboard — Power BI Project

A self-built Power BI project using a realistic, deliberately messy retail sales dataset
for a fictional Stockholm outdoor-gear retailer. Built to practice and demonstrate
Power Query data cleaning, star-schema data modeling, DAX measures, and dashboard design.

**Live dashboard:** 
## Below link is not public
(https://app.powerbi.com/groups/me/reports/8dc4c4b9-0f9e-44cd-aa63-3883468ca874/4b66936917b93de27603?experience=power-bi)
You can see the video attached how these reports or dashboard looks like

##This video shows all the above reports and how they interact. 
https://github.com/user-attachments/assets/2e54a061-191c-46ed-ab2d-2ae11af76fbc

**Full assignment brief:** [docs/ASSIGNMENT.md](docs/ASSIGNMENT.md)

## What this project demonstrates

- **Data cleaning (Power Query)**: fixing inconsistent date formats, removing duplicate
  rows, trimming whitespace, standardizing inconsistent text casing, and handling missing
  values — the same kind of messy-export problems you hit in real ETL work.
- **Data modeling**: a proper star schema (fact tables for orders and order lines, dimension
  tables for customers, products, and date) with a dedicated date table, not just the raw
  date column.
- **DAX**: 8 measures covering revenue, order metrics, time intelligence (YTD, month-over-
  month), and performance against monthly targets.
- **Dashboard design**: a 3-page report (Executive Overview, Customers, Operations) built
  around real business questions rather than "a chart for every column."

## The data

`data/Nordkompass_SalesData.xlsx` — 5 sheets:

| Sheet | Rows | Grain |
|---|---|---|
| Orders | 955 | one row per order |
| OrderLines | 1,819 | one row per product per order |
| Customers | 166 | one row per customer |
| Products | 30 | one row per product |
| SalesTargets_2025 | 48 | one row per category per month |

The data is synthetic but intentionally messy: mixed date formats, duplicate rows,
inconsistent capitalization, and missing values — see [docs/ASSIGNMENT.md](docs/ASSIGNMENT.md)
for the full list and the cleaning tasks tied to each one.

## How to reproduce this project

1. Open Power BI Desktop (free) and import `data/Nordkompass_SalesData.xlsx`.
2. Follow [docs/ASSIGNMENT.md](docs/ASSIGNMENT.md) end to end: clean the data in Power
   Query, build the model, write the DAX measures, build the 3 report pages, then publish.
3. Export your finished report as a PDF (File > Export > Export to PDF) and save it as
   `docs/Nordkompass_Dashboard.pdf` so the dashboard is viewable without opening Power BI.
4. Take a screenshot of each report page and save them in `screenshots/`.
5. Publish the report (File > Publish to web) and add the link at the top of this README.

## Repo structure

```
nordkompass-powerbi/
├── data/
│   └── Nordkompass_SalesData.xlsx   # the 5-sheet source dataset
├── docs/
│   ├── ASSIGNMENT.md                # full task brief: cleaning, modeling, DAX, pages
│   └── Nordkompass_Dashboard.pdf    # exported report (add after building)
├── screenshots/                      # page-by-page screenshots (add after building)
├── Nordkompass_Dashboard.pbix        # the Power BI file itself (add after building)
└── README.md
```

## Progress

- [ ] Data imported and cleaned in Power Query
- [ ] Star schema built with a dedicated date table
- [ ] 8 DAX measures implemented
- [ ] Executive Overview page
- [ ] Customers page
- [ ] Operations page
- [ ] Report published, link added above
- [ ] Screenshots and exported PDF added

## Tech stack

Power BI Desktop · Power Query (M) · DAX · Star schema data modeling

## Background

Built as part of a broader portfolio alongside a [SQL practice
repo](#) covering the same kind of star-schema modeling in SQL — together they show the
same data-engineering thinking applied from both the SQL/ETL side and the BI/reporting side.






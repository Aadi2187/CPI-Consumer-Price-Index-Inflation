# India CPI Inflation Study

An Excel-based analysis of India's Consumer Price Index (CPI) for the National Statistical Office case study. The project cleans the Government of India CPI dataset (Jan 2013 - May 2023) and answers five business questions on inflation drivers, trends and shocks.

## Files

| File | Description |
|------|-------------|
| `CPI_Inflation_Case_Study.xlsx` | Complete solution workbook (data, cleaning, pivots, charts) |
| `CPI_Inflation_Answers.pdf` | Written answers to the 5 questions with tables and charts |
| `CN-Excel-Lec29-INDIA_CPI_INFLATION_STUDY.pdf` | Original problem statement |
| `README.md` | This file |

## Dataset

- **Source:** Government of India (MoSPI / NSO) CPI index.
- **Coverage:** January 2013 - May 2023, 124 months.
- **Sectors:** Rural, Urban, Rural+Urban (CPI-R, CPI-U, combined).
- **Categories:** 27 columns, from Cereals and products to General index.
- **Values:** index numbers, not prices. They cannot be summed across months; use percentage changes for inflation.

## Workbook guide

| Sheet | Purpose |
|-------|---------|
| `Raw Data` | Original data (numbers stored as text, "NA" for gaps) |
| `ConsumerPriceIndex` | Cleaned data: numeric, gaps imputed |
| `CPI-Unpivot` | Long format (Sector, Year, Month, Category, CPI Value) with the bucket mapping ("Classified As"); 10,044 rows |
| `Insight1` | Q1: category contribution to the CPI basket |
| `Insight2` | Q2: Y-o-Y inflation trend from 2017 |
| `Insight3` | Q3: food prices, month-on-month, 12 months to May 2023 |
| `Insight4` | Q4: COVID-19 impact (Food, Health, Household) |
| `Insight5` | Q5: correlation of oil proxy with other categories |
| `Bonus Insights` | Food categories, 2022 vs 2023 by sector |

## Method

1. **Cleaning.** Converted text to numbers and replaced "NA" values. Rural Housing is missing in every month and April-May 2020 is missing for almost all categories (lockdown), so these were imputed with a moving-average approach.
2. **Bucketing.** Grouped the 27 categories into 10 buckets (Food, Luxury, Clothing, Housing, Fuel and light, Health, Education, Transport and communication, Misc, General Index) using the mapping in the problem statement.
3. **Contribution (Q1).** Sum of index values per bucket for the latest month (May 2023) divided by the total, with General Index excluded so shares add to 100%.
4. **Inflation rates.**
   - Month-on-month = (CPI this month - CPI last month) / CPI last month x 100
   - Annual = (average index this year - average index last year) / average index last year x 100
5. **Correlation (Q5).** `=CORREL` on month-on-month % changes, Jan 2021 - May 2023. "Fuel and light" is the proxy for imported oil prices because the dataset has no crude series.

## Key findings

| # | Question | Answer |
|---|----------|--------|
| 1 | Largest contribution to the CPI basket | **Food, about 45.8%** (Luxury 15.7% is next) |
| 2 | Year with the highest Y-o-Y inflation (2017 onward, Rural+Urban) | **2022 at 6.62%**, driven by the Russia-Ukraine war, edible-oil and fuel prices, and supply-chain disruption |
| 3 | Food MoM extremes, Jun 2022 - May 2023 | Highest **Oct 2022 (+0.97%)**, lowest **Dec 2022 (-1.36%)**; biggest absolute rise **Spices (+32.7 pts)** |
| 4 | COVID-19 effect | Food inflation jumped from 4.24% (2019) to **7.77% (2020)**; Health slowed in 2020 then rose to 7.76% in 2021; Transport accelerated most after March 2020 |
| 5 | Category most tied to oil price moves | **Transport and communication (r = 0.67)**; Education only 0.34 |

See `CPI_Inflation_Answers.pdf` for the full tables, charts and reasoning.

## Notes and limitations

- 2019 has 11 months of data and 2023 only 5 (Jan-May), so those annual averages are partial.
- April-May 2020 values are imputed, so COVID-period results lean on estimated points.
- The `Insight1` pivot counts rows (Food = 44.4%) rather than summing latest-month index values (Food = 45.8%). Both give the same ranking.
- The mapping places aggregate rows ("Food and beverages", "Clothing and footwear") inside Food and Clothing, which slightly overstates those buckets.
- Time-series questions (Q3-Q5) average the three sectors, matching the workbook pivots; Q2 uses the Rural+Urban column.
- Correlation shows association, not causation.

## How to reproduce

1. Open `CPI_Inflation_Case_Study.xlsx` in Excel.
2. Data tab: Get Data > From Text/CSV to load the raw CPI file, then clean it (Power Query) and impute the gaps.
3. Unpivot the category columns and add the bucket mapping.
4. Build a PivotTable and chart for each insight sheet.
5. Use `=CORREL(range1, range2)` for the Q5 correlations.

## Tools

Microsoft Excel (Power Query, PivotTables, charts, `CORREL`).

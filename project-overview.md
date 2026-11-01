# Orlando Tourism Tracker

**A monthly pulse on Orlando's visitor economy, built entirely from public data.**

Author: Medha Subramaniyan · Status: Data collected, analysis in progress · Last updated: November 2026

---

## 1. Overview

Orlando is one of the most visited places in the U.S., and tourism drives a large part of its economy. But there isn't one simple, public, up-to-date view of how tourism is doing month to month. The numbers are spread across county press releases, federal databases, and PDFs. Most people only see them as one-off headlines like "record month" or "tourism dips."

This project pulls those scattered sources into one clean monthly dataset. It uses that dataset to show seasonality, trends, and turning points, forecasts the next 12 months, and presents everything in a polished dashboard with a local story.

**Core question:** *Is Orlando tourism still growing, and what's driving the ups and downs?*

---



## 2. Why this project

- **It's a real local story.** Epic Universe opened in May 2025. Orlando is coming off COVID and records high tourism tax months, yet some recent months show softness. There's a live "boom or plateau?" question to answer.
- **It's useful to clients.** Hotels, restaurants, event promoters, attractions, and real estate agents in Central Florida all care about when visitors come and whether demand is growing. 
- **Most importantly I'm curious and interested in toursim and hospitiality and their impacts on consumer behavior in my backyard!** 

---



## 3. Goals



### Primary goals

1. Build a **clean, validated monthly dataset** (2012 to today) that combines tourism tax, airport, and employment data.
2. **Explain the patterns:** seasonality, long-term growth, COVID collapse and recovery, and recent momentum.
3. **Forecast** tourism tax collections for the next 12 months, with uncertainty ranges.
4. Publish a **dashboard** that a non-technical person (a business owner, a reporter) can understand in 30 seconds.



### Secondary goals

- Measure the **"Epic Universe effect"** on Orange County vs. Osceola County.
- Test whether **airport passengers lead tax collections**, and by how many months.
- Write a short **story piece** (Substack / LinkedIn) from the findings.



### Non-goals

- Hotel-level or property-level data. Detailed hotel data (STR) is paid, so it's out of scope.
- Real-time or daily data. This is a monthly tracker.
- Economic impact modeling, such as total visitor spending or jobs created.

---



## 4. Key questions


| #   | Question                                                                                           | How it's answered                                                                  |
| --- | -------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------- |
| 1   | What does a "normal" Orlando tourism year look like?                                               | Seasonal breakdown: peak months (March, July, December) vs. low months (September) |
| 2   | Is tourism still growing, or has it plateaued?                                                     | Year-over-year growth, rolling 12-month totals, trend line                         |
| 3   | How hard did COVID hit, and when did each indicator fully recover?                                 | Recovery timeline: when each series got back to its 2019 level                     |
| 4   | Did Epic Universe change the trajectory?                                                           | Before/after comparison from May 2025, Orange (where the park is) vs. Osceola      |
| 5   | Is the Kissimmee/vacation-rental market behaving differently from the I-Drive/Disney hotel market? | Osceola vs. Orange growth comparison                                               |
| 6   | Does airport traffic predict tax collections?                                                      | Lagged correlation between MCO passengers and TDT                                  |
| 7   | What should we expect over the next 12 months?                                                     | Forecast model with confidence bands                                               |


---



## 5. Data

All data is public and free. It was collected in October–November 2026.


| Dataset            | Source                                         | What it measures                                                       | Coverage            |
| ------------------ | ---------------------------------------------- | ---------------------------------------------------------------------- | ------------------- |
| Orange County TDT  | Orange County Comptroller (163 monthly PDFs)   | Hotel and short-term rental tax collections                            | Jan 2013 – Aug 2026 |
| Osceola County TDT | Osceola County BOCC (multi-year tables)        | Lodging tax collections in the Kissimmee/vacation-rental market        | Oct 2012 – Aug 2026 |
| MCO passengers     | U.S. Bureau of Transportation Statistics (BTS) | Scheduled departing passengers, domestic and international             | Jan 2012 – Jul 2026 |
| Tourism employment | BLS via FRED (`ORLA712LEIHN`)                  | Leisure and hospitality jobs in the Orlando metro area                 | Jan 1990 – Sep 2026 |
| Market Indicators  | Visit Orlando (one month)                      | Background: hotel occupancy, average daily rate, convention attendance | June 2026           |


**Why these three indicators together:**

- **Airport passengers** show visitors arriving.
- **Tax collections** show visitors spending on lodging.
- **Jobs** show the economy responding.

When all three move together, it's a real trend. When they split apart, that's the story.

**Known data caveats**

- TDT is recorded by *collection* month, which runs about 1–2 months behind actual hotel stays.
- The counties use an Oct–Sep fiscal year. Everything is converted to calendar months.
- BTS counts scheduled departing passengers only, about half of MCO's headline total.
- Orange County April 2014 is missing online. It can be recovered from the April 2015 report.
- Osceola's printed fiscal-year totals have small errors (FY19-20 is off by $20K), so totals are recalculated from the monthly numbers.

---



## 6. Approach



### Phase 1: Data pipeline ✅ partly done

- [x] Collect all source files into `orlando-tourism/raw/`
- [x] Build Osceola, MCO, and FRED monthly CSVs, checked against published totals
- [x] Confirm all 163 Orange PDFs can be read
- [ ] Write a Python script that pulls the monthly collection value out of each Orange PDF
- [ ] Fill in April 2014, then reconcile each extracted number against the prior-year figure printed in the next year's report
- [ ] Merge everything into one master table: `data/orlando_tourism_monthly.csv`



### Phase 2: Exploratory analysis

- Seasonality charts (average month-by-month pattern)
- Year-over-year growth and rolling 12-month totals
- COVID drop and recovery timeline
- Orange vs. Osceola comparison
- Lag analysis: MCO passengers vs. TDT



### Phase 3: Forecasting

- Models: seasonal naive (baseline), SARIMA, and Prophet
- Handle COVID by excluding Mar 2020 – Dec 2021 from training, or by flagging those months
- Backtest on the last 12 months and report accuracy (MAPE)
- Final 12-month forecast with 80% and 95% intervals



### Phase 4: Dashboard and story

- Interactive dashboard (Streamlit, Tableau Public, or a web page) with:
  - Headline numbers: latest month, year-over-year change, rolling 12-month total
  - Trend and seasonality charts
  - Orange vs. Osceola toggle
  - Forecast chart
  - An "Epic Universe effect" panel
- A 600–900 word write-up for Substack/LinkedIn
- A one-page case study for the NODE Studios portfolio

---



## 7. Early signals (from collected data)

- **Osceola set a record in FY2024-25: $85.2M**, up 5%. FY2025-26 has already reached **$86.0M through August**, passing the full prior year with one month still to go.
- **Orange County opened 2026 with its highest January ever: $35.4M**, up 5.9%.
- **June 2026 looked soft across the board** (Visit Orlando): hotel occupancy -4.5%, MCO passengers -4.8%, Orange TDT -0.4%. Year-to-date is still up.
- **International travel is a growth driver.** MCO international passengers reached about 4.05M in 2025, an all-time high and well above 2019 (about 3.46M).

These are starting points to dig into, not conclusions.

---



## 8. Deliverables


| Deliverable            | Format                    | Audience                        |
| ---------------------- | ------------------------- | ------------------------------- |
| Master monthly dataset | CSV                       | Me / reproducibility            |
| Data pipeline          | Python scripts (GitHub)   | Technical reviewers             |
| Analysis notebook      | Jupyter                   | Technical reviewers             |
| Tourism dashboard      | Interactive web dashboard | Clients, general public         |
| Story write-up         | Substack / LinkedIn post  | General audience                |
| Case study             | One-page portfolio piece  | Recruiters, prospective clients |


---



## 9. Success criteria

- Dataset covers **2013–2026 with no unexplained gaps**, and every series is reconciled against published totals.
- Forecast **beats the seasonal-naive baseline** on the backtest.
- Someone unfamiliar with the data can answer *"Is Orlando tourism up or down, and is that normal for this time of year?"* within 30 seconds of opening the dashboard.
- The write-up has **one clear, defensible headline finding**.
- The pipeline can be **re-run each month** when new reports are released.

---



## 10. Skills demonstrated

- **Data engineering:** extracting numbers from PDFs, building multi-source pipelines, validation and reconciliation
- **Analytics:** time-series breakdown, year-over-year and rolling metrics, lag correlation, before/after event analysis
- **Forecasting:** SARIMA / Prophet, backtesting, handling a structural break (COVID)
- **Visualization and storytelling:** dashboard design, turning data into a local narrative
- **Business framing:** turning public data into insight that's useful to local businesses

---



## 11. Risks and open questions


| Risk / question                                                  | Mitigation                                                                      |
| ---------------------------------------------------------------- | ------------------------------------------------------------------------------- |
| Orange PDF layout may change across 13 years                     | Test extraction on every file; flag anything that doesn't match and fix by hand |
| COVID distorts trends and forecasts                              | Exclude or flag 2020–21; report results with and without it                     |
| The tax collection lag makes month-to-month comparisons noisy    | Test lags of 1–2 months; lean on rolling 12-month totals                        |
| Epic Universe effect may be hard to separate from overall growth | Use Osceola as a comparison group; present it as "evidence of," not proof       |
| Should Seminole County be added?                                 | Phase 2 stretch goal, possibly through Florida Department of Revenue data       |
| Dashboard tool?                                                  | Decide after Phase 2 based on how much interactivity is needed                  |


---



## 12. Future extensions

- Monthly auto-update when the Comptroller posts new reports
- Add Florida Department of Revenue sales-tax data for restaurants and admissions
- Overlay events: conventions, holidays, hurricanes, major concerts
- Connect to the Airbnb event-demand project for a short-term rental angle
- Package the dashboard as a free monthly "Orlando Tourism Pulse" for local businesses


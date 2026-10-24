# Care-Transition--Efficiency--Placement-Outcome-Analytics
:: {align="center"}
# 🏥 CarePath Analytics

### Care Transition Efficiency & Placement Outcomes

**From hospital discharge to the next destination --- turning transition
data into operational insight.**

![Healthcare
Analytics](https://img.shields.io/badge/Analytics-Healthcare-145D91?style=for-the-badge)
![Power
BI](https://img.shields.io/badge/Power%20BI-Dashboard-F2C811?style=for-the-badge&logo=powerbi&logoColor=black)
![SQL](https://img.shields.io/badge/SQL-Advanced%20Analysis-278B87?style=for-the-badge&logo=postgresql&logoColor=white)
![Python](https://img.shields.io/badge/Python-Synthetic%20Data-3776AB?style=for-the-badge&logo=python&logoColor=white)

*A healthcare BI portfolio project exploring transition delays,
placement matching, readmission signals, and patient experience.*
:::💡 The Big Idea
A hospital journey does not end when treatment ends. The next
step---going home, moving to a skilled nursing facility, or entering
rehabilitation---also matters.

What happens when a placement takes too long? Which transitions miss
their intended destination? What patterns deserve a closer look?

CarePath Analytics is an end-to-end data analytics project that
explores these questions through a synthetic patient-transition dataset,
an interactive dashboard, and SQL investigations.
The mission: Help operations teams see where transitions slow
down, understand how outcomes differ across groups, and identify
questions worth investigating next.

🎯 Business Questions
- ⏱️ Transition efficiency: How long does it take to move from
  placement order to physical transfer?
- 🎯 Placement alignment: How often does the actual destination
  match the intended destination?
- 🚧 Delay investigation: Which recorded delay reasons are
  associated with longer transition times?
- 🔁 Readmission signals: How does the recorded 30-day readmission
  rate vary by diagnosis?
- ⭐ Patient experience: What does satisfaction look like across
  the dataset?
- 📅 Trends over time: How do transition metrics change by
  admission month?
These are descriptive analytics questions. Patterns can guide further
investigation, but do not establish that one factor caused another.
🧭 Project at a Glance
  Component                           What it does
  Synthetic data generation           Creates 650 fictional
                                      patient-transition records

  Data quality practice               Includes selected inconsistencies,
                                      missing scores, invalid values, and
                                      long-wait outliers

  Excel                               Stores the transition dataset and
                                      data dictionary

  Power BI                            Provides the design and DAX
                                      foundation for an interactive BI
                                      report

  Interactive HTML dashboard          Lets viewers explore the dataset
                                      using filters and charts

  SQL                                 Demonstrates grouped analysis,
                                      conditional aggregation, CTEs, and
                                      window functions

  Business interpretation             Connects metrics to operational
                                      questions without overstating
                                  conclusions
🖥️ Explore the Dashboard
Start here:
[`care_transition_dashboard.html`](care_transition_dashboard.html)

The interactive dashboard includes: - KPI cards for transition count,
average transition time, placement success, readmission rate, and
satisfaction - Monthly average transition-time trend -
Target-versus-actual placement comparison - Placement match-rate
visualization - Readmission rate by diagnosis - Average transition time
by recorded delay reason - Filterable transition-record table

View locally: Download the HTML file and open it in a modern
browser. The charting library loads from a CDN, so an internet
connection is needed. GitHub shows HTML source rather than running it in
the repository preview. For a hosted version, publish it using GitHub
Pages or another static host.
Dashboard values are calculated from the included synthetic data and
update with filters. They are not real hospital performance results.

📁 Suggested Repository Structure
carepath-analytics/
├── README.md
├── care_transition_data.xlsx
├── generate_care_transitions.py
├── care_transition_dashboard.html
├── care_transition_powerbi_theme.json
├── care_transition_dax_measures.txt
└── sql/
    └── care_transition_analysis.sql
The sql/ folder and SQL file are suggested organization. Add them
when you save your SQL solutions in the repository.
🧪 Dataset & Data Quality
The workbook contains a CareTransitions sheet and a DataDictionary
sheet.

  Field                               Description
  PatientID                         Synthetic transition identifier
  AdmissionDate / DischargeDate   Admission and discharge dates
  PrimaryDiagnosis                  Diagnosis category
  InsuranceType                     Payer category
  CurrentLocation                   Current care setting
  TargetPlacement                   Intended post-acute destination
  ActualPlacement                   Recorded final destination
  TransitionTimeHours               Hours from placement order to physical move
  DelayReason                       Recorded reason associated with the transition
  ReadmissionFlag                   Supplied Yes/No 30-day readmission indicator
  PatientSatisfactionScore          Satisfaction rating from 1 to 5
Intentional data-quality challenges
The dataset includes selected examples of inconsistent capitalization
and whitespace, missing satisfaction scores, negative transition time,
discharge dates before admission dates, dates outside the intended
reporting window, inconsistent readmission flag capitalization, and
transition times above 100 hours.

These issues simulate data-quality challenges for practice. They are not
claims about real healthcare data.
Recommended cleaning workflow
1. Preserve the raw workbook as the source.
2. Trim and standardize categorical fields.
3. Convert dates and numeric fields to correct data types.
4. Flag invalid date sequences and negative durations.
5. Standardize readmission values to Yes and No.
6. Keep legitimate long-wait outliers visible for investigation.
7. Document exclusions and denominator rules before calculating KPIs.
Data principle: Do not silently delete unusual values. Separate
invalid records from valid but extreme operational events.
📊 Power BI Design
The intended report uses a healthcare-inspired visual language: navy for
structure, blue for primary measures, teal/green for positive
indicators, and restrained warning colors for exceptions.
  Area                    Visual                  Purpose
  Top row                 KPI cards               Summarize transition
                                                  volume and outcomes

  Main left               Line chart              Average transition
                                                  hours by month

  Main right              Clustered bar chart     Compare target and
                                                  actual placement counts

  Lower left              Donut chart             Matched versus
                                                  mismatched placements

  Lower middle            Horizontal bar chart    Readmission rates by
                                                  diagnosis

  Lower right             Bar chart               Transition duration by
                                                  delay reason

  Bottom                  Detail table            Inspect synthetic
                                                  records

  Filter area             Slicers                 Filter diagnosis,
                                                  insurance, location,
                                                  placement, delay, and
                                              readmission
Core DAX measures
Starter formulas are available in
[`care_transition_dax_measures.txt`](care_transition_dax_measures.txt).

Average transition time (excluding negative durations):
Avg Transition Time (Hours) =
CALCULATE(
    AVERAGE(FactCareTransitions[TransitionTimeHours]),
    FactCareTransitions[TransitionTimeHours] >= 0
)
Placement success rate:
Placement Success Rate =
VAR ValidTransitions =
    FILTER(
        FactCareTransitions,
        NOT ISBLANK(FactCareTransitions[TargetPlacement]) &&
        NOT ISBLANK(FactCareTransitions[ActualPlacement])
    )
VAR SuccessfulTransitions =
    COUNTROWS(
        FILTER(
            ValidTransitions,
            FactCareTransitions[TargetPlacement] =
                FactCareTransitions[ActualPlacement]
        )
    )
RETURN
    DIVIDE(
        SuccessfulTransitions,
        COUNTROWS(ValidTransitions)
    )
Modeling note: Create and validate a date table, relationships, and
category standardization before interpreting the measures. The example
DAX assumes the table is named FactCareTransitions; update names if
your model differs.
🧠 SQL Investigation Lab
Five interview-style investigations are included in the project plan:
1. High-risk diagnosis scan: Find diagnoses with longer average
   transitions and readmission rates above a chosen threshold.
2. Delay-reason analysis: Compare average and median transition
   times and the share of transitions by recorded reason.
3. Placement mismatch analysis: Rank target-versus-actual placement
   combinations by mismatch count.
4. Month-over-month trend: Use LAG() to calculate changes in
   average transition time.
5. Combined outcome scan: Identify diagnoses with higher
   readmission rates and below-overall-average satisfaction.
SQL skills demonstrated
GROUP BY · HAVING · Conditional aggregation · CTEs · Window
functions · LAG() · DENSE_RANK() · Percentiles · Safe division with
NULLIF()

Save the final queries in a sql/ directory so reviewers can run and
inspect them independently.
🔍 Metric Definitions & Guardrails
  Metric                              Definition
  Average transition time             Mean of valid, non-negative
                                      transition-hour values

  Placement success rate              Matching target/actual placements
                                      divided by records with both values
                                      present

  Readmission rate                    Yes flags divided by records with
                                      valid Yes or No flags

  Patient satisfaction                Mean of available scores; missing
                                      values remain missing

  Delayed transition                  For this portfolio analysis, a
                                      transition taking more than 24
                                  hours
The 24-hour threshold is a project-defined analytical choice, not a
universal clinical standard.

Interpretation guardrails: - A placement mismatch is not
automatically an inappropriate placement. - A readmission flag alone
does not provide a verified encounter timeline. - An association between
delay and readmission does not prove causation. - Small group sizes can
make percentages unstable; show counts alongside rates. - The dataset is
synthetic, so observed patterns are practice examples, not evidence
about actual hospitals.
🚀 Run the Data Generator
Requirements: Python 3.10+ recommended.
pip install pandas openpyxl numpy
python generate_care_transitions.py
The script generates care_transition_data.xlsx in the working
directory. A fixed random seed makes the dataset reproducible for the
same script version and compatible library behavior.
Open the interactive dashboard
1. Download or clone this repository.
2. Open care_transition_dashboard.html in a browser.
3. Use the filters to explore diagnosis, insurance, care location,
   target placement, delay reason, and readmission status.
4. Compare charts and the filtered record table.
Build the native Power BI report
1. Open Power BI Desktop.
2. Select Get Data → Excel workbook and import
   care_transition_data.xlsx.
3. Clean and standardize data in Power Query.
4. Import care_transition_powerbi_theme.json through View → Themes
   → Browse for themes.
5. Add measures from care_transition_dax_measures.txt.
6. Build visuals and validate filters and calculations.
The HTML dashboard is interactive, but it is not a Power BI .pbix
file. The native report must be assembled in Power BI Desktop.
💼 Why This Project Matters
This case study demonstrates a complete analytical workflow:

Raw data → Quality checks → Business definitions → DAX and SQL →
Interactive reporting → Responsible interpretation

It showcases skills relevant to Data Analyst, BI Analyst, and Business
Analyst roles: - Data cleaning and validation - KPI design and
denominator awareness - Dashboard layout and interaction design - SQL
aggregation and window functions - Translating operational questions
into measurable analysis - Communicating limitations clearly

The strongest insight is not simply which metric is highest. It is
knowing which question the metric can answer, which it cannot answer,
and what should be investigated next.
⚠️ Disclaimer
This project uses synthetic, fictional data for education, portfolio
demonstration, and interview preparation. It is not designed for
clinical decision-making, diagnosis, treatment, or evaluation of real
healthcare providers.
::: {align="center"}
### Made for curious analysts who want data to lead to better questions. 💙

**CarePath Analytics · Healthcare BI Portfolio Project**
:::

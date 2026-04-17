
# CreditLens : Power BI Loan Risk Intelligence Dashboard

### Dashboard Link : https://app.powerbi.com/groups/b8d73067-ef59-4297-8e0a-3b6aedd7d9e9/reports/584e1798-3e7f-4565-8e2a-bb7f6816592c/0ffa304319be933b6e4e?experience=power-bi

## Problem Statement

This dashboard provides a comprehensive view of loan default behavior, applicant demographics, and financial risk patterns across various borrower profiles. It enables financial institutions to identify high-risk segments, monitor year-over-year trends in default rates and loan volumes, and make data-driven lending decisions.

Since the overall default rate fluctuates between **11.50% and 11.75%** across years, and unemployed applicants carry the **highest default rate (3.39%)**, the institution must focus on employment-type-based risk segmentation and credit score profiling to reduce exposure.

Average loan amounts remain relatively stable across age groups (~₹126K–₹128K), indicating that risk differentiation must come from behavioral and demographic signals rather than loan size alone.

---

## Dashboard Pages

### 1. Loan Default & Overview
- Loan Amount by Purpose (Home, Business, Education, Auto, Other)
- Average Income by Employment Type
- Default Rate by Employment Type
- Average Loan Amount by Age Groups
- Default Rate (%) by Year (2013–2018)

### 2. Applicant Demographics & Financial Profile
- Median Loan Amount by Credit Score Category
- Average Loan Amount (High Credit) by Age Group & Marital Status
- Total Loan by Credit Score Bins (Adults & Middle Age Adults)
- Number of Loans by Education Type

### 3. Financial Risk Metrics
- YOY Default Rate Change by Year
- YOY Loan Amount Change by Year
- YTD Loan Amount by Credit Score Bins and Marital Status
- Income Bracket & Employment Type breakdowns

---

## Steps Followed

- **Step 1** : Loaded the dataset (`Loan_default`) into Power BI Desktop.
- **Step 2** : Opened Power Query Editor. Under the **View** tab → **Data Preview** section, enabled **Column Distribution**, **Column Quality**, and **Column Profile**.
- **Step 3** : Set column profiling to run on the **entire dataset** (not just the default 1000 rows).
- **Step 4** : Validated data quality — no major errors or empty values were found across key columns. Minor nulls were handled at the measure level.
- **Step 5** : Created a **dedicated Measures Table** to keep all DAX calculations organized and separate from raw data columns.
- **Step 6** : Selected a consistent report **theme** to maintain visual coherence across all three pages.
- **Step 7** : Added **Visual Filters (Slicers)** for key fields such as `EmploymentType`, `CreditScore Category`, `MaritalStatus`, `Age Group`, and `Year` to allow dynamic exploration.
- **Step 8** : Built **Card Visuals** for key KPIs — Total Loan Amount, Default Rate, Average Income, and YTD figures.
- **Step 9** : Created **Bar & Clustered Column Charts** to represent default rates and loan amounts across employment types, age groups, and education levels.
- **Step 10** : Added **Line Charts** to visualize YOY trends in default rate and loan amount changes over 2013–2018.
- **Step 11** : Created a **matrix visual** for Average Loan Amount (High Credit) broken down by Age Group and Marital Status.
- **Step 12** : Designed **Page 3 (Financial Risk Metrics)** with a stacked bar chart for YTD Loan Amount segmented by Credit Score Bins and Marital Status, along with income bracket and employment type filter context cards.
- **Step 13** : Added **text boxes**, **shapes**, and branding elements (title, tagline, logo) to each page for a polished, professional layout.
- **Step 14** : Published the final report to **Power BI Service**.

---

## DAX Measures

### Default Rate by Year
```dax
Default Rate by Year = 
VAR totalLoans = CALCULATE(COUNTROWS('Loan_default'), ALLEXCEPT('Loan_default', 'Loan_default'[Year]))
VAR default    = CALCULATE(COUNTROWS(FILTER('Loan_default', 'Loan_default'[Default] = TRUE())), ALLEXCEPT('Loan_default', 'Loan_default'[Year]))
RETURN
DIVIDE(default, totalLoans) * 100

<img width="1920" height="1080" alt="Image" src="https://github.com/user-attachments/assets/5b761414-6158-42a6-b3ec-7793e418cd77" />
```

### Default Rate by Employment Type
```dax
Default Rate by Employment Type = 
VAR totalRecords = COUNTROWS(ALL('Loan_default'))
VAR defaultCases = COUNTROWS(FILTER('Loan_default', 'Loan_default'[Default] = TRUE()))
RETURN
CALCULATE(DIVIDE(defaultCases, totalRecords), ALLEXCEPT('Loan_default', 'Loan_default'[EmploymentType]))
```

<img width="541" height="376" alt="Image" src="https://github.com/user-attachments/assets/1959b27d-373c-4b65-9b94-1256425c5cdc" />


### YOY Loan Amount Change (%)
```dax
YOY Loan Amount Change = 
DIVIDE(
    CALCULATE(SUM('Loan_default'[LoanAmount]),
        'Loan_default'[Year] = YEAR(MAX('Loan_default'[Loan_Date_DD_MM_YYYY])))
    -
    CALCULATE(SUM('Loan_default'[LoanAmount]),
        'Loan_default'[Year] = YEAR(MAX('Loan_default'[Loan_Date_DD_MM_YYYY])) - 1),
    CALCULATE(SUM('Loan_default'[LoanAmount]),
        'Loan_default'[Year] = YEAR(MAX('Loan_default'[Loan_Date_DD_MM_YYYY])) - 1),
    0
)
```

<img width="760" height="295" alt="Image" src="https://github.com/user-attachments/assets/504b1212-2423-47dc-ac65-2307bdcc324c" />

> Additional measures for YTD Loan Amount, Median Loan by Credit Score, and Total Loan by Segment are organized in the Measures Table within the report.

---

## Insights

### [1] Default Rate Trends (2013–2018)

| Year | Default Rate (%) | YOY Change |
|------|-----------------|------------|
| 2013 | 11.62           | —          |
| 2014 | 11.60           | -0.0       |
| 2015 | 11.75           | +1.9       |
| 2016 | 11.50           | -2.8       |
| 2017 | 11.50           | -2.6       |
| 2018 | 11.70           | +0.8       |

→ Default rates have remained stubbornly range-bound between **11.50%–11.75%**, with no sustained downward trend.

---

### [2] Default Rate by Employment Type

| Employment Type | Default Rate |
|----------------|-------------|
| Unemployed      | 3.39%        |
| Full-time       | 3.01%        |
| Self-employed   | 2.86%        |
| Part-time       | 2.36%        |

→ **Unemployed applicants** carry the highest default risk — nearly **44% higher** than Part-time workers.

---

### [3] Average Income by Employment Type

| Employment Type | Avg. Income |
|----------------|------------|
| Full-time       | ₹82,890     |
| Unemployed      | ₹82,447     |
| Part-time       | ₹82,389     |
| Self-employed   | ₹82,272     |

→ Income differences across employment types are minimal, suggesting **income alone is not a reliable default predictor**.

---

### [4] Loan Amount by Purpose

| Purpose   | Total Loan Amount |
|-----------|------------------|
| Home      | 6,545M           |
| Education | 6,511M           |
| Auto      | 6,522M           |
| Business  | 6,498M           |
| Other     | 6,501M           |

→ Loan distribution across purposes is **nearly uniform**, indicating no single purpose dominates the portfolio.

---

### [5] Loan Amount by Age Group

| Age Group       | Avg. Loan Amount |
|----------------|-----------------|
| Adults          | ₹127,901         |
| Senior Citizens | ₹127,459         |
| Middle Age Adults| ₹126,674        |
| Teens           | ₹127,355         |

→ Average loan amounts are **consistent across age groups**, ruling out age as a sole risk factor.

<img width="1598" height="906" alt="Image" src="https://github.com/user-attachments/assets/ec104501-b233-41e8-a70a-f0d6f919bbfe" />

---

### [6] Median Loan by Credit Score Category

| Credit Score | Median Loan Amount |
|-------------|-------------------|
| Low          | ₹128,397           |
| High         | ₹127,764           |
| Very Low     | ₹127,515           |
| Medium       | ₹127,149           |

→ Interestingly, **Low credit score** applicants have the highest median loan amount — a pattern worth further investigation in the underwriting process.

---

### [7] Loans by Education Type

| Education     | Number of Loans |
|--------------|----------------|
| Bachelor's    | 64,366          |
| PhD           | 63,903          |
| Master's      | 63,541          |
| High School   | 63,537          |

→ Loan distribution across education levels is **broadly even**, with Bachelor's holders forming the largest borrower segment.

---

<img width="1598" height="901" alt="Image" src="https://github.com/user-attachments/assets/914f6445-c7c6-42a0-be14-2764136256d1" />

### [8] YTD Loan Amount Summary

| Segment         | Total       |
|----------------|-------------|
| All Loans       | ₹32.58 bn   |
| High Income     | ₹21.73 bn   |
| Medium Income   | ₹7.21 bn    |
| Low Income      | ₹3.63 bn    |

→ **High-income borrowers** account for ~67% of total YTD loan value, but their default concentration must be monitored given the high loan volume.

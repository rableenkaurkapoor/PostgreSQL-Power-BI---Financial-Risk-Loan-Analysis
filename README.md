# PostgreSQL & Power BI - Financial Risk & Loan Analysis

## End-to-End Loan Risk Analytics Pipeline
### (PostgreSQL + Data Modeling + Risk Scoring + BI-Ready Mart Layer)

---

## 🔹INDEX -
1. Project Overview
2. Business Problem Framing
3. Business Questions & Analytical Insights
4. Data Architecture (Staging → Core → Mart)
5. Data Transformation & Validation
6. Risk Segmentation & Risk Scoring Model
7. Mart Layer Analytical Views
8. Power BI Integration
9. BI Security Layer (Read-Only Role)
10. Data Model Documentation & ERD
11. Conclusion

---

## 1. Project Overview

Financial institutions require a structured analytics framework to monitor loan portfolio risk, identify default drivers, evaluate borrower risk across regions and employment segments, and support executive-level credit strategy decisions.

The objective of this project is to design an end-to-end analytics pipeline that transforms raw loan data into structured, decision-ready insights using PostgreSQL and Power BI.

---

## 2. BusinessAnalysis – Problem Framing

This project was approached from a Business Analyst perspective by first defining the core risk questions stakeholders needed answered. The objective was to translate business requirements into measurable analytical outputs that support credit risk strategy and executive decision-making.

The table below illustrates how stakeholder business questions were translated into measurable KPIs and implemented as analytical views within the PostgreSQL Mart layer.

| BUSINESS QUESTIONS - | Output (KPI) | |
|---|---|---|
| What is the total loan volume and default exposure by region? | Total loans + Default rate % | mart.region_risk_dashboard |
| How does employment type impact default risk? | Default rate by employment type | mart.employment_risk_dashboard |
| How are borrowers segmented by credit risk? | High / Medium / Low classification | mart.risk_segment_distribution |
| What is the average composite risk score by region? | Average composite risk score | mart.avg_risk_score_by_region |
| How can executives view consolidated portfolio risk metrics? | Consolidated KPI summary | mart.executive_risk_summary |

---

## 3. Business Questions & Analytical Insights

The following analyses translate key lending risk questions into SQL-based insights to evaluate loan portfolio performance, borrower risk patterns, and regional exposure.

---

### 🔹 How many loans were issued in each region?

**Key Insight:** The West region recorded the highest loan volume (280 loans) followed closely by the Northeast (258), indicating stronger lending demand and portfolio exposure in these regions.

**Image Placeholder:**  
![How many loans were issued in each region](images/how-many-loans-were-issued-in-each-region.png)

---

### 🔹 How many loans are Approved vs Default vs Late?

**Key Insight:** Most loans are Approved (503) or Paid Off (253), while 144 loans are defaulted, indicating that although the portfolio is largely performing, a noticeable portion of borrowers still present credit risk.

**Image Placeholder:**  
![How many loans are Approved vs Default vs Late](images/loan-status-breakdown.png)

---

### 🔹 Which region has the highest default risk?

**Key Insight:** The Northeast region shows the highest default rate (16.28%), suggesting higher borrower risk in this geography and indicating an area that may require stricter credit evaluation.

**Image Placeholder:**  
![Which region has the highest default risk](images/highest-default-risk-region.png)

---

### 🔹 Do lower credit score borrowers default more?

**Key Insight:** Borrowers with lower average credit scores show higher default occurrences, confirming that credit score is a strong indicator of borrower repayment risk.

**Image Placeholder:**  
![Do lower credit score borrowers default more](images/lower-credit-score-default.png)

---

### 🔹 Do people with higher Debt-to-Income ratio default more?

**Key Insight:** Borrowers with higher average debt-to-income ratios are more likely to default, highlighting DTI as an important financial stability indicator in credit risk assessment.

**Image Placeholder:**  
![Higher debt-to-income ratio default more](images/higher-dti-default.png)

---

## BUSINESS ANALYSIS -

### 🔹 Employment type vs default rate

**Key Insight:** Business owners show the highest default rate (16.22%), followed by salaried borrowers, indicating that employment type influences borrower repayment behavior.

**Image Placeholder:**  
![Employment type vs default rate](images/employment-type-vs-default-rate.png)

---

### 🔹 Create Risk Segments (High / Medium / Low risk classification)

**Key Insight:** Borrowers were successfully categorized into High, Medium, and Low risk segments, enabling clearer monitoring of portfolio risk exposure and supporting targeted lending strategies.

**Image Placeholder:**  
![Create Risk Segments](images/create-risk-segments.png)

---

### 🔹 Interest rate vs default (pricing risk alignment)

**Key Insight:** Higher-risk borrowers tend to be associated with slightly higher interest rates, suggesting that loan pricing partially reflects borrower risk levels.

**Image Placeholder:**  
![Interest rate vs default](images/interest-rate-vs-default.png)

---

### 🔹 Build a risk scoring logic

**Key Insight:** A composite risk score model was developed using credit score, debt-to-income ratio, and interest rate to provide a standardized metric for comparing borrower risk across the portfolio.

**Image Placeholder:**  
![Build a risk scoring logic](images/risk-scoring-logic.png)

---

### 🔹 Employment Risk Rank

**Key Insight:** employment categories were ranked by default rate, revealing that business owners contribute the highest risk, helping prioritize borrower segments for monitoring.

**Image Placeholder:**  
![Employment Risk Rank](images/employment-risk-rank.png)

---

### 🔹 Risk Segment Distribution %

**Key Insight:** More than half of borrowers fall into the High-Risk segment (56%), indicating a portfolio with a significant concentration of higher-risk borrowers.

**Image Placeholder:**  
![Risk Segment Distribution](images/risk-segment-distribution.png)

---

### 🔹 Average Risk Score per Region

**Key Insight:** The Northeast region shows the highest average risk score, confirming it as the most risk-exposed geographic segment in the portfolio.

**Image Placeholder:**  
![Average Risk Score per Region](images/average-risk-score-region.png)

---

### 🔹 Final Executive Summary View

**Key Insight:** This consolidated view integrates loan volume, default metrics, and risk indicators into a single decision-ready dataset, enabling executives to monitor portfolio risk and lending performance efficiently.

**Image Placeholder:**  
![Final Executive Summary View](images/final-executive-summary-view.png)

---

## 4. Data Architecture & Analytical Implementation (PostgreSQL Layer)

To support scalable loan risk analytics, I designed a structured database architecture named `loan_risk_db`.

I implemented a three-layer schema model (Staging → Core → Mart) to separate raw ingestion, cleaned production data, and business-ready analytical views.

**Image Placeholder:**  
![Loan Risk DB Architecture](images/loan-risk-db-architecture.png)

---

## 5. Controlled Raw Data Ingestion (Staging Layer Architecture)

To preserve raw source integrity and prevent schema conflicts during ingestion, I created a dedicated staging table (`staging.loan_raw`) where all fields were stored as text.

This controlled ingestion layer minimized type mismatch errors and established a clean separation between raw data loading and transformation logic.

**Image Placeholder:**  
![Staging Layer Architecture](images/staging-layer-architecture.png)

---

## 6. Data Transformation into Core Layer

I then engineered a structured production table (`core.loan`) by transforming and type-casting raw staging data into validated business attributes. This table was designed with appropriate data types, constraints, and a primary key to ensure referential integrity and analytical consistency.

**Image Placeholder:**  
![Core Layer Transformation](images/core-layer-transformation.png)

---

## 7. Performance Optimization

After structuring the core table, I implemented performance-optimized indexing strategies on high-selectivity analytical dimensions (`loan_status`, `region`, `employment_type`, `credit_score`) commonly used in filtering, grouping, and aggregation within BI reporting workloads.

---

## 8. Data Quality Validation

Before enabling downstream risk analytics, I implemented structured data quality controls on the `core.loan` table to ensure analytical reliability and governance compliance.

I validated primary key uniqueness (`loan_id`), enforced non-null integrity on critical business attributes (`region`, `employment_type`, `loan_status`), and verified acceptable value ranges for `credit_score` and `debt_to_income_ratio`.

These validation controls ensured that all 1,000 records met analytical integrity standards before progressing into the Mart layer.

### Data Quality Validation Queries on core.loan (Primary Key, Null Integrity & Range Checks)

**Image Placeholder:**  
![Data Quality Validation Queries](images/data-quality-validation-queries.png)

---

## 9. Mart Layer Analytical Views

With validated and performance-optimized production data in place, I transitioned into building the business-ready analytical Mart layer.

---

### mart.region_risk_dashboard

To evaluate loan performance and default exposure at the regional level.

I aggregated total loans, default counts, default rate %, average credit score, DTI, interest rate, and risk score by region. This view helps identify high-risk geographic segments.

**Image Placeholder:**  
![mart.region_risk_dashboard](images/mart-region-risk-dashboard.png)

---

### mart.employment_risk_dashboard

To analyze how employment type correlates with default risk.

I calculated loan counts, default rates, and risk metrics grouped by employment type to detect higher-risk borrower profiles.

After understanding region and employment risk, I introduced structured borrower classification.

**Image Placeholder:**  
![mart.employment_risk_dashboard](images/mart-employment-risk-dashboard.png)

---

### mart.risk_segment_distribution

To classify borrowers into High, Medium, and Low risk segments.

I implemented risk segmentation logic using credit score and DTI thresholds and calculated the percentage distribution of each segment.

This segmentation created clear borrower risk buckets that I later used to build a weighted risk scoring model.

**Image Placeholder:**  
![mart.risk_segment_distribution](images/mart-risk-segment-distribution.png)

---

### mart.avg_risk_score_by_region

To quantify regional risk using a weighted risk scoring model.

I engineered a weighted composite risk scoring model by normalizing credit score, scaling debt-to-income ratio, and incorporating interest rate sensitivity into a bounded 0–100 risk framework using lease and greatest controls.

This model quantified borrower risk at the regional level, enabling comparative risk benchmarking across geographic segments.

**Image Placeholder:**  
![mart.avg_risk_score_by_region](images/mart-avg-risk-score-by-region.png)

---

### mart.executive_risk_summary

To provide leadership with a high-level overview of portfolio risk exposure.

I combined loan volume, default metrics, and risk indicators into a single summarized view designed for strategic decision-making.

This view consolidated regional loan volume, default rates, average credit score, DTI, and composite risk score into a leadership-ready analytical layer.

It provides executives with a high-level portfolio risk overview to support strategic credit allocation and risk mitigation decisions.

**Image Placeholder:**  
![mart.executive_risk_summary](images/mart-executive-risk-summary.png)

---

### mart.master_region_dashboard

To consolidate multiple regional metrics into a structured reporting layer.

This view unified region-level loan volume, default metrics, credit indicators, and composite risk scores into a structured analytical reporting layer.

It standardizes regional KPIs into a consistent format for operational dashboards and comparative performance analysis.

**Image Placeholder:**  
![mart.master_region_dashboard](images/mart-master-region-dashboard.png)

---

### mart.powerbi_master

This view serves as a flattened semantic modeling layer designed specifically for BI consumption.

I consolidated row-level loan attributes, derived risk segments, composite risk scores, and standardized analytical metrics into a denormalized structure to reduce transformation logic inside Power BI.

By pushing business logic into PostgreSQL, I improved dashboard performance, simplified DAX calculations, and established a single source of truth for executive reporting.

This view is now connected directly to Power BI for executive dashboard creation.

**Image Placeholder:**  
![mart.powerbi_master](images/mart-powerbi-master.png)

---

## 10. Secure BI Access Layer – Read-Only bi_reader Role

To simulate a production-grade BI deployment environment, I implemented a dedicated read-only database role (`bi_reader`) to securely expose Mart-layer analytical views to Power BI.

This role enforces least-privilege access by granting:

- CONNECT access to the database
- USAGE on the Mart schema
- SELECT permissions on all current and future Mart tables and views

This ensures BI tools can query analytical outputs without write or modification privileges, maintaining data governance and security controls.

### Verification: bi_reader Access Validation

I validated role permissions by querying PostgreSQL system catalog tables to confirm `bi_reader` had SELECT access on all Mart-layer views.

**Image Placeholder:**  
![bi_reader access validation](images/bi-reader-access-validation.png)

---

## 11. Data Model Documentation & ERD

To document database structure and support architectural transparency, I exported an Entity Relationship Diagram (ERD) illustrating schema separation (Staging → Core → Mart) and logical data flow.

This documentation supports maintainability, onboarding, and analytical traceability across transformation layers.

**Image Placeholder:**  
![Data Model Documentation and ERD](images/data-model-documentation-erd.png)

---

## 12. Conclusion

This project demonstrates the development of an end-to-end loan risk analytics pipeline using PostgreSQL and Power BI. Raw financial data was transformed into structured analytical insights through a layered architecture consisting of staging, core, and mart schemas. By implementing risk segmentation, composite risk scoring, and business-ready analytical views, the project enables stakeholders to monitor loan portfolio performance, identify high-risk borrower segments, and support data-driven credit decision making.

---

## Tools Used

- PostgreSQL 17
- pgAdmin 4
- Power BI
- SQL
- Data Modeling
- Risk Scoring
- BI-Ready Mart Layer

---

## Folder Structure Suggestion

```bash
project-folder/
│
├── README.md
├── sql/
│   ├── staging_layer.sql
│   ├── core_layer.sql
│   ├── mart_views.sql
│   ├── validation_queries.sql
│   └── bi_reader_role.sql
│
├── images/
│   ├── how-many-loans-were-issued-in-each-region.png
│   ├── loan-status-breakdown.png
│   ├── highest-default-risk-region.png
│   ├── lower-credit-score-default.png
│   ├── higher-dti-default.png
│   ├── employment-type-vs-default-rate.png
│   ├── create-risk-segments.png
│   ├── interest-rate-vs-default.png
│   ├── risk-scoring-logic.png
│   ├── employment-risk-rank.png
│   ├── risk-segment-distribution.png
│   ├── average-risk-score-region.png
│   ├── final-executive-summary-view.png
│   ├── loan-risk-db-architecture.png
│   ├── staging-layer-architecture.png
│   ├── core-layer-transformation.png
│   ├── data-quality-validation-queries.png
│   ├── mart-region-risk-dashboard.png
│   ├── mart-employment-risk-dashboard.png
│   ├── mart-risk-segment-distribution.png
│   ├── mart-avg-risk-score-by-region.png
│   ├── mart-executive-risk-summary.png
│   ├── mart-master-region-dashboard.png
│   ├── mart-powerbi-master.png
│   ├── bi-reader-access-validation.png
│   └── data-model-documentation-erd.png
│
└── powerbi/
    └── dashboard.pbix

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

<img width="733" height="477" alt="image" src="https://github.com/user-attachments/assets/27aad3b6-7268-4830-8855-40aa089fdd3e" />

---

### 🔹 How many loans are Approved vs Default vs Late?

**Key Insight:** Most loans are Approved (503) or Paid Off (253), while 144 loans are defaulted, indicating that although the portfolio is largely performing, a noticeable portion of borrowers still present credit risk.
<img width="743" height="475" alt="image" src="https://github.com/user-attachments/assets/e66cb70a-abd5-4fb8-ae51-30a86121cb24" />


---

### 🔹 Which region has the highest default risk?

**Key Insight:** The Northeast region shows the highest default rate (16.28%), suggesting higher borrower risk in this geography and indicating an area that may require stricter credit evaluation.

<img width="732" height="656" alt="image" src="https://github.com/user-attachments/assets/3d006cb6-4f06-4717-9843-55c75195616d" />

---

### 🔹 Do lower credit score borrowers default more?

**Key Insight:** Borrowers with lower average credit scores show higher default occurrences, confirming that credit score is a strong indicator of borrower repayment risk.

<img width="730" height="491" alt="image" src="https://github.com/user-attachments/assets/5e608c80-a89e-444e-a2d3-95d73337258b" />

---

### 🔹 Do people with higher Debt-to-Income ratio default more?

**Key Insight:** Borrowers with higher average debt-to-income ratios are more likely to default, highlighting DTI as an important financial stability indicator in credit risk assessment.

<img width="737" height="477" alt="image" src="https://github.com/user-attachments/assets/074f3543-0066-4c3f-a4eb-dc7946e76c5a" />

---

## BUSINESS ANALYSIS -

### 🔹 Employment type vs default rate

**Key Insight:** Business owners show the highest default rate (16.22%), followed by salaried borrowers, indicating that employment type influences borrower repayment behavior.

<img width="735" height="490" alt="image" src="https://github.com/user-attachments/assets/2ad066b8-dbe5-42cc-9a84-2a3eb2d5b01e" />


---

### 🔹 Create Risk Segments (High / Medium / Low risk classification)

**Key Insight:** Borrowers were successfully categorized into High, Medium, and Low risk segments, enabling clearer monitoring of portfolio risk exposure and supporting targeted lending strategies.

<img width="753" height="492" alt="image" src="https://github.com/user-attachments/assets/f3a51df5-51f2-48de-a152-6bd74e8dcf00" />

---

### 🔹 Interest rate vs default (pricing risk alignment)

**Key Insight:** Higher-risk borrowers tend to be associated with slightly higher interest rates, suggesting that loan pricing partially reflects borrower risk levels.

<img width="723" height="648" alt="image" src="https://github.com/user-attachments/assets/b7dce2f5-76b4-451b-b56b-7e54017569cb" />

---

### 🔹 Build a risk scoring logic

**Key Insight:** A composite risk score model was developed using credit score, debt-to-income ratio, and interest rate to provide a standardized metric for comparing borrower risk across the portfolio.

<img width="743" height="452" alt="image" src="https://github.com/user-attachments/assets/b14bb1fa-ca00-488d-8917-1be1d3c4cca0" />

---

### 🔹 Employment Risk Rank

**Key Insight:** employment categories were ranked by default rate, revealing that business owners contribute the highest risk, helping prioritize borrower segments for monitoring.

<img width="717" height="492" alt="image" src="https://github.com/user-attachments/assets/d90b6fc7-95b8-4380-a682-3ec643411dfd" />

---

### 🔹 Risk Segment Distribution %

**Key Insight:** More than half of borrowers fall into the High-Risk segment (56%), indicating a portfolio with a significant concentration of higher-risk borrowers.

<img width="760" height="465" alt="image" src="https://github.com/user-attachments/assets/8d29eace-aeff-4832-aa14-d82d8126215d" />

---

### 🔹 Average Risk Score per Region

**Key Insight:** The Northeast region shows the highest average risk score, confirming it as the most risk-exposed geographic segment in the portfolio.

<img width="765" height="387" alt="image" src="https://github.com/user-attachments/assets/9d91792b-6321-4847-896b-ca14dbf26fee" />

---

### 🔹 Final Executive Summary View

**Key Insight:** This consolidated view integrates loan volume, default metrics, and risk indicators into a single decision-ready dataset, enabling executives to monitor portfolio risk and lending performance efficiently.

<img width="733" height="360" alt="image" src="https://github.com/user-attachments/assets/f71ffc48-a005-4eae-a9a8-74b05d0df7a1" />

---

## 4. Data Architecture & Analytical Implementation (PostgreSQL Layer)

To support scalable loan risk analytics, I designed a structured database architecture named `loan_risk_db`.

I implemented a three-layer schema model (Staging → Core → Mart) to separate raw ingestion, cleaned production data, and business-ready analytical views.

<img width="866" height="507" alt="image" src="https://github.com/user-attachments/assets/e830bc06-3d1a-4c40-86df-d0dea93674e1" />

---

## 5. Controlled Raw Data Ingestion (Staging Layer Architecture)

To preserve raw source integrity and prevent schema conflicts during ingestion, I created a dedicated staging table (`staging.loan_raw`) where all fields were stored as text.

This controlled ingestion layer minimized type mismatch errors and established a clean separation between raw data loading and transformation logic.

<img width="582" height="508" alt="image" src="https://github.com/user-attachments/assets/00708ad9-b74b-49e7-9a42-3be223482839" />

---

## 6. Data Transformation into Core Layer

I then engineered a structured production table (`core.loan`) by transforming and type-casting raw staging data into validated business attributes. This table was designed with appropriate data types, constraints, and a primary key to ensure referential integrity and analytical consistency.

<img width="888" height="537" alt="image" src="https://github.com/user-attachments/assets/cd300807-8e2f-4f40-9f36-5341e02dc7e2" />


---

## 7. Performance Optimization

After structuring the core table, I implemented performance-optimized indexing strategies on high-selectivity analytical dimensions (`loan_status`, `region`, `employment_type`, `credit_score`) commonly used in filtering, grouping, and aggregation within BI reporting workloads.

---

## 8. Data Quality Validation

Before enabling downstream risk analytics, I implemented structured data quality controls on the `core.loan` table to ensure analytical reliability and governance compliance.

I validated primary key uniqueness (`loan_id`), enforced non-null integrity on critical business attributes (`region`, `employment_type`, `loan_status`), and verified acceptable value ranges for `credit_score` and `debt_to_income_ratio`.

These validation controls ensured that all 1,000 records met analytical integrity standards before progressing into the Mart layer.

<img width="848" height="552" alt="image" src="https://github.com/user-attachments/assets/17e418bb-9836-427b-aade-950d5ab14594" />

---

### Data Quality Validation Queries on core.loan (Primary Key, Null Integrity & Range Checks)
---

## 9. Mart Layer Analytical Views

With validated and performance-optimized production data in place, I transitioned into building the business-ready analytical Mart layer.

---

### mart.region_risk_dashboard

To evaluate loan performance and default exposure at the regional level.

I aggregated total loans, default counts, default rate %, average credit score, DTI, interest rate, and risk score by region. This view helps identify high-risk geographic segments.

<img width="897" height="708" alt="image" src="https://github.com/user-attachments/assets/7479bcbb-6111-4b57-ad97-a073ba2c33c2" />


---

### mart.employment_risk_dashboard

To analyze how employment type correlates with default risk.

I calculated loan counts, default rates, and risk metrics grouped by employment type to detect higher-risk borrower profiles.

After understanding region and employment risk, I introduced structured borrower classification.

<img width="877" height="617" alt="image" src="https://github.com/user-attachments/assets/4f2e035a-31ef-4e1d-bbbe-eeed94c99e97" />

---

### mart.risk_segment_distribution

To classify borrowers into High, Medium, and Low risk segments.

I implemented risk segmentation logic using credit score and DTI thresholds and calculated the percentage distribution of each segment.

This segmentation created clear borrower risk buckets that I later used to build a weighted risk scoring model.

<img width="896" height="782" alt="image" src="https://github.com/user-attachments/assets/80a85a7d-9d54-4601-a441-3404af02ef40" />

---

### mart.avg_risk_score_by_region

To quantify regional risk using a weighted risk scoring model.

I engineered a weighted composite risk scoring model by normalizing credit score, scaling debt-to-income ratio, and incorporating interest rate sensitivity into a bounded 0–100 risk framework using lease and greatest controls.

This model quantified borrower risk at the regional level, enabling comparative risk benchmarking across geographic segments.

<img width="885" height="605" alt="image" src="https://github.com/user-attachments/assets/382b780f-a633-47ea-a409-bab687526d74" />


---

### mart.executive_risk_summary

To provide leadership with a high-level overview of portfolio risk exposure.

I combined loan volume, default metrics, and risk indicators into a single summarized view designed for strategic decision-making.

This view consolidated regional loan volume, default rates, average credit score, DTI, and composite risk score into a leadership-ready analytical layer.

It provides executives with a high-level portfolio risk overview to support strategic credit allocation and risk mitigation decisions.

<img width="897" height="778" alt="image" src="https://github.com/user-attachments/assets/16175084-8bd1-42f3-86e2-d2129e3dc671" />


---

### mart.master_region_dashboard

To consolidate multiple regional metrics into a structured reporting layer.

This view unified region-level loan volume, default metrics, credit indicators, and composite risk scores into a structured analytical reporting layer.

It standardizes regional KPIs into a consistent format for operational dashboards and comparative performance analysis.

<img width="907" height="297" alt="image" src="https://github.com/user-attachments/assets/b162fd0e-a609-4beb-9b7a-72a86ee04771" />


---

### mart.powerbi_master

This view serves as a flattened semantic modeling layer designed specifically for BI consumption.

I consolidated row-level loan attributes, derived risk segments, composite risk scores, and standardized analytical metrics into a denormalized structure to reduce transformation logic inside Power BI.

By pushing business logic into PostgreSQL, I improved dashboard performance, simplified DAX calculations, and established a single source of truth for executive reporting.

This view is now connected directly to Power BI for executive dashboard creation.

<img width="887" height="472" alt="image" src="https://github.com/user-attachments/assets/91550f44-8bd3-4040-88a4-0e2726b4e930" />


---

## 10. Secure BI Access Layer – Read-Only bi_reader Role

To simulate a production-grade BI deployment environment, I implemented a dedicated read-only database role (`bi_reader`) to securely expose Mart-layer analytical views to Power BI.

<img width="896" height="472" alt="image" src="https://github.com/user-attachments/assets/ff9f27d4-811f-4e58-8d6f-0295eda0dd64" />

This role enforces least-privilege access by granting:

- CONNECT access to the database
- USAGE on the Mart schema
- SELECT permissions on all current and future Mart tables and views

This ensures BI tools can query analytical outputs without write or modification privileges, maintaining data governance and security controls.

<img width="726" height="585" alt="image" src="https://github.com/user-attachments/assets/1d7daa51-3459-48f7-b8c1-81979d9ced96" />


### Verification: bi_reader Access Validation

I validated role permissions by querying PostgreSQL system catalog tables to confirm `bi_reader` had SELECT access on all Mart-layer views.

<img width="578" height="681" alt="image" src="https://github.com/user-attachments/assets/674fab4d-4473-4cad-bc4e-130c752ec9b2" />

---

## 11. Data Model Documentation & ERD

To document database structure and support architectural transparency, I exported an Entity Relationship Diagram (ERD) illustrating schema separation (Staging → Core → Mart) and logical data flow.

This documentation supports maintainability, onboarding, and analytical traceability across transformation layers.

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

NYC Jobs Data Engineering Coding Challenge - Documentation
1. Overview

This project implements a PySpark-based data pipeline to ingest, clean, transform, and analyze NYC job posting data. The goal is to explore the dataset, engineer meaningful features, compute business KPIs, and store curated results for downstream analytics.

Note: Initially, we faced a technical glitch while cloning the client-shared repository. Other public repositories cloned successfully, but the shared repo could not be cloned. The project was therefore created manually.

2. Data Exploration

Dataset contains structured CSV with job posting attributes.

Columns include:

Categorical: agency, job_category, salary_frequency, preferred_skills, business_title

Numerical: salary_range_from, salary_range_to

Date-like: posting_date, post_until, posting_updated

Profiling performed:

Row counts

Null counts per column

Schema inspection

Distinct value checks for categorical columns

Salary distribution overview

Top categories and postings

3. Data Processing
Cleaning

Drop records missing critical fields (job_id, agency, job_category)

Cast salary columns to numeric

Standardize column names

Handle nulls for salary and categorical fields

Feature Engineering

avg_salary = (salary_range_from + salary_range_to) / 2

salary_band = LOW / MID / HIGH

skill column exploded from preferred_skills

has_masters flag derived from business_title

process_year extracted from process_date

Additional derived columns for analytics convenience (e.g., salary per agency last 2 years)

Feature Removal

Columns not used in analytics (contacts, addresses) excluded from curated output

Columns with high null rates removed

4. KPIs Implemented

Top 10 job postings per category

Salary distribution per job category

Correlation between higher degree and salary

Highest salary per agency

Average salary per agency (last 2 years)

Highest paid skills in the US market

5. Storage

Curated dataset written as CSV and Parquet:

CSV: /data_engineering_takehome1/jupyter/output/

Parquet (optional for downstream analytics): data/curated/nyc_jobs_curated

Files are created in the mounted Docker volume, corresponding to the project folder on the host machine.

6. Testing

Unit tests implemented for transformation functions

Mock DataFrames used to validate logic

Assertions validate expected outputs

Edge cases such as missing salary, empty skills, and duplicate rows handled

7. Deployment Proposal

Option 1: Docker

Build image with Spark + Python

Run containerized pipeline with Spark master and workers

Mount dataset and code volumes for portability

Option 2: Cloud

Upload code to Databricks or EMR

Schedule job using native scheduler

8. Triggering Approach

Manual trigger: python main.py

Automated:

Cron job

Airflow DAG

Cloud scheduler

9. Challenges

Initial repo cloning issue due to technical glitch

Date parsing inconsistencies

Null handling in salary fields

Exploding multi-valued skills

Handling missing or inconsistent job categories

10. Assumptions

Salary ranges represent annual values unless specified otherwise

Preferred skills are comma-separated strings

posting_date field reflects job posting creation date

Missing or null salary treated as 0 for KPI calculations

Job Category null values labeled as Unknown for analysis

11. Learnings

PySpark aggregation optimization

Feature engineering at scale

Designing modular data pipelines

Writing testable Spark code

Dockerizing Spark workloads for reproducible environments

Handling real-world messy datasets efficiently
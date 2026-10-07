# Panda-Industry-Application

Data manipulation and cleaning are critical foundational steps in data science and analytics. Raw enterprise datasets frequently contain structural anomalies, missing entries, malformed text symbols, and duplicate rows. Utilizing Pandas and NumPy, this project demonstrates robust data exploration, advanced data cleaning pipelines, and conditional feature engineering across four distinct real-world industry domains: E-Commerce, Healthcare, Banking, and Energy Utilities.
---

## Problem Statement

Real-world operational datasets are rarely clean or ready for immediate consumption. Analysts frequently encounter critical data quality issues across different sectors:

* **E-Commerce:** Managing large customer files to identify high-value customer tiers without manual overhead.
* **Healthcare:** Handling missing patient ages, correcting string-formatted visit counts (e.g., 'six', '4 visits'), and resolving inconsistent city string formatting (thatis; 'lagos', 'ABUJA', 'Port-Harcourt').
* **Banking:** Cleaning financial transaction columns corrupted by currency symbols (₦), commas, typographical errors (e.g., '1O000' with the letter 'O'), missing amounts, and text artifacts like '##VALUE!' or 'Not Available'.
* **Energy Utilities:** Processing multi-attribute consumer energy records featuring missing city locations, unformatted string consumption logs, and inconsistent utility source labels.
---

## Solution
To resolve these challenges, the project implements comprehensive Pandas dataframes and NumPy vectorized conditional operations to inspect structural integrity, drop duplicates, coerce unformatted strings into proper numeric types, impute missing values using median/default strategies, and generate segmented summary tables for strategic business decisions.

---

## Importance
* **Data Reliability:** Ensures analytical models and business intelligence dashboards are built on clean, verified datasets.
* **Automated Data Cleaning Pipelines:** Replaces tedious manual spreadsheet corrections with reproducible, programmatic transformation scripts.
* **Decision Intelligence:** Unlocks targeted customer segments across retail, health, finance, and energy sectors to optimize resource allocation and revenue generation.

---

## Feature
* **Structural Inspection:** Rapid loading and overview verification using .head(), .shape, .columns, .dtypes, and .info().
* **Advanced Data Cleaning:** Stripping trailing whitespaces, standardizing capitalization via .str.title(), handling type coercion errors (errors='coerce'), and mapping text words to numbers.
* **Missing Value and Duplicate Management:** Detecting null values via .isna().sum(), filtering duplicate entries, and applying strategic imputation.
* **Conditional Feature Engineering:** Creating new analytical categories using np.where() based on numerical thresholds (e.g., Sales $\ge$ ₦100,000, Visit Count $\ge$ 5, Transaction Amount $\ge$ ₦100,000, or Monthly Consumption $\ge$ 500 kWh).
* **Aggregations and Groupby Analysis:** Summarizing data distributions across cities, transaction types, customer types, and patient categories using .value_counts() and .groupby().

---

## Python Workflow

* **Environment Setup:** Import numpy and pandas libraries.
* **Data Ingestion:** Load raw CSV files into individual Pandas DataFrames (df, df_1, df_2, df_3).
* **Exploratory Data Analysis (EDA):** Inspect dimensions, data types, summary statistics, and missing value distributions.
* **Data Cleaning and Preprocessing:** Clean corrupted text rows, convert columns to numeric formats, handle duplicates, and fill null fields.
* **Feature Engineering and Aggregation:** Construct conditional classification columns and execute grouped statistical
* **Summarisation**: summaries to yield actionable business insights.

---

## Project Walkthrough and Code Snippets by Sector
* **E-Commerce Customer Analysis**: Ingests a 5,000-row customer sales record, verifies data integrity, and segments shoppers based on purchasing volume.

**Code Snippet:**
import numpy as np
import pandas as pd

df = pd.read_csv(r'\Users\oseni\Downloads\customer_sales - customer_sales.csv')

# Condition-based segmentation for high-value shoppers
df['Customer Value'] = np.where(df['Sales'] >= 100000, "High value", "Standard")
high_value_customers = df['Customer Value'].value_counts().max()
print(f"Number of High Value Customers is: {high_value_customers}")

# Pandas Industry Application: E-Commerce, Healthcare, Banking, and Energy Data Analysis

## Introduction
Data manipulation and cleaning are critical foundational steps in data science and analytics. Raw enterprise datasets frequently contain structural anomalies, missing entries, malformed text symbols, and duplicate rows. Utilizing **Pandas** and **NumPy**, this project demonstrates robust data exploration, advanced data cleaning pipelines, and conditional feature engineering across four distinct real-world industry domains: E-Commerce, Healthcare, Banking, and Energy Utilities.

## Problem Statement
Real-world operational datasets are rarely clean or ready for immediate consumption. Analysts frequently encounter critical data quality issues across different sectors:

* **E-Commerce:** Managing large customer files to identify high-value customer tiers without manual overhead.
* **Healthcare:** Handling missing patient ages, correcting string-formatted visit counts (e.g., `'six'`, `'4 visits'`), and resolving inconsistent city string formatting (e.g., `'lagos'`, `'ABUJA'`, `'Port-Harcourt'`).
* **Banking:** Cleaning financial transaction columns corrupted by currency symbols (`₦`), commas, typographical errors (e.g., `'1O000'` with the letter 'O'), missing amounts, and text artifacts like `'##VALUE!'` or `'Not Available'`.
* **Energy Utilities:** Processing multi-attribute consumer energy records featuring missing city locations, unformatted string consumption logs, and inconsistent utility source labels.

## Solution
To resolve these challenges, the project implements comprehensive Pandas dataframes and NumPy vectorized conditional operations to inspect structural integrity, drop duplicates, coerce unformatted strings into proper numeric types, impute missing values using median/default strategies, and generate segmented summary tables for strategic business decisions.

## Importance
* **Data Reliability:** Ensures analytical models and business intelligence dashboards are built on clean, verified datasets.
* **Automated Data Cleaning Pipelines:** Replaces tedious manual spreadsheet corrections with reproducible, programmatic transformation scripts.
* **Decision Intelligence:** Unlocks targeted customer segments across retail, health, finance, and energy sectors to optimize resource allocation and revenue generation.

## Feature
* **Structural Inspection:** Rapid loading and overview verification using `.head()`, `.shape`, `.columns`, `.dtypes`, and `.info()`.
* **Advanced Data Cleaning:** Stripping trailing whitespaces, standardizing capitalization via `.str.title()`, handling type coercion errors (`errors='coerce'`), and mapping text words to numbers.
* **Missing Value & Duplicate Management:** Detecting null values via `.isna().sum()`, filtering duplicate entries, and applying strategic imputation.
* **Conditional Feature Engineering:** Creating new analytical categories using `np.where()` based on numerical thresholds (e.g., Sales $\ge$ ₦100,000, Visit Count $\ge$ 5, Transaction Amount $\ge$ ₦100,000, or Monthly Consumption $\ge$ 500 kWh).
* **Aggregations & Groupby Analysis:** Summarizing data distributions across cities, transaction types, customer types, and patient categories using `.value_counts()` and `.groupby()`.

## Python Workflow
1. **Environment Setup:** Import `numpy` and `pandas` libraries.
2. **Data Ingestion:** Load raw CSV files into individual Pandas DataFrames (`df`, `df_1`, `df_2`, `df_3`).
3. **Exploratory Data Analysis (EDA):** Inspect dimensions, data types, summary statistics, and missing value distributions.
4. **Data Cleaning & Preprocessing:** Clean corrupted text rows, convert columns to numeric formats, handle duplicates, and fill null fields.
5. **Feature Engineering & Aggregation:** Construct conditional classification columns and execute grouped statistical summaries to yield actionable business insights.

## Project Walkthrough

### 1. E-Commerce Customer Analysis
Ingests a 5,000-row customer sales record, verifies data integrity, and segments shoppers based on purchasing volume.
```python
import numpy as np
import pandas as pd

df = pd.read_csv(r'\Users\oseni\Downloads\customer_sales - customer_sales.csv')

# Condition-based segmentation for high-value shoppers
df['Customer Value'] = np.where(df['Sales'] >= 100000, "High value", "Standard")
high_value_customers = df['Customer Value'].value_counts().max()
print(f"Number of High Value Customers is: {high_value_customers}")
```
### 2. Hospital Patient Data Cleaning
Processes a 3,000-row patient dataset, rectifies float-formatted age columns using median imputation, standardizes messy city names, and converts string-based visit counts into proper integers.
```
df_1 = pd.read_csv(r'\Users\oseni\Downloads\hospital_patient_data - hospital_patient_data_cleaning_practice.csv')

# Clean and impute Age
df_1['Age'] = pd.to_numeric(df_1['Age'], errors='coerce')
median_age = df_1['Age'].median()
df_1['Age'] = df_1['Age'].fillna(median_age).astype(int)

# Clean Visit_Count strings to numeric integers
df_1['Visit_Count'] = df_1['Visit_Count'].astype(str).str.replace('visits', '', case=False).str.strip()
word_to_num = {'six': '6', 'seven': '7', 'ten': '10'}
df_1['Visit_Count'] = df_1['Visit_Count'].str.lower().replace(word_to_num)
df_1['Visit_Count'] = pd.to_numeric(df_1['Visit_Count'], errors='coerce').fillna(0).astype(int)
```
### 3. Banking Customer Transaction Analysis
Inspects 3,000 banking transaction logs, strips currency symbols and typographic errors, handles missing values, and categorizes high-value transfers.
```
df_2 = pd.read_csv(r'\Users\oseni\Downloads\Banking_Customer_Transation - Banking_Customer_Transation.csv')

# Clean corrupted Amount column
df_2['Amount'] = df_2['Amount'].str.replace('₦', '', regex=False)
df_2['Amount'] = df_2['Amount'].str.replace(',', '', regex=False)
df_2['Amount'] = df_2['Amount'].str.replace('O', '0', regex=False)
df_2['Amount'] = pd.to_numeric(df_2['Amount'], errors='coerce')

# Create Large Transaction indicator
df_2['Transaction_Category'] = np.where(df_2['Amount'] >= 100000, "Large Transaction", "Regular Transaction")
```
### 4. Energy Consumption Analysis
Analyzes 3,012 utility records, eliminates duplicate logs, cleans unformatted monthly consumption string entries, handles missing structural attributes, and groups consumption by customer type.
```
df_3 = pd.read_csv(r'\Users\oseni\Downloads\Energy_Consumption - Energy_Consumption.csv')

# Remove duplicates and clean Monthly Consumption string symbols
df_3 = df_3.drop_duplicates()
df_3['Monthly Consumption'] = df_3['Monthly Consumption'].str.replace(',', '', regex=False)
df_3['Monthly Consumption'] = df_3['Monthly Consumption'].str.replace('O', '0', regex=False)
df_3['Monthly Consumption'] = pd.to_numeric(df_3['Monthly Consumption'], errors='coerce')

# Categorize consumption tiers and create summary groupby
df_3['Consumption_Category'] = np.where(df_3['Monthly Consumption'] >= 500, "High Consumption", "Low Consumption")
avg_consumption_by_type = df_3.groupby('Customer Type')['Monthly Consumption'].mean()
print(avg_consumption_by_type.round(3))
```
## Screenshot
![Description of your screenshot](panda_1.png)
![Description of your screenshot](panda_2.png)
![Description of your screenshot](panda_3.png)
![Description of your screenshot](panda_4.png)
![Description of your screenshot](panda_5.png)
![Description of your screenshot](panda_6.png)
![Description of your screenshot](panda_7.png)
![Description of your screenshot](panda_8.png)
![Description of your screenshot](panda_9.png)
![Description of your screenshot](panda_10.png)
![Description of your screenshot](panda_11.png)
![Description of your screenshot](panda_12.png)
![Description of your screenshot](panda_13.png)
![Description of your screenshot](panda_14.png)
![Description of your screenshot](panda_15.png)
![Description of your screenshot](panda_16.png)
![Description of your screenshot](panda_17.png)
![Description of your screenshot](panda_18.png)
![Description of your screenshot](panda_19.png)
![Description of your screenshot](panda_20.png)
![Description of your screenshot](panda_21.png)
![Description of your screenshot](panda_22.png)

## Results
* **E-Commerce:** Verified a clean 5,000-row dataset and successfully classified 2,983 customers into the high-value purchasing tier (Sales $\ge$ ₦100,000).
* **Healthcare:** Cleaned patient records, resolved string-to-integer anomalies in visit counts, and structured summary metrics showing that 1,986 frequent patients drive the majority of hospital traffic (averaging 8.5 visits).
* **Banking:** Processed banking transactions, successfully filtering out text errors and currency artifacts to uncover that Benin City holds the highest average transaction amount among all regions.
* **Energy Utilities:** Cleaned utility data records, identifying that industrial consumers drive an average consumption footprint of 6,464.33 kWh per month

---

## Findings
* **E-Commerce:** More than 59% of shoppers generate individual sales values exceeding ₦100,000, presenting a lucrative core audience for loyalty engagement.
* **Healthcare:** Hospital workloads are heavily skewed; frequent patients make up roughly two-thirds of active visits, demanding optimized scheduling structures to avoid facility overcrowding.
* **Banking:** Regional financial performance varies significantly, with Benin City emerging as the most valuable region for generating high-revenue transaction amounts per customer.
* **Energy Utilities:** Industrial accounts represent the smallest customer volume group yet drive the highest average monthly consumption, indicating that grid forecasting must prioritize heavy industrial load centers.

---

## Improvement
* **Automation Functions:** Wrap repetitive data cleaning and type coercion tasks into reusable Python functions to streamline multi-file enterprise pipelines.
* **Advanced Visualization:** Incorporate Matplotlib and Seaborn to plot regional transaction distributions, hospital visit frequencies, and utility consumption tiers graphically.
* **Database Integration:** Connect scripts directly to SQL databases or cloud storage buckets for live, automated data hygiene monitoring.

## Author
**OSENI, Latifat Omolara**

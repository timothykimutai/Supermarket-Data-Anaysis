# Retail Sales Data Analysis

## Project Overview
This project involves an end-to-end analysis of retail sales data, focusing on branch performance, customer demographics, product lines, and profitability metrics. The goal is to extract actionable insights on revenue, customer satisfaction, and gross income to optimize profit margins and inform strategic decisions.

## Problem Statement
The retail company seeks to analyze sales data across different branches and cities to gain insights into customer preferences, revenue by product line, and profitability metrics. Key objectives include:
1. Understanding revenue by branch and product line.
2. Assessing customer satisfaction through ratings.
3. Analyzing gross income by demographics to optimize profit margins.

## Data Columns Used
The analysis is based on the following columns:
- **Branch**: Store branch information.
- **City**: Location of each branch.
- **Gender**: Customer demographic information.
- **Product_line**: Product category.
- **Unit_price**, **Tax_5**, **Sales**, **cogs**, **gross_margin_percentage**, **gross_income**: Financial metrics for profitability analysis.
- **Date**: Transaction date.
- **Rating**: Customer satisfaction measure.

---

## Steps for Analysis

### Step 1: Data Extraction

**Objective**: Extract relevant fields for analysis using SQL.

#### SQL Query
```sql
SELECT 
    Branch,
    City,
    Gender,
    Product_line,
    Unit_price,
    Tax_5,
    Sales,
    Date,
    cogs,
    gross_margin_percentage,
    gross_income,
    Rating
FROM 
    sales_data
WHERE 
    Date >= '2023-01-01';
```

**Explanation**:
- Filters data from 2023 onwards to focus on recent transactions.
- Selects key columns for branch details, demographics, product line, sales metrics, and ratings.

### Step 2: Data Processing and Analysis in Python

#### Step 2.1: Load Data
Load the extracted data (assumed saved as `sales_data.csv`) into a pandas DataFrame.

```python
import pandas as pd

df = pd.read_csv('sales_data.csv')
```

#### Step 2.2: Data Cleaning

1. **Check for Missing Values**:
   ```python
   print(df.isnull().sum())
   ```
   This identifies any missing values, crucial for ensuring data integrity.

2. **Handle Missing Values**:
   ```python
   df = df.dropna()
   ```
   For simplicity, rows with missing values are dropped; however, imputation methods may be used in practice.

#### Step 2.3: Data Transformation

1. **Convert Date Column to DateTime**:
   ```python
   df['Date'] = pd.to_datetime(df['Date'])
   ```
   Converting `Date` allows for time-based operations.

2. **Extract Year and Month**:
   ```python
   df['year'] = df['Date'].dt.year
   df['month'] = df['Date'].dt.month
   ```
   This transformation facilitates monthly and yearly aggregations.

#### Step 2.4: Exploratory Data Analysis

1. **Monthly Sales by Branch**:
   Calculate total monthly sales per branch to analyze location-based revenue trends.
   ```python
   monthly_sales_branch = df.groupby(['Branch', 'year', 'month'])['Sales'].sum().reset_index()
   ```

2. **Average Rating by Product Line**:
   Analyze customer satisfaction by calculating the average rating per product line.
   ```python
   avg_rating_product_line = df.groupby('Product_line')['Rating'].mean().reset_index()
   ```

3. **Gross Income by City and Gender**:
   Evaluate profitability by customer demographics (city and gender).
   ```python
   gross_income_city_gender = df.groupby(['City', 'Gender'])['gross_income'].sum().reset_index()
   ```

4. **Gross Margin by Product Line**:
   Determine average gross margin percentage for each product line to assess profitability.
   ```python
   gross_margin_product_line = df.groupby('Product_line')['gross_margin_percentage'].mean().reset_index()
   ```

#### Step 2.5: Saving Analysis Results
Export the processed data for visualization in Power BI.

```python
monthly_sales_branch.to_csv('monthly_sales_branch.csv', index=False)
avg_rating_product_line.to_csv('avg_rating_product_line.csv', index=False)
gross_income_city_gender.to_csv('gross_income_city_gender.csv', index=False)
gross_margin_product_line.to_csv('gross_margin_product_line.csv', index=False)
```

---

### Step 3: Data Visualization in Power BI

**Objective**: Visualize insights from the processed data.

#### Steps:

1. **Import Data**: Load the CSV files generated in Step 2.5 into Power BI.

2. **Visualizations**:

   - **Monthly Sales by Branch**: A line chart visualizing sales trends across branches by month and year.
   - **Average Rating by Product Line**: A bar chart showing customer satisfaction by product line.
   - **Gross Income by City and Gender**: A stacked bar chart representing gross income segmented by gender for each city.
   - **Gross Margin by Product Line**: A clustered bar chart comparing gross margin percentages by product line.

3. **Filters and Slicers**: 
   Add filters for `year`, `month`, and `Branch` to interactively explore data by time period and location.

### Insights:
1. **Branch Performance**: High-performing branches can be identified, aiding in targeted investment.
2. **Customer Satisfaction**: Product lines with higher customer ratings highlight areas of satisfaction.
3. **Demographic Profitability**: Gross income by city and gender helps pinpoint valuable customer segments.
4. **Product Profitability**: Gross margin analysis by product line informs decisions for optimizing profit margins.

---

## Summary

This project provides a comprehensive analysis of retail sales data, utilizing SQL for extraction, Python for data processing, and Power BI for visualization. Each stage contributes to a holistic understanding of branch performance, customer satisfaction, and profitability, enabling data-driven decisions for the retail company.

--- 

This README serves as a guide to the data analysis process and includes detailed steps to replicate each stage.


---

# Strategic Marketing for Personal Loans

## Project Overview

This project aims to empower banks with data-driven insights for strategic marketing of personal loans. By analyzing customer data, we can identify high-potential customers, uncover financial trends, and profile customer demographics. The project is divided into several tasks, each focusing on a specific aspect of data processing and analysis.

## Table of Contents

1. [Introduction](#introduction)
2. [Data Cleaning and Preparation](#data-cleaning-and-preparation)
3. [Data Transformation](#data-transformation)
4. [Data Analysis](#data-analysis)
5. [Conclusion](#conclusion)
6. [Usage](#usage)
7. [Acknowledgements](#acknowledgements)

## Introduction

In the world of modern banking, where competition is fierce, leveraging data to make informed decisions is crucial. This project uses Python and SQL to analyze personal loan data, with the ultimate goal of providing actionable insights for strategic marketing.

## Data Cleaning and Preparation

### Task 1: Unlocking Banking Potential
We start by importing the necessary libraries and reading the dataset.

```python
import pandas as pd
df = pd.read_csv('Bank_Personal_Loan_Modelling.csv')
```

### Task 2: The Quest for Accuracy
Identify and handle duplicate records to ensure data accuracy.

```python
duplicates = df.duplicated().sum()
```

### Task 3: Chasing Data Perfection
Identify and handle missing values to ensure data completeness.

```python
null_values = df.isnull().sum()
```

### Task 4: Streamlining Data for Precision
Drop unnecessary columns to streamline the dataset.

```python
df.drop(columns=['ID', 'ZIP Code'], inplace=True)
```

### Task 5: Removing Negative Experience
Filter out records with negative values in the Experience column.

```python
condition_mask = df['Experience'] >= 0
filtered_df = df[condition_mask]
```

## Data Transformation

### Task 1: Transforming Education Levels
Convert numeric education levels into meaningful categories.

```python
def edu(x):
    if x == 1:
        return "Undergraduate"
    elif x == 2:
        return "Graduate"
    elif x == 3:
        return "Advanced/Professional"
df['Education'] = df['Education'].apply(edu)
```

### Task 2: Categorizing Account Holders
Categorize account holders based on their securities and CD account status.

```python
def security(y):
    if (y['Securities Account'] == 1) & (y['CD Account'] == 1):
        return "Holds Both Accounts"
    elif (y['Securities Account'] == 1):
        return "Holds Securities Account"
    elif (y['CD Account'] == 1):
        return "Holds CD Account"
    else:
        return "No Accounts"
df['Account Category'] = df.apply(security, axis=1)
```

### Task 3: Archiving the Transformed Data
Save the cleaned and transformed data for further analysis.

```python
df.to_csv('liability.csv', index=False)
```

## Data Analysis

### Task 1: Identifying High-Potential Customers
Identify the top 10 customers based on income.

```sql
SELECT * FROM liability ORDER BY Income DESC LIMIT 10;
```

### Task 2: Uncovering Educational Financial Trends
Analyze average income based on education levels.

```sql
SELECT Education, AVG(Income) FROM liability GROUP BY Education;
```

### Task 3: Top Income Earners by Education
Rank income earners within each education level.

```sql
WITH RankedData AS (
    SELECT *, RANK() OVER (PARTITION BY Education ORDER BY Income DESC) AS Rank
    FROM liability
)
SELECT * FROM RankedData WHERE Rank = 1;
```

### Task 4: Profiling Customer Demographics
Classify customers into age groups and analyze their credit card spending.

```sql
SELECT CASE
    WHEN Age BETWEEN 18 AND 30 THEN '18-30'
    WHEN Age BETWEEN 31 AND 45 THEN '31-45'
    WHEN Age BETWEEN 46 AND 60 THEN '46-60'
    ELSE '60+'
END AS AgeGroup, AVG(CCAvg) AS AvgCreditCardSpending
FROM liability
GROUP BY AgeGroup;
```

### Task 5: Analyzing Age vs. Credit Card Spending
Calculate the average age of customers with above-average credit card spending.

```sql
SELECT AVG(Age) AS AverageAge FROM liability WHERE CCAvg > (SELECT AVG(CCAvg) FROM liability);
```

### Task 6: Unveiling High-Income Elite
Identify customers with income significantly higher than average.

```sql
SELECT * FROM liability WHERE Income > 1.5 * (SELECT AVG(Income) FROM liability);
```

### Task 7: Family Dynamics Analysis
Analyze the youngest age of customers within each family size.

```sql
SELECT Family, MIN(Age) AS YoungestAge FROM liability GROUP BY Family;
```

### Task 8: Mortgage Holders
Identify customers who have mortgages.

```sql
SELECT * FROM liability WHERE Mortgage > 0;
```

### Task 9: Understanding Customer Distribution
Count the number of customers within each education level.

```sql
SELECT Education, COUNT(*) AS CustomerCount FROM liability GROUP BY Education;
```

## Conclusion

This project demonstrates the power of data analysis in strategic marketing for personal loans. By cleaning, transforming, and analyzing the data, we can uncover valuable insights that can help banks tailor their marketing strategies to target high-potential customers effectively.

## Usage

To run this project, follow these steps:

1. Clone the repository.
2. Install the necessary dependencies.
3. Run the Jupyter notebook to execute the code and perform the analysis.

## Acknowledgements

This project was created using Python and SQL for data analysis. Special thanks to the contributors of the libraries and tools used in this project.

---

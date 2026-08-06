<img width="1983" height="793" alt="Panoramico GitHub" src="https://github.com/user-attachments/assets/d26d80ab-5163-441f-bdb4-0d83c2e34dff" />


# Tech Layoffs Analysis 2020–2023

## 1. Introduction

The technology sector experienced significant workforce reductions between 2020 and 2023. This project analyzes global layoff data to understand how workforce reductions varied across companies, industries, countries, and time periods.

The analysis aims to identify relevant workforce trends that can support benchmarking and provide insights for strategic planning and Human Resources (HR) analysis.

---

## 2. Business Problem

Organizations need to understand how workforce reductions have evolved across industries and markets in order to identify trends, compare companies, and better understand changes in the technology sector.

This project seeks to answer the following questions:

- Which industries experienced the highest number of layoffs?
- During which periods were layoffs highest?
- Which countries recorded the highest number of layoffs?
- Which companies experienced the largest workforce reductions?
- Which companies had raised the most funding in the dataset?

### Objective

Analyze global technology layoffs from 2020 to 2023 by company, industry, country, and time period to identify key workforce trends and provide insights that can support strategic and workforce planning.

---

## 3. Dataset

This project uses a layoffs dataset published by Alex The Analyst on GitHub for educational and portfolio development purposes.

The dataset contains the following fields:

- `company`
- `location`
- `industry`
- `total_laid_off`
- `percentage_laid_off`
- `date`
- `stage`
- `country`
- `funds_raised_millions`

The dataset covers layoffs reported between 2020 and 2023.

**Dataset:** [https://github.com/AlexTheAnalyst/MySQL-YouTube-Series/blob/main/layoffs.csv]

### Tools

- Microsoft Excel
- MySQL
- Power BI

---

## 4. Data Cleaning

The data cleaning process was performed using SQL.

### Cleaning Steps

1. Create a staging table
2. Identify duplicate records
3. Remove duplicate records
4. Validate the cleaned dataset

### Staging Table

A staging table was created to preserve the original dataset and perform cleaning operations safely.

```sql
CREATE TABLE layoffs_staging 
LIKE layoffs;

INSERT layoffs_staging
SELECT *
FROM layoffs;
```

###  Duplicate Detection
ROW_NUMBER() and PARTITION BY were used to identify duplicate records based on company, location, industry, layoffs, percentage, date, stage, country, and funding.


```sql
WITH duplicate_cte AS
(
    SELECT *,
        ROW_NUMBER() OVER(
            PARTITION BY company, location, industry,
            total_laid_off, percentage_laid_off,
            `date`, stage, country,
            funds_raised_millions
        ) AS row_num
    FROM layoffs_staging
)
SELECT *
FROM duplicate_cte
WHERE row_num > 1;
```
<img width="2401" height="1350" alt="01" src="https://github.com/user-attachments/assets/5673e7f3-cbb6-4ad8-84c2-b16ba7e68850" />

###  Duplicate Removal
Duplicate records were removed while retaining the first occurrence of each duplicated group.

```sql
DELETE
FROM layoffs_staging2
WHERE row_num > 1;
```

##  5. SQL Analysis
After cleaning the dataset, SQL was used to perform exploratory analysis and identify key patterns.

### Total Layoffs
The maximum number of layoffs recorded in a single company entry was 12000, while the maximum value of percentage_laid_off was 100%.
<img width="2401" height="1350" alt="02" src="https://github.com/user-attachments/assets/a84aecc7-15b9-4e18-b044-2484f389b492" />


### Layoffs by Industry
Question: Which industries experienced the highest number of layoffs?
The industries with the highest number of layoffs included:
Consumer
Retail
Other

```sql
SELECT industry, SUM(total_laid_off)
FROM layoffs_staging2
GROUP BY industry
ORDER BY 2 DESC;
```
<img width="2401" height="1350" alt="03" src="https://github.com/user-attachments/assets/319eeee2-aaf1-4e8e-8331-bbd130d511f3" />



### Layoffs by Company
Question: Which companies experienced the largest workforce reductions?
The companies with the highest total layoffs included:

```sql
SELECT company, SUM(total_laid_off)
FROM layoffs_staging2
GROUP BY company
ORDER BY 2 DESC;
```
<img width="2401" height="1350" alt="04" src="https://github.com/user-attachments/assets/10e3f539-6133-49f8-9c7d-c0bedfecdd93" />


### Top 5 Companies by Year
CTEs and DENSE_RANK() were used to identify the top five companies by layoffs for each year.

```sql
DENSE_RANK() OVER (
    PARTITION BY years
    ORDER BY total_laid_off DESC
) AS Ranking
```

<img width="2401" height="1350" alt="05" src="https://github.com/user-attachments/assets/ccfc7301-bc9b-4162-a3ff-1fd8cec71129" />


### Layoffs by Country
Question: Which countries recorded the highest number of layoffs?
The countries with the highest number of layoffs included:
United States
India
Netherlands

```sql
SELECT country, SUM(total_laid_off)
FROM layoffs_staging2
GROUP BY country
ORDER BY 2 DESC;
```
<img width="2401" height="1350" alt="06" src="https://github.com/user-attachments/assets/9b1f57d9-2bc0-4d9a-a10e-44b294f5efc3" />


### Layoffs Over Time
Question: During which years were layoffs highest?

```sql
SELECT YEAR(`date`), SUM(total_laid_off)
FROM layoffs_staging2
GROUP BY YEAR(`date`)
ORDER BY 1 DESC;
```

The total layoffs by year were: 

<img width="2401" height="1350" alt="07" src="https://github.com/user-attachments/assets/20541a58-b0f3-4811-9a27-4575bb80f313" />


## 6. Power BI Dashboard
The cleaned and analyzed data was used to create an interactive Power BI dashboard.
The dashboard allows users to explore layoffs by:
-Year
-Industry
-Company
-Country
-Time period

<img width="2401" height="1350" alt="08" src="https://github.com/user-attachments/assets/053f700e-770a-42c1-987e-64d425f10e84" />


##  7. Key Insights

Consumer, Retail, and Other were among the industries with the highest number of layoffs during the 2020–2023 period.
Amazon recorded the highest total number of layoffs, with 18,150 employees affected, followed by Google with 12,000 and Meta with 11,000.

The companies with the highest number of layoffs varied significantly by year. In 2020, Uber ranked first, followed by Booking.com, Groupon, Swiggy, and Airbnb.

- In 2021, ByteDance ranked first, followed by Katerra, Zillow, Instacart, and White Hat Jr.
- In 2022, Meta ranked first, followed by Amazon, Cisco, Peloton, and a tie between Carvana and Philips.
- In 2023, Google ranked first, followed by Microsoft, Ericsson, Amazon, and a tie between Salesforce and Dell.

The companies most affected by layoffs changed from year to year, indicating that workforce reductions were not concentrated in a single company throughout the entire period.

## 8. Conclusion

The analysis shows that technology layoffs varied considerably across industries, companies, countries, and years between 2020 and 2023.

Consumer, Retail, and Other were among the industries with the highest number of layoffs, while companies such as Amazon, Google, and Meta recorded some of the largest workforce reductions in the dataset.

The yearly analysis also shows that the companies most affected changed over time, highlighting how workforce reductions varied across different periods and companies.

The combination of SQL and Power BI provided a structured approach to cleaning, analyzing, and visualizing the dataset. The final dashboard allows users to explore layoff trends interactively and compare workforce reductions across different companies, industries, countries, and time periods.


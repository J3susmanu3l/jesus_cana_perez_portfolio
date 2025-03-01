# Data Analyst Portfolio

Welcome to my portfolio! I am an aspiring data analyst in the early stages of my career, passionate about turning raw data into actionable insights. This repository features my projects, starting with my first SQL data cleaning project copied from Alex the Analyst's free YouTube videocourse.

---

## Table of Contents

- [Project Overview](#project-overview)
- [Data Cleaning Process](#data-cleaning-process)
- [SQL Scripts](#sql-scripts)
- [Project Conclusion](#project-conclusion)
- [Contact](#contact)

---

## Project Overview

**Project Name:** SQL Data Cleaning  
**Inspiration:** [Alex the Analyst - YouTube Free Videocourse](https://www.youtube.com/watch?v=OT1RErkfLNQ&t=12758s)  
**Description:**  
This project demonstrates the process of cleaning a dataset using SQL. The steps include:
- Removing duplicate records.
- Standardizing data (e.g., trimming spaces and correcting text formats).
- Handling null or blank values.
- Adjusting data types (e.g., converting date formats).

---

## Data Cleaning Process

1. **Remove Duplicates:**  
   - Create a staging table.
   - Use the `ROW_NUMBER()` window function to identify duplicate rows.
   - Delete duplicate records while retaining the original.

2. **Standardize Data:**  
   - Trim unnecessary spaces from text fields.
   - Correct inconsistencies in string formats (e.g., location names, industry names).
   - Convert date strings to a proper date format.

3. **Handle Null or Blank Values:**  
   - Identify rows with null or blank values.
   - Update or delete these records as appropriate to ensure data integrity.

---

## SQL Scripts

### 1. Creating and Populating the Staging Table

```sql
-- View original data
SELECT *
FROM layoffs;

-- Create a staging table similar to the original
CREATE TABLE layoffs_staging LIKE layoffs;

-- Confirm the staging table structure
SELECT *
FROM layoffs_staging;

-- Insert data into the staging table
INSERT INTO layoffs_staging
SELECT *
FROM layoffs;

-- Identify duplicates using row_number()
SELECT *, 
       ROW_NUMBER() OVER (
         PARTITION BY company, location, industry, total_laid_off, percentage_laid_off, `date`, country, funds_raised_millions
       ) AS row_num
FROM layoffs_staging;

-- Using a CTE to view duplicates
WITH duplicate_cte AS (
    SELECT *, 
           ROW_NUMBER() OVER (
             PARTITION BY company, location, industry, total_laid_off, percentage_laid_off, `date`, country, funds_raised_millions
           ) AS row_num
    FROM layoffs_staging
)
SELECT *
FROM duplicate_cte
WHERE row_num > 1;


CREATE TABLE layoffs_staging2 (
  company TEXT,
  location TEXT,
  industry TEXT,
  total_laid_off INT DEFAULT NULL,
  percentage_laid_off TEXT,
  `date` TEXT,
  stage TEXT,
  country TEXT,
  funds_raised_millions INT DEFAULT NULL,
  row_num INT
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_0900_ai_ci;

-- Insert data with row numbers
INSERT INTO layoffs_staging2
SELECT *, 
       ROW_NUMBER() OVER (
         PARTITION BY company, location, industry, total_laid_off, percentage_laid_off, `date`, country, funds_raised_millions
       ) AS row_num
FROM layoffs_staging;

-- Delete duplicate records (retain rows with row_num = 1)
DELETE FROM layoffs_staging2
WHERE row_num > 1;


-- Trim spaces from company names
UPDATE layoffs_staging2
SET company = TRIM(company);

-- Standardize industry names (e.g., changing variants to 'Crypto')
UPDATE layoffs_staging2
SET industry = 'Crypto'
WHERE industry LIKE 'Crypto%';

-- Correct location names
UPDATE layoffs_staging2
SET location = 'Düsseldorf'
WHERE location = 'DÃ¼sseldorf';

UPDATE layoffs_staging2
SET location = 'Florianópolis'
WHERE location = 'FlorianÃ³polis';

UPDATE layoffs_staging2
SET location = 'Malmö'
WHERE location = 'MalmÃ¶';

-- Convert date from string to date format and modify the column type
UPDATE layoffs_staging2
SET `date` = STR_TO_DATE(`date`, '%m/%d/%Y');

ALTER TABLE layoffs_staging2
MODIFY COLUMN `date` DATE;


-- Identify rows with null values in key fields
SELECT *
FROM layoffs_staging2
WHERE total_laid_off IS NULL
  AND percentage_laid_off IS NULL;

-- Identify rows with null or empty industry fields
SELECT *
FROM layoffs_staging2
WHERE industry IS NULL OR industry = '';

-- Update empty industry values to NULL
UPDATE layoffs_staging2
SET industry = NULL
WHERE industry = '';

-- Remove rows with missing values in numeric fields
DELETE FROM layoffs_staging2
WHERE total_laid_off IS NULL
  AND percentage_laid_off IS NULL;


-- Display the final cleaned data
SELECT *
FROM layoffs_staging2;

-- Remove the temporary row number column as it is no longer needed
ALTER TABLE layoffs_staging2
DROP COLUMN row_num;


Project Conclusion
This project demonstrates a systematic approach to data cleaning using SQL. By removing duplicates, standardizing data, and handling missing values, the dataset is now more consistent and reliable for further analysis. This project marks the beginning of my journey as a data analyst, and I look forward to tackling more complex data challenges.

Contact
Feel free to reach out with questions, feedback, or opportunities for collaboration:

Email: jesusmanuelcanaperes@gmail.com
LinkedIn: [Jesus Cana Perez ](https://www.linkedin.com/in/jesus-cana-perez-070098315/)

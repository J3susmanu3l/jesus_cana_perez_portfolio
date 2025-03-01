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


# 🏥 Healthcare SQL Case Study

This project is a collection of SQL practice exercises using a simulated healthcare database.  
The dataset includes the following tables:

- `patients`
- `admissions`
- `doctors`
- `province_names`

## 📌 Objective
To explore healthcare-related data using SQL and extract meaningful insights through queries.  
This case study focuses on analyzing patient admissions, doctor assignments, regional trends, and more.

## 📚 Question Source
All SQL challenges in this repository are inspired by questions from  
👉 [SQL Practice – Healthcare Case Study](https://www.sql-practice.com)

## 🛠️ Topics Covered
- Filtering and conditional queries  
- Aggregations and grouping  
- String and date manipulation  
- Joins across patient, doctor, and province tables  
- Window functions (e.g., LAG, RANK)  
- Subqueries and CTEs  
- Data cleaning and formatting  
- Real-world healthcare scenarios (e.g., admissions, diagnosis, patient management)

## 📈 Sample Questions

- **How many patients were admitted in each province?**  
  *Aggregation and grouping by location*

- **Show all columns from admissions where the patient was admitted and discharged on the same day.**  
  *Filtering based on date comparisons*

- **Update the patients table for the allergies column. If the patient's allergies is null then replace it with 'NKA'.**  
  *Data cleaning with update statements*

- **Show patient_id, diagnosis from admissions. Find patients admitted multiple times for the same diagnosis.**  
  *Detecting duplicates with grouping and filtering*

- **Display patient's full name, height in feet (rounded), weight in pounds (rounded), birth_date, and full gender description.**  
  *Data transformation and unit conversion*




# 🚑 Healthcare SQL Case Study

Practice your SQL skills with real healthcare data—think patients, admissions, and doctors. Perfect for getting comfortable with common SQL tasks and real-world scenarios.


### 📚 Topics Covered
- [Filtering and conditional queries](#-filtering-and-conditional-queries)
- [Aggregations and grouping](#-aggregations-and-grouping)
- Joins across multiple tables  
- Window functions and ranking  
- Subqueries and CTEs  
- Data cleaning and formatting  

## 📍 Filtering and conditional queries  

**❓ Show patient_id and first_name from patients where their first_name start and ends with 's' and is at least 6 characters long.**

- Looking for names that **start and end with 's'** → use `LIKE 's%s'`
- Filter to names that are **6 or more characters long** → `LEN() >= 6`
- Combines **string pattern matching** and a **basic length check**

````sql
SELECT
  patient_id,
  first_name
FROM patients
WHERE
  first_name LIKE 's%s'
  AND len(first_name) >= 6;
````

**❓ Show patient_id, weight, height, isObese from the patients table. Display isObese as a boolean 0 or 1. Obese is defined as weight(kg)/(height(m)2) >= 30. weight is in units kg. height is in units cm.**

- Calculate BMI using weight and height (height converted from cm to meters)  
- Check if BMI is 30 or higher to determine obesity → returns 1 if obese, else 0  
- Display patient ID, weight, height, and obesity status  

````sql
SELECT 
    patient_id, 
    weight, 
    height, 
    CASE 
        WHEN weight / POWER(height / 100.0, 2) >= 30 THEN 1
        ELSE 0
    END AS isObese
FROM patients;
````

## 📍 Aggregations and grouping

**❓Show the percent of patients that have 'M' as their gender. Round the answer to the nearest hundreth number and in percent form.**

- Count how many patients have gender = 'M' → `CASE WHEN gender = 'M' THEN 1 ELSE 0 END`
- Calculate the **percentage** of male patients → divide by total count and multiply by 100
- **Round** the result to 2 decimal places → `ROUND(..., 2)`
- **Convert** `(CAST)` the number to a string and append a '%' sign → `CONCAT(..., '%')`

````sql
SELECT 
    CONCAT(
        CAST(ROUND(SUM(CASE WHEN gender = 'M' THEN 1 ELSE 0 END) * 100.0 / COUNT(*), 2) AS VARCHAR),
        '%'
    ) AS percent_of_male_patients
FROM patients;
````

**❓ Each admission costs $50 for patients without insurance, and $10 for patients with insurance. All patients with an even patient_id have insurance.Give each patient a 'Yes' if they have insurance, and a 'No' if they don't have insurance. 
Add up the admission_total cost for each has_insurance group.**

- Determine if each `patient_id` is **even** → `% 2 = 0`  
- Even IDs are labeled `'Yes'` for having insurance, odd ones `'No'` → `CASE WHEN patient_id % 2 = 0 THEN 'Yes' ELSE 'No'`
- Assign **admission cost** based on insurance:
  - $10 if has insurance (even ID)
  - $50 if no insurance (odd ID)

````sql
SELECT 
    CASE 
        WHEN patient_id % 2 = 0 THEN 'Yes'
        ELSE 'No' 
    END AS has_insurance,
    SUM(
        CASE 
            WHEN patient_id % 2 = 0 THEN 10
            ELSE 50 
        END
    ) AS cost_after_insurance
FROM admissions 
GROUP BY has_insurance;
````

## 📍 Window Functions

**❓ For each day display the total amount of admissions on that day. Display the amount changed from the previous date.**

- Use `LAG()` to compare today’s count with the **previous day’s** → `LAG(count(*)) OVER (ORDER BY admission_date)`
- Calculate **daily change in admissions** → `count(*) - LAG(...)`

````sql
SELECT admission_date, 
     count(*) as admission_day,
     count(*) - LAG(count(*)) OVER (order by admission_date) as admission_count_change 
FROM admissions
GROUP BY admission_date;
````


**❓ Show all of the patients grouped into weight groups.
Show the total amount of patients in each weight group.
Order the list by the weight group decending.**

- Group patients by weight ranges of 10 (e.g., 50-59, 60-69) → `FLOOR(weight/10)*10`
- Count unique patients in each weight group → `COUNT(DISTINCT patient_id)`

````sql
SELECT COUNT(DISTINCT patient_id) AS patients_in_group, 
       FLOOR(weight/10)*10 AS weight_group
FROM patients
GROUP BY weight_group
ORDER BY weight_group DESC;
````

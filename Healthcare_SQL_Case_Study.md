# 🚑 Healthcare SQL Case Study

Practice your SQL skills with real healthcare data—think patients, admissions, and doctors. Perfect for getting comfortable with common SQL tasks and real-world scenarios.


### 📚 Topics Covered
- Filtering and conditional queries
- Aggregations and grouping
- Window functions and ranking  
- Joins and Subqueries
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


**❓ We are looking for a specific patient. Pull all columns for the patient who matches the following criteria: - First_name contains an 'r' after the first two letters.**
1. Identifies their gender as 'F'
2. Born in February, May, or December
3. Their weight would be between 60kg and 80kg
4. Their patient_id is an odd number
5. They are from the city 'Kingston'

- Filter conditions include:
  - `first_name` starts with any two characters followed by `'r'` → `LIKE '__r%'`
  - Birth month is February, May, or December → `MONTH(birth_date) IN (2, 5, 12)`
  - Patient ID is odd → `patient_id % 2 != 0`

````sql
SELECT *
FROM patients
WHERE first_name LIKE ('__r%')
  AND gender = 'F'
  AND MONTH(birth_date) IN (2, 5, 12)
  AND weight BETWEEN 60 AND 80
  AND patient_id % 2 != 0
  AND city = 'Kingston';
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


## 📍 Joins and Subqueries

**❓ We need a breakdown for the total amount of admissions each doctor has started each year. Show the doctor_id, doctor_full_name, specialty, year, total_admissions for that year.**


- Count total admissions per doctor per year  
  - Use `GROUP BY` on `attending_doctor_id` and `YEAR(admission_date)`  
- Format doctor information  
  - Combine `first_name` and `last_name` using `CONCAT()`  
  - Include `specialty` from the `doctors` table  

````sql
SELECT doc.*,
       ad.selected_year,
       ad.total_admissions
FROM
(
  SELECT attending_doctor_id,
         YEAR(admission_date) AS selected_year,
         COUNT(*) AS total_admissions
  FROM admissions
  GROUP BY selected_year, attending_doctor_id
) ad
JOIN
(
  SELECT doctor_id,
         CONCAT(first_name, ' ', last_name) AS doctor_name,
         specialty
  FROM doctors
) doc
ON ad.attending_doctor_id = doc.doctor_id;
````

**❓ Show the provinces that has more patients identified as 'M' than 'F'. Must only show full province_name.**

````sql
SELECT pro.province_name
FROM
(
  SELECT province_id,
         SUM(CASE WHEN gender = 'F' THEN 1 ELSE 0 END) AS f_count,
         SUM(CASE WHEN gender = 'M' THEN 1 ELSE 0 END) AS m_count
  FROM patients
  GROUP BY province_id
  HAVING m_count > f_count
) pat
LEFT JOIN province_names pro
  ON pat.province_id = pro.province_id;
````

**❓ All patients who have gone through admissions, can see their medical documents on our site. Those patients are given a temporary password after their first admission. Show the patient_id and temp_password.**

The password must be the following, in order:
1. patient_id
2. the numerical length of patient's last_name
3. year of patient's birth_date

````sql
SELECT 
    patient_id, 
    CONCAT(patient_id, LEN(last_name), YEAR(birth_date)) AS temp_password
FROM 
    patients
WHERE 
    patient_id IN (SELECT patient_id FROM admissions);
````

**❓ Show patient_id, first_name, last_name, and attending doctor's specialty.
Show only the patients who has a diagnosis as 'Epilepsy' and the doctor's first name is 'Lisa'
Check patients, admissions, and doctors tables for required information.**

````sql
SELECT
  p.patient_id,
  p.first_name AS patient_first_name,
  p.last_name AS patient_last_name,
  ph.specialty AS attending_doctor_specialty
FROM patients p
  JOIN admissions a ON a.patient_id = p.patient_id
  JOIN doctors ph ON ph.doctor_id = a.attending_doctor_id
WHERE
  ph.first_name = 'Lisa' and
  a.diagnosis = 'Epilepsy';
````


## 📍 Data cleaning and formatting

**❓ Sort the province names in ascending order in such a way that the province 'Ontario' is always on top.**

- Sort with a custom rule:
  - If `province_name` is `'Ontario'`, rank it first (assign 0)
  - All other provinces get ranked after (assign 1)

````sql
SELECT province_name
FROM province_names
ORDER BY 
  CASE WHEN province_name = 'Ontario' THEN 0 ELSE 1 END,
  province_name;
````

**❓ Display patient's full name,
height in the units feet rounded to 1 decimal,
weight in the unit pounds rounded to 0 decimals,
birth_date,
gender non abbreviated.**

````sql
SELECT 
    CONCAT(first_name, ' ', last_name) AS patient_name,
    ROUND(height / 30.48, 1) AS height,
    ROUND(weight * 2.205) AS weight,
    birth_date,
    CASE 
        WHEN gender = 'M' THEN 'MALE'
        WHEN gender = 'F' THEN 'FEMALE'
        ELSE gender
    END AS gender_type
FROM patients;

````

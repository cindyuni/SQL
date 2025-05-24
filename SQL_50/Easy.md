#### [197. Rising Temperature](https://leetcode.com/problems/rising-temperature/description/?envType=study-plan-v2&envId=top-sql-50)
Write a solution to find all dates id with higher temperatures compared to its previous dates (yesterday).

#### 💡 SQL Solution

```sql
SELECT id
FROM (
    SELECT id, 
            recordDate,
            LAG(recordDate) OVER (ORDER BY recordDate) as prev_date,
            temperature - LAG(temperature) OVER (ORDER BY recordDate) as temp_difference          
    FROM Weather
) as sub
WHERE temp_difference > 0
AND DATEDIFF(DAY, prev_date, recordDate) = 1;
```
⚡ This solution beats **80%** of other submissions' runtime.

---

#### [550. Game Play Analysis IV](https://leetcode.com/problems/game-play-analysis-iv/description/?envType=study-plan-v2&envId=top-sql-50)
Write a solution to report the **fraction** of players that logged in again on the day after the day they first logged in, **rounded to 2 decimal** places. In other words, you need to count the number of players that logged in for at least two consecutive days starting from their first login date, then divide that number by the total number of players.

#### 💡 SQL Solution

```sql
WITH CTE AS(
    SELECT *, MIN(event_date) OVER (PARTITION BY player_id) AS first_login_date
    FROM Activity
    )

SELECT ROUND(SUM(CASE WHEN event_date = DATEADD(day, 1, first_login_date) THEN 1 ELSE 0 END)*1.0 / COUNT(DISTINCT player_id), 2) as fraction
FROM CTE;
```
⚡ This solution beats **99.29%** of other submissions' runtime.

---
#### [577. Employee Bonus](https://leetcode.com/problems/employee-bonus/description/?envType=study-plan-v2&envId=top-sql-50)

Write a solution to report the name and bonus amount of each employee with a bonus **less than** `1000`.

#### 💡 SQL Solution

```sql
SELECT E.name, B.bonus
FROM Employee E
LEFT JOIN Bonus B
ON E.empId = B.empId
WHERE B.bonus < 1000 OR B.bonus is NULL;
```
⚡ This solution beats **65%** of other submissions' runtime.

---
#### [584. Find Customer Referee](https://leetcode.com/problems/find-customer-referee/description/?envType=study-plan-v2&envId=top-sql-50)
Find the names of the customer that are not referred by the customer with `id = 2`.

#### 💡 SQL Solution

```sql
SELECT name 
FROM customer
WHERE referee_id <> 2 
OR referee_id IS NULL;
```
⚡ This solution beats **87%** of other submissions' runtime.

---
#### [595. Big Countries](https://leetcode.com/problems/big-countries/description/?envType=study-plan-v2&envId=top-sql-50)
 
A country is big if:
- it has an area of at least three million (i.e., 3000000 km2), or
- it has a population of at least twenty-five million (i.e., 25000000).

Write a solution to find the name, population, and area of the big countries.

#### 💡 SQL Solution

```sql
SELECT name, population, area
FROM World
WHERE area >= 3000000
OR population >=  25000000;
```
⚡ This solution beats **60%** of other submissions' runtime.

---

#### [596. Classes More Than 5 Students](https://leetcode.com/problems/classes-more-than-5-students/description/?envType=study-plan-v2&envId=top-sql-50)
 
Write a solution to find all the classes that have **at least five students**.

#### 💡 SQL Solution

```sql
SELECT class
FROM Courses
GROUP BY class
HAVING COUNT(DISTINCT student) >= 5;
```
⚡ This solution beats **77.36%** of other submissions' runtime.

---

#### [620. Not Boring Movies](https://leetcode.com/problems/not-boring-movies/description/?envType=study-plan-v2&envId=top-sql-50)
 
Write a solution to report the movies with an **odd-numbered ID** and a **description that is not "boring"**.

Return the result table ordered by rating in **descending** order.

#### 💡 SQL Solution

```sql
SELECT id, movie, description, rating
FROM Cinema 
WHERE id%2 != 0 AND description != 'boring' 
ORDER BY rating DESC;
```
⚡ This solution beats **74%** of other submissions' runtime.


---
#### [1068. Product Sales Analysis I](https://leetcode.com/problems/product-sales-analysis-i/description/?envType=study-plan-v2&envId=top-sql-50)
 
Write a solution to report the product_name, year, and price for each sale_id in the Sales table.

#### 💡 SQL Solution

```sql
SELECT P.product_name, S.year, S.price
FROM Sales S JOIN Product P
ON S.product_id = P.product_id;
```
⚡ This solution beats **98.41%** of other submissions' runtime.

---

#### [1070. Product Sales Analysis III](https://leetcode.com/problems/product-sales-analysis-iii/description/?envType=study-plan-v2&envId=top-sql-50)
 
Write a solution to select the **product id, year, quantity, and price** for the **first year** of every product sold. If any product is bought multiple times in its first year, return all sales separately.

#### 💡 SQL Solution

```sql
WITH CTE AS (
    SELECT *, MIN(year) OVER (PARTITION BY product_id) AS first_year
    FROM Sales
)

SELECT product_id, first_year, quantity, price
FROM CTE 
WHERE year = first_year;
```
⚡ This solution beats **87.87%** of other submissions' runtime.

---

#### [1075. Project Employees I](https://leetcode.com/problems/project-employees-i/description/?envType=study-plan-v2&envId=top-sql-50)
 
Write an SQL query that reports the **average** experience years of all the employees for each project, **rounded to 2 digits**.

#### 💡 SQL Solution

```sql
SELECT P.project_id, ROUND(AVG(E.experience_years*1.0), 2) as average_years
FROM Project P
JOIN Employee E
ON P.employee_id = E.employee_id
GROUP BY project_id
```
⚡ This solution beats **82.45%** of other submissions' runtime.

---

#### [1141. User Activity for the Past 30 Days I](https://leetcode.com/problems/project-employees-i/description/?envType=study-plan-v2&envId=top-sql-50)
 
Write a solution to find the daily active user count for a period of `30 days` ending `2019-07-27` inclusively. A user was active on someday if they made at least one activity on that day.

#### 💡 SQL Solution

```sql
SELECT activity_date AS day, COUNT(DISTINCT user_id) AS active_users
FROM Activity
WHERE activity_date BETWEEN DATEADD(day, -29, CAST('2019-07-27' AS DATE)) and CAST('2019-07-27' AS DATE)
GROUP BY activity_date;
```
⚡ This solution beats **83.5%** of other submissions' runtime.

---

#### [1148. Article Views I](https://leetcode.com/problems/article-views-i/description/?envType=study-plan-v2&envId=top-sql-50)
Write a solution to find all the authors that viewed at least one of their own articles.
Return the result table sorted by id in ascending order.

#### 💡 SQL Solution

```sql
SELECT DISTINCT(author_id) AS id 
FROM Views 
WHERE author_id = viewer_id 
ORDER BY id;
````
⚡ This solution beats **55%** of other submissions' runtime.

---

#### [1174. Immediate Food Delivery II](https://leetcode.com/problems/immediate-food-delivery-ii/description/?envType=study-plan-v2&envId=top-sql-50)
If the customer's preferred delivery date is the same as the order date, then the order is called **immediate**; otherwise, it is called **scheduled**.

The **first order** of a customer is the order with the earliest order date that the customer made. It is guaranteed that a customer has precisely one first order.

Write a solution to find the percentage of immediate orders in the first orders of all customers, rounded to **2 decimal places**.

#### 💡 SQL Solution

```sql
WITH CTE AS (
    SELECT *,
        MIN(order_date) OVER (PARTITION BY customer_id) AS first_order_date
    FROM Delivery
)
SELECT 
    ROUND(
        SUM(CASE 
            WHEN order_date = first_order_date 
                 AND order_date = customer_pref_delivery_date
            THEN 1 ELSE 0 END) * 100.0 / COUNT(DISTINCT customer_id), 2
    ) AS immediate_percentage
FROM CTE;
````
⚡ This solution beats **89.72%** of other submissions' runtime.

---

#### [1193. Monthly Transactions I](https://leetcode.com/problems/monthly-transactions-i/description/?envType=study-plan-v2&envId=top-sql-50)
Write an SQL query to find for each month and country, the number of transactions and their total amount, the number of approved transactions and their total amount.

#### 💡 SQL Solution

```sql
SELECT FORMAT(trans_date, 'yyyy-MM') as month, 
       country, 
       COUNT(id) as trans_count, 
       SUM(case when state='approved' then 1 else 0 end) as approved_count, 
       SUM(amount) as trans_total_amount,  
       SUM(case when state='approved' then amount else 0 end) as approved_total_amount
FROM Transactions
GROUP BY FORMAT(trans_date, 'yyyy-MM'), country;
````
⚡ This solution beats **72.97%** of other submissions' runtime.

---
#### [1211. Queries Quality and Percentage](https://leetcode.com/problems/queries-quality-and-percentage/description/?envType=study-plan-v2&envId=top-sql-50)
We define query `quality` as: The average of the ratio between query rating and its position.

We also define `poor query percentage` as: The percentage of all queries with rating `less than 3`.

Write a solution to find each query_name, the quality and poor_query_percentage.
Both quality and poor_query_percentage should be rounded to **2 decimal places**.

#### 💡 SQL Solution

```sql
SELECT query_name, ROUND(AVG(rating*1.0/position), 2) AS quality, ROUND(SUM(CASE WHEN rating < 3 THEN 1 ELSE 0 END)*100.0/count(*), 2) AS poor_query_percentage
FROM Queries
GROUP BY query_name
````
⚡ This solution beats **83.02%** of other submissions' runtime.

---
#### [1251. Average Selling Price](https://leetcode.com/problems/average-selling-price/description/?envType=study-plan-v2&envId=top-sql-50)
Write a solution to find the average selling price for each product. average_price should be **rounded to 2 decimal places**. If a product does not have any sold units, its average selling price is assumed to be 0.

#### 💡 SQL Solution

```sql
SELECT 
    P.product_id, 
    ROUND(ISNULL(SUM(US.units * P.price) * 1.0 / ISNULL(SUM(US.units), 0), 0), 2) AS average_price
FROM 
    Prices P
LEFT JOIN 
    UnitsSold US
    ON P.product_id = US.product_id
    AND US.purchase_date BETWEEN P.start_date AND P.end_date
GROUP BY 
    P.product_id;
````
⚡ This solution beats **97.94%** of other submissions' runtime.

---

#### [1280. Students and Examinations](https://leetcode.com/problems/students-and-examinations/?envType=study-plan-v2&envId=top-sql-50)

Write a solution to find the number of times each student attended each exam.
Return the result table ordered by student_id and subject_name.

#### 💡 SQL Solution

```sql
SELECT DISTINCT(author_id) AS id 
FROM Views 
WHERE author_id = viewer_id 
ORDER BY id;
````
⚡ This solution beats **55%** of other submissions' runtime.

---

#### [1378. Replace Employee ID With The Unique Identifier](https://leetcode.com/problems/replace-employee-id-with-the-unique-identifier/description/?envType=study-plan-v2&envId=top-sql-50)
Write a solution to show the unique ID of each user, If a user does not have a **unique ID** replace just show `null`.

#### 💡 SQL Solution

```sql
SELECT EU.unique_id, E.NAME
FROM Employees E
LEFT JOIN EmployeeUNI EU
ON E.id = EU.id;
````
⚡ This solution beats **88%** of other submissions' runtime.

---

#### [1581. Customer Who Visited but Did Not Make Any Transactions](https://leetcode.com/problems/customer-who-visited-but-did-not-make-any-transactions/description/?envType=study-plan-v2&envId=top-sql-50)
Write a solution to find the IDs of the users who visited without making any transactions and the number of times they made these types of visits.


#### 💡 SQL Solution

```sql
select v.customer_id, count(*) as count_no_trans
from Visits v
left join Transactions t
on v.visit_id = t.visit_id
WHERE t.transaction_id is null
GROUP BY v.customer_id;
````
⚡ This solution beats **93.38%** of other submissions' runtime.

---
#### [1633. Percentage of Users Attended a Contest](https://leetcode.com/problems/percentage-of-users-attended-a-contest/description/?envType=study-plan-v2&envId=top-sql-50)

Write a solution to find the percentage of the users registered in each contest rounded to **two decimals**.

Return the result table ordered by **percentage in descending order**. In case of a tie, order it by **contest_id in ascending order**.


#### 💡 SQL Solution

```sql
SELECT contest_id, ROUND((count(user_id)*100.0/(select count(*) from Users)), 2) as percentage
FROM Register
GROUP BY contest_id
ORDER BY percentage DESC, contest_id ASC;
````
⚡ This solution beats **87.08%** of other submissions' runtime.

---

#### [1661. Average Time of Process per Machine](https://leetcode.com/problems/average-time-of-process-per-machine/description/?envType=study-plan-v2&envId=top-sql-50)
There is a factory website that has several machines each running the same number of processes. Write a solution to find the average time each machine takes to complete a process.

The time to complete a process is the `'end' timestamp` minus the `'start' timestamp`. The average time is calculated by the total time to complete every process on the machine divided by the number of processes that were run.

The resulting table should have the `machine_id` along with the **average time** as `processing_time`, which should be **rounded to 3 decimal places**.

#### 💡 SQL Solution

```sql
SELECT a1.machine_id,  round(avg(a2.timestamp - a1.timestamp), 3) as processing_time
FROM Activity a1
JOIN Activity a2
ON a1.machine_id = a2.machine_id and a1.process_id = a2.process_id
WHERE a1.activity_type = 'start' and a2.activity_type = 'end' 
GROUP BY a1.machine_id;
````
⚡ This solution beats **76.97%** of other submissions' runtime.

---

#### [1683. Invalid Tweets](https://leetcode.com/problems/invalid-tweets/description/?envType=study-plan-v2&envId=top-sql-50)
Write a solution to find the IDs of the invalid tweets. The tweet is invalid if the number of characters used in the content of the tweet is **strictly greater** than `15`.

#### 💡 SQL Solution

```sql
SELECT tweet_id
FROM Tweets
WHERE LEN(content) > 15;
````
⚡ This solution beats **64%** of other submissions' runtime.

---

#### [1729. Find Followers Count](https://leetcode.com/problems/find-followers-count/submissions/1642478268/?envType=study-plan-v2&envId=top-sql-50)

Write a solution that will, for each user, return the number of followers.
Return the result table ordered by user_id in ascending order.

#### 💡 SQL Solution

```sql
SELECT user_id, count(follower_id) as followers_count
FROM Followers
GROUP BY user_id;
````
⚡ This solution beats **64.32%** of other submissions' runtime.

---

#### [1757. Recyclable and Low Fat Products](https://leetcode.com/problems/recyclable-and-low-fat-products/solutions/?envType=study-plan-v2&envId=top-sql-50)
Write a solution to find the IDs of products that are **both** low fat and recyclable.

#### 💡 SQL Solution

```sql
SELECT product_id
FROM Products
WHERE low_fats = 'Y' AND recyclable = 'Y';
````
⚡ This solution beats **79%** of other submissions' runtime.

---
#### [2356. Number of Unique Subjects Taught by Each Teacher](https://leetcode.com/problems/number-of-unique-subjects-taught-by-each-teacher/description/?envType=study-plan-v2&envId=top-sql-50)
Write a solution to calculate the number of unique subjects each teacher teaches in the university.

#### 💡 SQL Solution

```sql
SELECT teacher_id, COUNT(DISTINCT subject_id) AS cnt
FROM Teacher
GROUP BY teacher_id;
````
⚡ This solution beats **92%** of other submissions' runtime.

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
#### [1757. Recyclable and Low Fat Products](https://leetcode.com/problems/recyclable-and-low-fat-products/solutions/?envType=study-plan-v2&envId=top-sql-50)
Write a solution to find the IDs of products that are **both** low fat and recyclable.

#### 💡 SQL Solution

```sql
SELECT product_id
FROM Products
WHERE low_fats = 'Y' AND recyclable = 'Y';
````
⚡ This solution beats **79%** of other submissions' runtime.

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

#### [1757. Recyclable and Low Fat Products](https://leetcode.com/problems/recyclable-and-low-fat-products/solutions/?envType=study-plan-v2&envId=top-sql-50)
Write a solution to find the IDs of products that are **both** low fat and recyclable.

#### 💡 SQL Solution

```sql
SELECT product_id
FROM Products
WHERE low_fats = 'Y' AND recyclable = 'Y';
````
⚡ This solution beats **79%** of other submissions' runtime.

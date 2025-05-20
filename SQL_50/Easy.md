
### [1757. Recyclable and Low Fat Products](https://leetcode.com/problems/recyclable-and-low-fat-products/solutions/?envType=study-plan-v2&envId=top-sql-50)

#### 🧾 Table: `Products`

| Column Name | Type  | Description                        |
|-------------|-------|------------------------------------|
| product_id  | int   | Primary key                        |
| low_fats    | enum  | `'Y'` means low fat, `'N'` means not |
| recyclable  | enum  | `'Y'` means recyclable, `'N'` means not |



#### Problem Description

Write a solution to find the IDs of products that are **both** low fat and recyclable.
Return the result table in **any order**.


#### 💡 SQL Solution

```sql
SELECT product_id
FROM Products
WHERE low_fats = 'Y' AND recyclable = 'Y';

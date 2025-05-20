#### [570. Managers with at Least 5 Direct Reports](https://leetcode.com/problems/managers-with-at-least-5-direct-reports/?envType=study-plan-v2&envId=top-sql-50)
Write a solution to find managers with at least **five direct reports**.

#### 💡 SQL Solution

```sql
SELECT name 
FROM employee
WHERE id in (
    SELECT e.managerID
    FROM Employee E
    JOIN Employee M on e.managerID = M.id
    GROUP BY e.managerID
    HAVING count(e.id) >= 5);
```
⚡ This solution beats **77.73%%** of other submissions' runtime.

---


#### [1934. Confirmation Rate](https://leetcode.com/problems/confirmation-rate/description/?envType=study-plan-v2&envId=top-sql-50)
The confirmation rate of a user is the number of 'confirmed' messages divided by the total number of requested confirmation messages. The confirmation rate of a user that did not request any confirmation messages is 0. Round the confirmation rate to **two decimal** places.

Write a solution to find the confirmation rate of each user.

#### 💡 SQL Solution

```sql
SELECT S.USER_ID,
        round(avg(CASE action WHEN 'confirmed' then 1.0 else 0 end), 2) as confirmation_rate
FROM SIGNUPS S
LEFT JOIN Confirmations C
ON S.user_id = C.user_id
GROUP BY S.user_id;
```
⚡ This solution beats **98.95%%** of other submissions' runtime.

---

---
sidebar_position: 2
---

# Leetcode SQL50

https://leetcode.com/studyplan/top-sql-50/

![](./sql-50-demo.png)

## Tổng hợp note tutorial

### 1. SELECT (EASY - tầm 12p)

- Câu 1.
- Câu 2.
- Câu 3.
- Câu 4.
- Câu 5.

### 2. BASIC JOINS (1h)

- Câu 1.
- Câu 2.
- Câu 3.
- Câu 4.
- Câu 5.
- Câu 6.
- Câu 7.
- Câu 8.
- Câu 9.

### 3. Basic Aggregate Functions

- Câu 1.
- Câu 2.
- Câu 3.
- Câu 4.
- Câu 5.
- Câu 6.
- Câu 7.
- Câu 8.

### 4. Sorting and Grouping

- Câu 1.
- Câu 2.
- Câu 3.
- Câu 4.
- Câu 5.
- Câu 6.
- Câu 7.

### 5. Advanced Select and Joins

- Câu 1.
- Câu 2.
- Câu 3.
- Câu 4.
- Câu 5.
- Câu 6.
- Câu 7.

### 6. Subqueries

- Câu 1.
- Câu 2.
- Câu 3.
- Câu 4.
- Câu 5.
- Câu 6.
- Câu 7.

### 7. Advanced String Functions / Regex / Clause

- Câu 1.
- Câu 2.
- Câu 3.
- Câu 4.
- Câu 5.
- Câu 6.
- Câu 7.

**Ví dụ query cuối tài liệu:**

```sql
-- Write your MySQL query statement below
SELECT
    user_id,
    CONCAT(UPPER(LEFT(name, 1)), LOWER(SUBSTRING(name, 2))) AS name
FROM Users
ORDER BY user_id;
```

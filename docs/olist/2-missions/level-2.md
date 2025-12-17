# Level 2: Simple Question

---

## Task 2.1: Data request from Sales

You receive the following email from Sofia, the Regional Sales Manager.

:::note Email
**From**: Sofia (Regional Sales Manager)

**To**: Data Team

**Subject**: URGENT: 2017 Numbers

> "Hey team! 👋
> I'm preparing my slide deck for the board meeting tomorrow. I need to know our Total Sales for 2017.
> Just get me the single number. Thanks!"

:::

**Logic / Approach:**
> 1.Identify the table that contains sales value:
> - Sales value is stored in the order_items table
> - Relevant columns: price (item price), freight_value (shipping cost)
> 2. Filter by time period and valid orders:
> - The order date and order status are stored in the orders table -> JOIN orders with order_items using order_id
> - Filter orders placed in 2017 using order_purchase_timestamp
> - Only include orders with order_status = 'delivered'
> 3. Calculate Total Sales:
> - Total Sales = SUM(price + freight_value) for delivered orders in 2017

```sql
SELECT 
  SUM(oi.price + oi.freight_value) AS total_sales_2017
FROM orders o
JOIN order_items oi
  ON o.order_id = oi.order_id
WHERE o.order_status = 'delivered'
  AND o.order_purchase_timestamp >= '2017-01-01'
  AND o.order_purchase_timestamp < '2018-01-01';
```
![](../assets/results2.1.png)

**Total Sales for 2017 is: 6,921,535.24**


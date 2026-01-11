# 1. Total Revenue

## Problem

:::info Question
Sofia (the last VP of Sales):

> Hi Yen, thank you for your last help.
> Actually we are flying blind right now.
> I need a dashboard that tells me how much money we have processed in history and how many orders that represents.
> Can you do me that favor? Keep it simple, I need it for my slide deck tomorrow.

Your goal is to answer two questions:

1. What is our Total Revenue?
2. How many orders have we processed?

**Deliverable:**

This time, **save it as Metabase's questions**:

- Metabase questions that answer the two questions above.
  - Name: `KPI - Total Revenue` and `KPI - Total Orders`
  - Description

:::

**Logic / Approach:**
- Processed orders include orders with the *order_status* of "shipped" or "delivered", and "canceled" orders that have a non-null *order_delivered_carrier_date*

![](../assets/processed%20order.jpeg)

```sql
SELECT
  COUNT(DISTINCT order_id) AS total_processed_orders
FROM orders
WHERE order_status IN ('shipped', 'delivered')
  OR (order_status = 'canceled'
  AND order_delivered_carrier_date IS NOT NULL);
```
![](../assets/total%20orders.png)

- Total revenue is calculated as the actual amount received from customers, based on *payment_value*

```sql
SELECT
  SUM(op.payment_value) AS total_revenue
FROM orders o
JOIN order_payments op ON o.order_id = op.order_id
WHERE o.order_status = 'delivered';
```
![](../assets/total%20revenue%20card.png)
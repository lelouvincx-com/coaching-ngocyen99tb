# 4. Price vs. Freight Structure

## Problem

:::info Question
Sofia looks at the chart. "Wait, does this revenue figure include what customers paid for shipping? Because we pay that money straight to the shipping vendors; we don't keep it. If shipping is high, our margins are lower."

Can you help distinguish between Product Revenue (Price) and Shipping Revenue (Freight)?

Deliverable:

- A Metabase question that shows the breakdown of revenue into Price vs. Freight.

:::

**Logic / Approach:**

- Calculate Product Revenue (Price) = SUM(order_items.price); Shipping Revenue (Freight) = SUM(order_items.freight_value)

- Join orders with order_items using order_id to access order status. Apply filter orders.order_status = 'delivered'

- After computing the two totals (originally as columns), use UNPIVOT to transform columns into rows

```sql
WITH totals AS (
  SELECT
    SUM(oi.price) AS 'Total Product Price',
    SUM(oi.freight_value) AS 'Total Shipping Freight'
  FROM orders o
  JOIN order_items oi
    ON o.order_id = oi.order_id
  WHERE o.order_status = 'delivered' )
SELECT
  metric,
  value
FROM totals
UNPIVOT (value FOR metric IN (
  'Total Product Price',
  'Total Shipping Freight' ));
```

![](../assets/revenue-breakdown.png)

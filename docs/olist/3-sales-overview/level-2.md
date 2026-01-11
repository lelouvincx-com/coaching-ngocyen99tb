# 2. Monthly Revenue Trend

## Problem 1

:::info Question
After seeing the total revenue number, Sofia is impressed but cautious.

> Okay, the number is great. But are we growing? Or are we dying? Could you show me the trend over time?

Your goal is to plot the revenue history by time.

Deliverable:

- A Metabase question that shows the monthly revenue trend over time.
  - Name: `Trend - Monthly Revenue`
  - Description

```sql
SELECT
  DATE_TRUNC('month', o.order_purchase_timestamp) AS month,
  SUM(op.payment_value) AS monthly_revenue
FROM orders o
JOIN order_payments op
  ON o.order_id = op.order_id
WHERE o.order_status = 'delivered'
GROUP BY 1;
```

![](../assets/trend-monthly-revenue.png)

:::

## Problem 2

:::info Question
It's time to learn about **Data Granularity**.

**Materials:**

https://medium.com/@adedola/data-granularity-d8ba107dd236

![](https://media.secondbrain.lelouvincx.com/2026/01/00aaf6aed63273ad3a159d6f51373e42.png)

![](https://media.secondbrain.lelouvincx.com/2026/01/91dca15c5e363f7fc7d19833013ddf3b.png)
:::

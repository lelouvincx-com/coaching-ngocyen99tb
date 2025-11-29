# 2. SQL Missions

## Level 0 - Prepare Your Environment

Before diving into the missions, ensure you have access to the Olist database. Set up your SQL environment and familiarize yourself with the following datasets.

Choose either:

- RunSQL
- Dbeaver

The data is available at: https://public.lelouvincx.com/brazilian_ecommerce.duckdb

**Goal**: Ready your SQL environment by querying `SELECT * FROM olist_orders_dataset` to confirm access.

![](./assets/sql-ready.png)

## Level 1 – “RJ Cartographer”

_Objective: Find and understand your dataset._

**Story beat**
Before you analyze anything, you must locate **who** in the dataset lives in RJ and **what orders** belong to them. You’re drawing a map of the crisis zone.

---

### Task 1: Know Your Audience (Simple Filtering)

**Business Value:** Before we analyze behavior, we need to know the size of our cohort in Rio de Janeiro.

- **Goal:** List all customer profiles located in the state of Rio de Janeiro ('RJ').

- **Success Criteria:**
  - "How many unique customers do we have in Rio de Janeiro?" _(Hint: It should be around 12,000+ rows)_.
  - "Besides the city 'rio de janeiro', what other cities appear in this list?"

<details>
<summary>💡 Hint</summary>

- Query the `olist_customers_dataset`.
- Filter where `customer_state` is 'RJ'.
- Select `customer_id` and `customer_city`.

</details>

### Task 2: The Transaction History (Basic Join)

**Business Value:** A customer profile is useless without their purchase history. We need to attach orders to these people.

- **Goal:** Join the Orders table to the Customers table to find every order placed by an RJ customer.

- **Success Criteria:**
  - "Does the row count match your previous query, or is it different? Why?"
  - "Can you see the `order_status` column in the results?"

<details>
<summary>💡 Hint</summary>

- `INNER JOIN` `olist_orders_dataset` with `olist_customers_dataset`.
- Join key: `customer_id`.
- Filter for `customer_state = 'RJ'`.

</details>

### Task 3: The Timeline (Date Handling)

**Business Value:** The VP needs to know if this is a recent problem or a historical one. We need to establish the date range of our data.

- **Goal:** Find the date range of all orders placed in RJ.

- **Success Criteria:**
  - "What is the date of the very first order in RJ?"
  - "When was the last order placed?"
  - "Does this cover the Black Friday period?"

<details>
<summary>💡 Hint</summary>

- Use the joined dataset from Task 2.
- Use `MIN()` and `MAX()` functions on `order_purchase_timestamp`.
- _Bonus:_ Cast the timestamp to a `DATE` format to make it readable.

</details>

### Task 4: The Funnel Audit (Aggregation & Nulls)

**Business Value:** Not all orders make it to the customer. We need to see how many orders were actually delivered vs. cancelled or unavailable.

- **Goal:** Count the number of orders per `order_status` for RJ customers.

- **Success Criteria:**
  - "How many orders in RJ were 'canceled'?"
  - "How many were 'delivered'?"
  - _Critical Check:_ "Are there any statuses where the count is surprisingly high?"

<details>
<summary>💡 Hint</summary>

- Use `GROUP BY order_status`.
- `COUNT` the order IDs.
- Order the results by the count in descending order.

</details>

### Task 5: The "Pulse Check" (3-Table Join)

**Business Value:** Now we need the "Voice of the Customer." We need to see the average star rating for these specific orders.

- **Goal:** Calculate the average review score for all **delivered** orders in RJ.

- **Success Criteria:**
  - "What is the average score for RJ? (Is it below 4.0?)"
  - "How does this compare if you remove the 'RJ' filter and look at the whole country?"

<details>
<summary>💡 Hint</summary>

- Join `olist_order_reviews_dataset` to your existing Orders + Customers query.
- **Filter 1:** `customer_state = 'RJ'`
- **Filter 2:** `order_status = 'delivered'`
- Use `AVG(review_score)`.

</details>

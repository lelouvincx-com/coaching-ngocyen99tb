# 1. Dataset Introduction

## Conceptual Overview

**The world you’re entering:**

- Olist connects small sellers to big online marketplaces.
- Orders are shipped through logistics partners across Brazil.
- The Brazilian E-Commerce Public Dataset by Olist captures ~100k real orders from 2016–2018, including customers, orders, payments, deliveries, and reviews.

![alt text](./assets/image.png)

Olist connects small businesses from all over Brazil to channels without hassle and with a single contract. Those merchants are able to sell their products through the Olist Store and ship them directly to the customers using Olist logistics partners.

After a customer purchases the product from Olist Store a seller gets notified to fulfill that order. Once the customer receives the product, or the estimated delivery date is due, the customer gets a satisfaction survey by email where he can give a note for the purchase experience and write down some comments.

## ERD

<iframe  allowfullscreen width="100%" height="600" src="https://dbdocs.io/embed/3c687c0abb1040e2f9be3a629c7da92e/170abe8e57ab4432b2a099c878f650be"> </iframe>

**ERD link**: https://dbdocs.io/lelouvincx/brazillian-ecommerce

You begin with read access to the following:

- **`olist_orders_dataset`** (central fact table)

  - `order_id` — unique order key
  - `order_status` — delivered / shipped / canceled / etc.
  - `order_estimated_delivery_date` — when Olist promised delivery
  - `order_delivered_customer_date` — when the customer actually got it

- **`olist_order_reviews_dataset`**

  - `order_id`
  - `review_score` — 1 to 5 stars

- **`olist_customers_dataset`**

  - `customer_id`
  - `customer_state` — two-letter state code (e.g., RJ, SP, MG)

Later levels may optionally touch:

- `olist_order_items_dataset` (each line item in an order)
- `olist_sellers_dataset` (seller info)
- `olist_geolocation_dataset` (ZIP → lat/long)

:::tip
**Rule:** For this quest, you **must** always filter on customers in `customer_state = 'RJ'`. That’s your battlefield.
:::

## Acknowledgements

Thanks to Olist for releasing this dataset.

Original link: https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce

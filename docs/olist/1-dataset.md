---
sidebar_position: 2
---

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

- **`orders`** (central fact table)

  - `order_id` — unique order key
  - `order_status` — delivered / shipped / canceled / etc.
  - `order_estimated_delivery_date` — when Olist promised delivery
  - `order_delivered_customer_date` — when the customer actually got it

- **`order_reviews`**

  - `order_id`
  - `review_score` — 1 to 5 stars

- **`customers`**

  - `customer_id`
  - `customer_state` — two-letter state code (e.g., RJ, SP, MG)

Later levels may optionally touch:

- `order_items` (each line item in an order)
- `sellers` (seller info)
- `geolocation` (ZIP → lat/long)

:::tip
**Rule:** For this quest, you **must** always filter on customers in `customer_state = 'RJ'`. That’s your battlefield.
:::

<details>
<summary>Dataset schema (full)</summary>
```dbml
// Brazilian E-commerce Database Schema

Project brazilian_ecommerce {
  database_type: 'PostgreSQL'
  Note: '''
    # Brazilian E-commerce Dataset ERD
    This database contains information about orders, customers, products, sellers, and reviews from a Brazilian e-commerce platform (Olist).

    **Source**: https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce
    **Dataset size**: ~100K orders, 33K products, 3K sellers
    **Time period**: 2016-2018
  '''
}

Table customers {
  customer_id uuid [primary key, note: 'Unique identifier for each customer (per order)']
  customer_unique_id uuid [note: 'Unique identifier for customer across multiple orders']
  customer_zip_code_prefix int [note: '5-digit ZIP code prefix']
  customer_city varchar [note: 'Customer city name']
  customer_state varchar(2) [note: '2-letter state abbreviation (e.g., SP, RJ)']

  Note: 'Customer information. One customer_id per order, but customer_unique_id links multiple orders from same customer.'

  indexes {
    customer_unique_id
    customer_zip_code_prefix
    (customer_city, customer_state)
  }
}

Table sellers {
  seller_id uuid [primary key, note: 'Unique identifier for each seller/merchant']
  seller_zip_code_prefix int [note: '5-digit ZIP code prefix of seller location']
  seller_city varchar [note: 'Seller city name']
  seller_state varchar(2) [note: '2-letter state abbreviation']

  Note: 'Seller/merchant information. Each seller can have multiple products in order_items.'

  indexes {
    seller_zip_code_prefix
    (seller_city, seller_state)
  }
}

Table products {
  product_id uuid [primary key, note: 'Unique identifier for each product']
  product_category_name varchar [note: 'Product category name in Portuguese']
  product_name_lenght int [null, note: 'Length of product name in characters']
  product_description_lenght int [null, note: 'Length of product description in characters']
  product_photos_qty int [null, note: 'Number of product photos']
  product_weight_g int [null, note: 'Product weight in grams']
  product_length_cm int [null, note: 'Product length in centimeters']
  product_height_cm int [null, note: 'Product height in centimeters']
  product_width_cm int [null, note: 'Product width in centimeters']

  Note: 'Product catalog with dimensions and category. Physical dimensions used for shipping calculations.'

  indexes {
    product_category_name
  }
}

Table product_category_translation {
  product_category_name varchar [primary key, note: 'Category name in Portuguese']
  product_category_name_english varchar [note: 'Category name translated to English']

  Note: 'Translation table for product categories from Portuguese to English. Contains 70 category mappings.'
}

Table geolocation {
  geolocation_zip_code_prefix int [note: '5-digit ZIP code prefix']
  geolocation_lat decimal(10,8) [note: 'Latitude coordinate']
  geolocation_lng decimal(11,8) [note: 'Longitude coordinate']
  geolocation_city varchar [note: 'City name']
  geolocation_state varchar(2) [note: '2-letter state abbreviation']

  Note: 'Geographic lookup table with 1M+ records mapping ZIP codes to coordinates. Multiple entries per ZIP code due to coordinate variations.'

  indexes {
    geolocation_zip_code_prefix [name: 'idx_geo_zip']
    (geolocation_city, geolocation_state) [name: 'idx_geo_location']
  }
}

Table orders {
  order_id uuid [primary key, note: 'Unique identifier for each order']
  customer_id uuid [not null, note: 'Reference to customer who placed the order']
  order_status varchar [note: 'Order status: delivered, shipped, canceled, etc.']
  order_purchase_timestamp timestamp [not null, note: 'When the order was created']
  order_approved_at timestamp [null, note: 'When payment was approved']
  order_delivered_carrier_date timestamp [null, note: 'When order was handed to logistics carrier']
  order_delivered_customer_date timestamp [null, note: 'When order was delivered to customer']
  order_estimated_delivery_date timestamp [note: 'Estimated delivery date shown to customer']

  Note: 'Central fact table for orders. Contains order lifecycle timestamps from purchase to delivery.'

  indexes {
    customer_id
    order_status
    order_purchase_timestamp
  }
}

Table order_items {
  order_id uuid [note: 'Reference to parent order']
  order_item_id int [note: 'Sequential number identifying item within order (1, 2, 3...)']
  product_id uuid [not null, note: 'Reference to product purchased']
  seller_id uuid [not null, note: 'Reference to seller fulfilling this item']
  shipping_limit_date timestamp [note: 'Deadline for seller to ship the item']
  price decimal(10,2) [note: 'Item price in BRL (Brazilian Real)']
  freight_value decimal(10,2) [note: 'Shipping cost for this item in BRL']

  Note: 'Order line items. Bridge table connecting orders to products and sellers. Each order can have multiple items from different sellers.'

  indexes {
    (order_id, order_item_id) [pk]
    product_id
    seller_id
    shipping_limit_date
  }
}

Table order_payments {
  order_id uuid [note: 'Reference to parent order']
  payment_sequential int [note: 'Sequential number for multiple payments per order (1, 2, 3...)']
  payment_type varchar [note: 'Payment method: credit_card, boleto, voucher, debit_card']
  payment_installments int [note: 'Number of installments (common in Brazil)']
  payment_value decimal(10,2) [note: 'Payment amount in BRL']

  Note: 'Payment information. Orders can have multiple payment methods (e.g., split payment). One payment_sequential per payment type used.'

  indexes {
    (order_id, payment_sequential) [pk]
    payment_type
  }
}

Table order_reviews {
  review_id uuid [pk, note: 'Unique identifier for each review']
  order_id uuid [not null, note: 'Reference to order being reviewed']
  review_score int [note: 'Rating from 1 to 5 stars']
  review_comment_title varchar [null, note: 'Review title written by customer']
  review_comment_message text [null, note: 'Review message/body written by customer']
  review_creation_date timestamp [note: 'When customer wrote the review']
  review_answer_timestamp timestamp [note: 'When review was processed/answered']

  Note: 'Customer reviews and ratings. Not all orders have reviews. Contains satisfaction scores and optional text feedback.'

  indexes {
    order_id
    review_score
    review_creation_date
  }
}

// ===== RELATIONSHIPS =====

// Customer to Orders (1:M)
Ref has: customers.customer_id < orders.customer_id

// Orders to Order Items (1:M)
Ref contains: orders.order_id < order_items.order_id

// Orders to Order Payments (1:M)
Ref paid_by: orders.order_id < order_payments.order_id

// Orders to Order Reviews (1:1 or 1:0)
Ref reviewed_in: orders.order_id - order_reviews.order_id

// Products to Order Items (1:M)
Ref sold_in: products.product_id < order_items.product_id

// Sellers to Order Items (1:M)
Ref fulfills: sellers.seller_id < order_items.seller_id

// Product Category Translation to Products (1:M)
Ref categorizes: product_category_translation.product_category_name < products.product_category_name

// Geolocation to Customers (M:M via zip code lookup)
Ref locates_customer: geolocation.geolocation_zip_code_prefix < customers.customer_zip_code_prefix

// Geolocation to Sellers (M:M via zip code lookup)
Ref locates_seller: geolocation.geolocation_zip_code_prefix < sellers.seller_zip_code_prefix
```
</details>

## Acknowledgements

Thanks to Olist for releasing this dataset.

Original link: https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce

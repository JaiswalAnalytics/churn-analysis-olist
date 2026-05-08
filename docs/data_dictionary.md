 # Data Dictionary — Olist Brazilian E-Commerce Dataset

**Source:** [Kaggle — Brazilian E-Commerce Public Dataset by Olist](https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce)  
**Period:** October 2016 – October 2018  
**Total Orders:** 99,441 | **Unique Customers:** 96,096 | **Files:** 9 CSVs

---

## Data Model Overview

```
olist_customers_dataset
        │
        └──► olist_orders_dataset (FACT TABLE)
                    │
        ┌───────────┼────────────────┐
        │           │                │
order_items    order_payments   order_reviews
        │
     products ──► category_translation
        │
     sellers ──► geolocation
```

---

## File Descriptions

### 1. olist_orders_dataset.csv
**Shape:** 99,441 rows × 8 columns | **Role:** Central FACT table

| Column | Type | Description | Nulls |
|--------|------|-------------|-------|
| `order_id` | string | Unique order identifier | 0 |
| `customer_id` | string | Key to customer dataset (not unique across orders) | 0 |
| `order_status` | string | delivered / shipped / canceled / unavailable / invoiced / processing / created / approved | 0 |
| `order_purchase_timestamp` | datetime | When customer placed the order | 0 |
| `order_approved_at` | datetime | When payment was approved | 160 → filled with purchase timestamp |
| `order_delivered_carrier_date` | datetime | When order was handed to logistics partner | 1,783 → undelivered orders |
| `order_delivered_customer_date` | datetime | When order was delivered to customer | 2,965 → undelivered orders |
| `order_estimated_delivery_date` | datetime | Estimated delivery date shown to customer at purchase | 0 |

**Engineered columns:**
- `delivery_delay_days` = `order_delivered_customer_date` − `order_estimated_delivery_date` (negative = early)
- `is_delivered` = 1 if delivered, 0 if not
- `Delay Bucket` = categorical grouping of delay (Early 7d+, Early 1–7d, On time, 1–3d late, etc.)

---

### 2. olist_customers_dataset.csv
**Shape:** 99,441 rows × 5 columns

| Column | Type | Description | Nulls |
|--------|------|-------------|-------|
| `customer_id` | string | Joins to orders dataset — NOT unique across orders | 0 |
| `customer_unique_id` | string | True unique customer identifier — use this for RFM | 0 |
| `customer_zip_code_prefix` | string | First 5 digits of zip code | 0 |
| `customer_city` | string | Customer city name | 0 |
| `customer_state` | string | 2-letter Brazilian state code (e.g. SP, RJ) | 0 |

> ⚠️ **Key note:** Always use `customer_unique_id` for customer-level analysis. `customer_id` repeats across orders and will overcount unique customers.

---

### 3. olist_order_items_dataset.csv
**Shape:** 112,650 rows × 7 columns (multiple items per order)

| Column | Type | Description | Nulls |
|--------|------|-------------|-------|
| `order_id` | string | Links to orders dataset | 0 |
| `order_item_id` | integer | Sequential item number within an order (1, 2, 3...) | 0 |
| `product_id` | string | Links to products dataset | 0 |
| `seller_id` | string | Links to sellers dataset | 0 |
| `shipping_limit_date` | datetime | Seller must ship by this date | 0 |
| `price` | float | Item price in BRL (R$) | 0 |
| `freight_value` | float | Freight cost charged to customer | 0 |

**Aggregated to order level:**
- `total_items` = count of `order_item_id` per order
- `total_price` = sum of `price` per order
- `total_freight` = sum of `freight_value` per order

---

### 4. olist_order_payments_dataset.csv
**Shape:** 103,886 rows × 5 columns (multiple payment methods per order)

| Column | Type | Description | Nulls |
|--------|------|-------------|-------|
| `order_id` | string | Links to orders dataset | 0 |
| `payment_sequential` | integer | Multiple payments possible (1, 2, 3...) | 0 |
| `payment_type` | string | credit_card / boleto / voucher / debit_card | 0 |
| `payment_installments` | integer | Number of installments chosen | 0 |
| `payment_value` | float | Value paid in this transaction (BRL) | 0 |

**Aggregated to order level:**
- `total_payment` = sum of `payment_value` per order
- `payment_installments` = max installments per order
- `payment_type` = first payment method per order

---

### 5. olist_order_reviews_dataset.csv
**Shape:** 99,224 rows × 7 columns

| Column | Type | Description | Nulls |
|--------|------|-------------|-------|
| `review_id` | string | Unique review identifier | 0 |
| `order_id` | string | Links to orders dataset | 0 |
| `review_score` | integer | Customer rating 1–5 (5 = best) | 0 |
| `review_comment_title` | string | Optional short title | 87,656 → optional field |
| `review_comment_message` | string | Optional full review text | 58,247 → optional field |
| `review_creation_date` | datetime | When review was created | 0 |
| `review_answer_timestamp` | datetime | When seller responded | 0 |

> Only `review_score` is used in this project. Comment fields are optional and have high null rates — not a data quality issue.

---

### 6. olist_products_dataset.csv
**Shape:** 32,951 rows × 9 columns

| Column | Type | Description | Nulls |
|--------|------|-------------|-------|
| `product_id` | string | Unique product identifier | 0 |
| `product_category_name` | string | Category name in Portuguese | 610 → filled as 'unknown' |
| `product_name_lenght` | integer | Character count of product name | 610 |
| `product_description_lenght` | integer | Character count of description | 610 |
| `product_photos_qty` | integer | Number of product photos | 610 |
| `product_weight_g` | float | Weight in grams | 2 → filled with median |
| `product_length_cm` | float | Length in cm | 2 → filled with median |
| `product_height_cm` | float | Height in cm | 2 → filled with median |
| `product_width_cm` | float | Width in cm | 2 → filled with median |

---

### 7. olist_sellers_dataset.csv
**Shape:** 3,095 rows × 4 columns | **Nulls:** 0

| Column | Type | Description |
|--------|------|-------------|
| `seller_id` | string | Unique seller identifier |
| `seller_zip_code_prefix` | string | First 5 digits of seller zip |
| `seller_city` | string | Seller city |
| `seller_state` | string | 2-letter state code |

---

### 8. olist_geolocation_dataset.csv
**Shape:** 1,000,163 rows × 5 columns | **Nulls:** 0

| Column | Type | Description |
|--------|------|-------------|
| `geolocation_zip_code_prefix` | string | Zip code prefix |
| `geolocation_lat` | float | Latitude |
| `geolocation_lng` | float | Longitude |
| `geolocation_city` | string | City name |
| `geolocation_state` | string | State code |

> Used for geographic churn analysis. Contains multiple lat/lng per zip — use median or first record per zip.

---

### 9. product_category_name_translation.csv
**Shape:** 71 rows × 2 columns | **Nulls:** 0

| Column | Type | Description |
|--------|------|-------------|
| `product_category_name` | string | Category name in Portuguese |
| `product_category_name_english` | string | English translation |

---

## Engineered / Master Table Columns

These columns were created during data cleaning and do not exist in the raw files:

| Column | Source | Description |
|--------|--------|-------------|
| `delivery_delay_days` | orders | Actual − Estimated delivery date in days |
| `is_delivered` | orders | 1 = delivered, 0 = not delivered |
| `Delay Bucket` | orders | Categorical delay grouping (7 values) |
| `Delay Sort` | orders | Numeric sort key for Delay Bucket (1–7) |
| `total_payment` | payments | Sum of all payments per order |
| `total_items` | items | Count of items per order |
| `total_price` | items | Sum of item prices per order |
| `total_freight` | items | Sum of freight per order |
| `product_category_name_english` | products + translation | English category name |

---

## RFM Table Columns

| Column | Description |
|--------|-------------|
| `customer_unique_id` | True unique customer ID |
| `Recency` | Days since last order from snapshot date |
| `Frequency` | Total number of orders placed |
| `Monetary` | Total spend in R$ |
| `R_Score` | Recency score 1–5 (5 = most recent) |
| `F_Score` | Frequency score 1–5 (5 = most frequent) |
| `M_Score` | Monetary score 1–5 (5 = highest spend) |
| `Segment` | Champion / Loyal / Potential Loyalist / At Risk / Lost |

---

*Last updated: May 2026 | Project: Customer Churn & Revenue Leakage Analysis*

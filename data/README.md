# Data folder

This folder holds the CYRV e-commerce dataset: real, anonymized orders from a
Brazilian online marketplace. Each CSV covers one part of the order flow
(customers, orders, items, payments, reviews, products, sellers). They link
together through shared ID columns, described below.

## Files

### `CYRV_customers_dataset.csv`
One row per customer, per order.
- `customer_id` — id used in `CYRV_orders_dataset.csv` to link to an order.
- `customer_unique_id` — id for the actual person; a person can have several `customer_id`s across different orders.
- `customer_zip_code_prefix` — customer's zip code prefix.
- `customer_city` — customer's city.
- `customer_state` — customer's state.

### `CYRV_orders_dataset.csv`
One row per order. The central table linking customers, items, and payments.
- `order_id` — unique order id, used across most other files.
- `customer_id` — links to `CYRV_customers_dataset.csv`.
- `order_status` — e.g. delivered, shipped, canceled.
- `order_purchase_timestamp` — when the order was placed.
- `order_approved_at` — when payment was approved.
- `order_delivered_carrier_date` — when the order was handed to the carrier.
- `order_delivered_customer_date` — when the order reached the customer.
- `order_estimated_delivery_date` — delivery date shown to the customer at purchase.

### `CYRV_order_items_dataset.csv`
One row per item within an order (orders can have multiple items).
- `order_id` — links to `CYRV_orders_dataset.csv`.
- `order_item_id` — position of the item within the order.
- `product_id` — links to `CYRV_products_dataset.csv`.
- `seller_id` — links to `CYRV_sellers_dataset.csv`.
- `shipping_limit_date` — deadline for the seller to hand the item to the carrier.
- `price` — item price.
- `freight_value` — shipping cost.

### `CYRV_order_payments_dataset.csv`
One row per payment made towards an order (an order can be paid in installments or with multiple methods).
- `order_id` — links to `CYRV_orders_dataset.csv`.
- `payment_sequential` — order of the payment if there were several.
- `payment_type` — e.g. credit card, boleto, voucher.
- `payment_installments` — number of installments.
- `payment_value` — amount paid.

### `CYRV_order_reviews_dataset.csv`
One row per customer review of an order.
- `review_id` — unique review id.
- `order_id` — links to `CYRV_orders_dataset.csv`.
- `review_score` — 1 to 5 rating.
- `review_comment_title` — optional review title.
- `review_comment_message` — optional review text.
- `review_creation_date` — when the review was sent.
- `review_answer_timestamp` — when the review was answered.

### `CYRV_products_dataset.csv`
One row per product.
- `product_id` — links to `CYRV_order_items_dataset.csv`.
- `product_category_name` — category in Portuguese; see the translation file below.
- `product_name_lenght` — length of the product name.
- `product_description_lenght` — length of the product description.
- `product_photos_qty` — number of photos in the listing.
- `product_weight_g` — package weight in grams.
- `product_length_cm` — package length in cm.
- `product_height_cm` — package height in cm.
- `product_width_cm` — package width in cm.

### `CYRV_sellers_dataset.csv`
One row per seller.
- `seller_id` — links to `CYRV_order_items_dataset.csv`.
- `seller_zip_code_prefix` — seller's zip code prefix.
- `seller_city` — seller's city.
- `seller_state` — seller's state.

### `CYRV_geolocation_dataset.csv`
Zip code prefix to latitude/longitude lookup, used to map customer and seller locations.
- `geolocation_zip_code_prefix` — links to the zip prefix columns in the customers/sellers files.
- `geolocation_lat` — latitude.
- `geolocation_lng` — longitude.
- `geolocation_city` — city name.
- `geolocation_state` — state name.

### `product_category_name_translation.csv`
Small lookup table translating `product_category_name` (Portuguese) to English.
- `product_category_name` — links to `CYRV_products_dataset.csv`.
- `product_category_name_english` — English translation.

## How the files connect

```
customers → orders → order_items → products
                   → order_payments      → sellers
                   → order_reviews

geolocation: joins to customers/sellers via zip code prefix
product_category_name_translation: joins to products via category name
```

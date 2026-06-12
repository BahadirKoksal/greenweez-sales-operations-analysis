# 🛒 Greenweez Sales Operations Analysis — BigQuery SQL

## 🎯 Objective

Greenweez is a French organic e-commerce company. This project analyzes sales and operational data from 2021 to measure true profitability beyond just revenue — combining product margins, shipping costs, operational expenses, and ad spend into a unified view.

**Key Research Questions:**
- What is the true operational profit per order after all costs are accounted for?
- Which products have high, medium, or low margin levels?
- What types of promotions are being applied, and at what discount rates?
- How do logistics and advertising costs impact daily profitability?

---

## 📁 Datasets

**Source:** Google BigQuery — `course17` dataset

### `gwz_sales_17` — Raw Sales Data (1,168,081 rows)

| Column | Type | Description |
|---|---|---|
| `date_date` | DATE | Order date |
| `orders_id` | INTEGER | Order identifier |
| `products_id` | INTEGER | Product identifier |
| `customers_id` | INTEGER | Customer identifier |
| `category_1` | STRING | Top-level product category |
| `category_2` | STRING | Mid-level product category |
| `category_3` | STRING | Detailed product category |
| `turnover` | FLOAT | Revenue after discount |
| `turnover_before_promo` | FLOAT | Revenue before discount |
| `purchase_cost` | FLOAT | Cost of goods sold |
| `qty` | INTEGER | Quantity sold |
| `code` | STRING | Promo code applied |
| `promo_name` | STRING | Promotion name |

### `gwz_ship_17` — Shipping Data

| Column | Type | Description |
|---|---|---|
| `orders_id` | INTEGER | Order identifier |
| `transporter` | STRING | Delivery company |
| `shipping_fee` | FLOAT | Shipping fee charged to customer |
| `log_cost` | FLOAT | Logistics cost |
| `ship_cost` | FLOAT | Shipping cost |

### `gwz_campaign_17` — Advertising Data

| Column | Type | Description |
|---|---|---|
| `date_date` | DATE | Campaign date |
| `ads_cost` | FLOAT | Daily advertising spend |

---

## 🗂️ Data Architecture

This project follows a layered data transformation approach:

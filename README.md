
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

```
gwz_sales_17 (raw — 1.1M rows)
    ↓
gwz_orders (aggregated by order)
    ↓
gwz_orders_operational (+ shipping costs via JOIN)
    ↓
gwz_orders_join (cleaner CTE-based version)
    ↓
gwz_campaign_join (+ ad costs via JOIN, aggregated by day)
```

---

## 🔍 Analysis Steps

**1. Order-Level Aggregation**
Raw row-level sales data (one row per product per order) was aggregated to order level using `GROUP BY orders_id`, summing turnover, margin, and costs.

**2. Shipping Cost Join**
The orders table was joined with the shipping table (`gwz_ship_17`) on `orders_id` to add `shipping_fee` and `operational_cost` (`log_cost + ship_cost`) per order. `LEFT JOIN` was used to retain orders with no shipping record.

**3. Ad Spend Join (CTE approach)**
Two CTEs were used to aggregate orders and campaign data by date independently, then joined on `date_date` to produce a daily P&L view.

**4. Margin Analysis**
Product-level margin was calculated and categorized:

| margin_percent | margin_level |
|---|---|
| < 5% | low |
| 5% – 40% | medium |
| > 40% | high |

**5. Promotion Analysis**
Discount type was classified using both name-based matching and percentage thresholds:

| Rule | promo_type |
|---|---|
| Name contains "DLC" or "DLUO" | Short-lived |
| Discount ≥ 30% | High promotion |
| Discount 10% – 30% | Medium promotion |
| Discount < 10% | Low promotion |

> Note: DLC (Date Limite de Consommation = expiry date) products are categorized separately regardless of discount rate — these are stock-driven markdowns, not strategic promotions.

---

## 📊 Key Metrics

| Metric | Description |
|---|---|
| `turnover` | Revenue collected from customer |
| `margin` | `turnover - purchase_cost` |
| `shipping_fee` | Shipping revenue from customer |
| `operational_cost` | `log_cost + ship_cost` |
| `ads_cost` | Daily advertising spend |
| **True profitability** | `margin + shipping_fee - operational_cost - ads_cost` |

---

## 🛠️ SQL Techniques Used

| Technique | Purpose |
|---|---|
| `GROUP BY` + `SUM()` | Aggregate row-level data to order and day level |
| `LEFT JOIN` | Combine tables while retaining unmatched orders |
| `WITH` (CTE) | Break complex queries into readable named steps |
| Chained CTEs | Feed one CTE's output into the next (3-layer chains) |
| `SAFE_DIVIDE()` | Prevent division-by-zero errors in percentage calculations |
| `UPPER()` + `LIKE` | Case-insensitive text matching for promo name detection |
| `CASE WHEN` | Classify numeric values into business categories |
| `ROUND()` | Format percentage values to 2 decimal places |

---

## 🔧 Tools & Environment

- **Google BigQuery** — SQL query execution and table management
- **Standard SQL** — All queries written in BigQuery-compatible SQL
- **Google Cloud Console** — Dataset and project management

---



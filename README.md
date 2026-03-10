# 🛍️ Customer Shopping Behaviour Analysis
### End-to-End Data Analytics Project | Python · SQL · Power BI

![Python](https://img.shields.io/badge/Python-3.x-3776AB?logo=python&logoColor=white)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-Database-336791?logo=postgresql&logoColor=white)
![Power BI](https://img.shields.io/badge/Power%20BI-Dashboard-F2C811?logo=powerbi&logoColor=black)
![Status](https://img.shields.io/badge/Status-Complete-00c9b1)

---

## 📌 Project Overview

An end-to-end data analytics project analysing **3,900 retail customer transactions** to uncover purchasing patterns, demographic trends, seasonal behaviour, and payment preferences — and translate them into actionable business recommendations.

**Business Question:**
> *"How can the company leverage consumer shopping data to identify trends, improve customer engagement, and optimise marketing and product strategies?"*

**Pipeline:**
```
Raw CSV → Python (EDA + Cleaning + Feature Engineering) → PostgreSQL → SQL Business Queries → Power BI Dashboard
```

---

## 📊 Key Results

| Metric | Value |
|--------|-------|
| Total Revenue | $233,081 |
| Total Transactions | 3,900 |
| Average Order Value | $59.76 |
| Average Review Rating | 3.75 / 5 |
| Best Season (Revenue) | Fall — $60,018 |
| Best Season (Avg Order) | Fall — $61.56 |
| Top Category | Clothing — $104,264 (44.7% of revenue) |
| Loyal Customers | 80% (3,116 customers with 10+ purchases) |
| Subscription Rate | 27% (1,053 customers) |
| Discount-Resilient Customers | 839 (used discount + spent above avg) |
| Top Payment Method | PayPal (677 transactions) |
| Top Revenue State | Montana ($5,784) |

---

## 🗂️ Repository Structure

```
Customer_Behaviour_Analysis/
├── customer_shopping_behavior.csv                       # Raw dataset (3,900 rows, 18 columns)
├── Customer_Shopping_Behaviour_Analysis.ipynb           # Python: EDA, cleaning, feature engineering, visualisations, SQL load
├── customer_shoping_behavior_1.sql                      # 20+ SQL business queries
├── Customer_Shopping_Behaviour_Analysis_DASHBOARD.pbix  # Power BI interactive dashboard
└── README.md
```

---

## 🧰 Tech Stack

| Layer | Tool | Purpose |
|-------|------|---------|
| Data Wrangling | Python — Pandas, NumPy | Cleaning, feature engineering, EDA |
| Visualisation | Python — Matplotlib | 7 chart types across 5 analysis themes |
| Database Connection | SQLAlchemy + psycopg2 | Load cleaned data to PostgreSQL |
| Database | PostgreSQL | Structured storage for SQL analysis |
| Business Analysis | SQL | 20+ queries — CTEs, window functions, subqueries |
| Dashboard | Microsoft Power BI Desktop | Interactive reporting with custom dark theme |

---

## 📋 Dataset

| Attribute | Detail |
|-----------|--------|
| Source | Customer Shopping Behaviour — Retail |
| Rows | 3,900 transactions |
| Columns | 18 (age, gender, category, purchase amount, season, payment method, etc.) |
| Nulls | 37 in `Review Rating` — imputed with category-wise median |
| Redundancy | `promo_code_used` = `discount_applied` in 100% of rows — dropped |

---

## 🔬 Python: Data Preparation

### Cleaning Steps

```python
import pandas as pd

df = pd.read_csv("customer_shopping_behavior.csv")

# 1. Impute Review Rating nulls with category-wise median
df["Review Rating"] = df.groupby("Category")['Review Rating'].transform(
    lambda x: x.fillna(x.median())
)

# 2. Standardise column names
df.columns = df.columns.str.lower().str.replace(" ", "_")
df = df.rename(columns={'purchase_amount_(usd)': 'purchase_amount'})

# 3. Validate and drop redundant column
(df['promo_code_used'] == df['discount_applied']).all()  # Returns True → 100% match confirmed
df = df.drop('promo_code_used', axis=1)
```

### Feature Engineering

```python
# Age group — quartile-based equal-sized segments
labels = ['Young Adult', 'Adult', 'Middle-Age', 'Senior']
df['age_group'] = pd.qcut(df['age'], q=4, labels=labels)

# Purchase frequency mapped to numeric days
frequency_mapping = {
    'Weekly': 7, 'Bi-Weekly': 14, 'Fortnightly': 14,
    'Monthly': 30, 'Quarterly': 90, 'Every 3 Months': 90, 'Annually': 365
}
df['purchase_frequency_days'] = df['frequency_of_purchases'].map(frequency_mapping)
```

### Load to PostgreSQL

```python
from sqlalchemy import create_engine
import urllib.parse

safe_password = urllib.parse.quote_plus("your_password")
engine = create_engine(
    f"postgresql+psycopg2://postgres:{safe_password}@localhost:5432/customer_behaviour_analysis"
)
df.to_sql("customer", engine, if_exists="replace", index=False)
```

---

## 📈 Python Visualisations (7 Charts)

| Chart | What It Shows |
|-------|---------------|
| Dual-axis bar + line | Seasonal revenue (bars) + avg order value (line) per season |
| Side-by-side bar + donut | Gender total revenue + customer split (68% male / 32% female) |
| Horizontal bar + bar | Revenue by category + avg review rating by category |
| Horizontal bar (x2) | Payment method distribution + avg spend by shipping type |
| Donut + bar + horizontal bar | Loyalty segments + subscriber vs non-subscriber avg spend + purchase frequency |
| Side-by-side bar | Discount impact on avg spend + top 10 most purchased items |

---

## 🗃️ SQL: Business Queries (20+)

| # | Business Question | SQL Technique |
|---|-------------------|---------------|
| 1 | Revenue by gender | GROUP BY + SUM |
| 2 | Discount users who spent above average | Scalar subquery in WHERE |
| 3 | Top 5 products by review rating | GROUP BY + AVG + ROUND |
| 4 | Avg spend: Standard vs Express shipping | WHERE IN + GROUP BY |
| 5 | Subscriber vs non-subscriber revenue & avg spend | GROUP BY + COUNT + SUM |
| 6 | Top 5 products by discount usage rate (%) | CASE WHEN + percentage calc |
| 7 | Customer segmentation: New / Returning / Loyal | **CTE + CASE WHEN** |
| 8 | Top 3 products within each category | **CTE + ROW_NUMBER() OVER (PARTITION BY)** |
| 9 | Revenue, orders, avg order by season | GROUP BY + multiple aggregations |
| 10 | Best category per season | **CTE + RANK() OVER (PARTITION BY)** |
| 11 | Revenue & orders by age group | CASE WHEN ranges + GROUP BY |
| 12 | Most popular category per age group | Nested CASE WHEN + RANK() window |
| 13 | Payment method: revenue, avg spend, % of total | **SUM(COUNT(*)) OVER()** |
| 14 | Shipping preference by subscription status | GROUP BY 2 columns |
| 15 | Express shipping customers with 5+ purchases | Multi-condition WHERE filter |
| 16 | High-value customers above 90th percentile | **PERCENTILE_CONT(0.90) WITHIN GROUP** |
| 17 | Loyalty segment vs avg spend and rating | CASE WHEN + multiple aggregations |
| 18 | Gender × Category revenue matrix | GROUP BY 2 columns |
| 19 | Purchase frequency revenue analysis | GROUP BY + AVG + SUM |
| 20 | Cumulative revenue % by category | **SUM() OVER(ORDER BY) running total** |
| 21 | Non-subscribers with loyal buying behaviour | Subquery + multiple WHERE conditions |

**Sample — Top 3 Products per Category (Window Function):**
```sql
WITH item_counts AS (
    SELECT item_purchased, category,
           COUNT(customer_id) AS total_orders,
           ROW_NUMBER() OVER(PARTITION BY category ORDER BY COUNT(customer_id) DESC) AS item_rank
    FROM customer
    GROUP BY category, item_purchased
)
SELECT item_rank, category, item_purchased, total_orders
FROM item_counts
WHERE item_rank <= 3;
```

**Sample — High-Value Customer Identification (90th Percentile):**
```sql
WITH percentile AS (
    SELECT PERCENTILE_CONT(0.90) WITHIN GROUP (ORDER BY purchase_amount) AS p90
    FROM customer
)
SELECT c.customer_id, c.age, c.gender, c.category,
       c.purchase_amount, c.previous_purchases, c.subscription_status
FROM customer c, percentile p
WHERE c.purchase_amount >= p.p90
ORDER BY c.purchase_amount DESC;
```

---

## 📊 Power BI Dashboard


<img width="1239" height="736" alt="Dashboard_Screenshot_Final" src="https://github.com/user-attachments/assets/3b685900-8b44-41e5-9acd-796685595169" />


Interactive dark-themed dashboard with a custom JSON theme, 4 KPI cards, 5 visuals, and 5 slicers.


| Visual | What It Shows |
|--------|---------------|
| KPI Card | Total Revenue — $233,081 |
| KPI Card | Total Customers — 3,900 |
| KPI Card | Avg Order Value — $59.76 |
| KPI Card | Avg Review Rating — 3.75 |
| Donut Chart | Subscription Status — Yes 27% / No 73% |
| Column Chart | Revenue by Season — Fall leads at $60,018 |
| Pie Chart | Revenue by Gender — Male $158K / Female $75K |
| Bar Chart | Revenue by Category — Clothing dominant at $104K |
| Bar Chart | Revenue by Age Group — Young Adult segment leading |

**Slicers:** Subscription Status · Gender · Category · Season · Shipping Type

---

## 💡 Key Findings & Business Recommendations

### 1. Summer Needs a Promotional Push
Summer has the lowest avg order value ($58.41 vs Fall's $61.56 — a $3.15 gap). A targeted bundle or discount campaign in Summer can close this gap and add ~$4K in seasonal revenue.

### 2. Female Customer Base Is Under-Represented
Female customers spend nearly the same per transaction ($59.47 vs male $59.83), but represent only 32% of all transactions. The revenue gap is an **acquisition problem, not a product problem** — the opportunity is in growing female customer count, not changing the product mix.

### 3. Convert Loyal Non-Subscribers
80% of customers are loyal (10+ purchases) but only 27% are subscribed. Approximately 2,116 loyal customers are buying repeatedly without being on the subscription programme — the single biggest untapped revenue opportunity.

### 4. Upsell Outerwear with Accessories
Outerwear has the lowest avg order value ($57.17) and lowest volume (324 orders). Bundling it with high-demand Accessories (Jewelry, Belts) in Fall/Winter campaigns can lift basket size significantly.

### 5. Target the Discount-Resilient Segment
839 customers used a discount AND still spent above the overall average ($59.76). These customers respond to promotional triggers but are not price-limited — ideal for premium product campaigns that don't require deep discounts.

### 6. Protect Fast-Shipping Customers
2-Day Shipping users have the highest avg order value ($60.73) in the dataset. These are the highest-value customers — prioritising their checkout and fulfilment experience protects your most profitable segment.

---


## 👤 Author

**Ganesh Jag**
GitHub: [@GaneshJag31](https://github.com/GaneshJag31)

---

*End-to-end Data Analytics portfolio project — Python · SQL · Power BI*

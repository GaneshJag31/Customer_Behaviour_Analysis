# 🛍️ Customer Behaviour Analysis
### End-to-End Data Analytics Project | SQL · Python · Power BI

![Python](https://img.shields.io/badge/Python-3.x-blue?logo=python)
![SQL](https://img.shields.io/badge/SQL-PostgreSQL%20%7C%20MySQL-orange?logo=postgresql)
![Power BI](https://img.shields.io/badge/PowerBI-Dashboard-yellow?logo=powerbi)
![Status](https://img.shields.io/badge/Status-Complete-brightgreen)

---

## 📌 Project Overview

A complete, end-to-end data analytics project analysing **3,900 retail customer transactions** to uncover purchasing patterns, demographic trends, seasonal behaviour, and payment preferences.

The project simulates a real-world analyst workflow:
**Raw Data → Python EDA & Cleaning → SQL Database → Business Queries → Power BI Dashboard**

---

## 🎯 Business Objective

> *"How do customer demographics, seasons, and purchase behaviour interact — and what can a retail business do about it?"*

Key questions answered:
- Which seasons and categories drive the most revenue?
- How do subscribers vs non-subscribers differ in spend?
- What are the top-performing products and customer segments?
- Which states and age groups are the most active buyers?
- How does payment method correlate with purchase value?

---

## 🗂️ Repository Structure

```
Customer_Behaviour_Analysis/
├── customer_shopping_behavior.csv              # Raw dataset (3,900 rows, 18 columns)
├── Customer_Shopping_Behavior_Analysis.ipynb   # Python: EDA, cleaning, SQL load
├── customer_behavior_sql_queries.sql           # 25+ SQL business queries
├── customer_behavior_dashboard.pbix            # Power BI interactive dashboard
└── README.md                                   # Project documentation
```

---

## 🧰 Tech Stack

| Layer | Tool |
|-------|------|
| Data Wrangling | Python — Pandas, NumPy |
| Visualisation (EDA) | Matplotlib, Seaborn |
| Database Connection | SQLAlchemy |
| Database | PostgreSQL / MySQL (configurable) |
| Business Analysis | SQL (CTEs, Window Functions, GROUP BY) |
| Dashboard | Microsoft Power BI Desktop |

---

## 📊 Dataset

| Attribute | Detail |
|-----------|--------|
| Source | Customer Shopping Behaviour (Retail) |
| Rows | 3,900 transactions |
| Columns | 18 (age, gender, category, amount, season, payment, etc.) |
| Target | Purchase Amount (USD) + behavioural attributes |

**Key columns:** `Age`, `Gender`, `Category`, `Purchase Amount (USD)`, `Season`, `Subscription Status`, `Payment Method`, `Review Rating`, `Previous Purchases`, `Promo Code Used`

---

## 🔬 Project Workflow

### Step 1 — Python: EDA & Cleaning
```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt

df = pd.read_csv('customer_shopping_behavior.csv')

# Shape, nulls, dtypes
print(df.shape)        # (3900, 18)
print(df.isnull().sum())

# Distribution of purchase amounts
df['Purchase Amount (USD)'].hist(bins=30)
plt.title('Purchase Amount Distribution')
plt.show()
```

**Cleaning steps performed:**
- Checked and confirmed zero null values
- Standardised categorical columns (Gender, Season, Category)
- Validated data types — converted booleans and numerics correctly
- Removed any duplicate rows

### Step 2 — Load to SQL Database
```python
from sqlalchemy import create_engine

# PostgreSQL
engine = create_engine("postgresql+psycopg2://user:password@localhost:5432/customer_db")

# Or MySQL
# engine = create_engine("mysql+pymysql://user:password@localhost:3306/customer_db")

df.to_sql('customer_behaviour', engine, if_exists='replace', index=False)
print("Data loaded successfully.")
```

### Step 3 — SQL Business Queries
Open `customer_behavior_sql_queries.sql` and run against your connected database.

**Sample query:**
```sql
-- Seasonal Revenue Breakdown
SELECT season,
       COUNT(*) AS total_orders,
       ROUND(SUM(purchase_amount_usd), 2) AS total_revenue,
       ROUND(AVG(purchase_amount_usd), 2) AS avg_order_value
FROM customer_behaviour
GROUP BY season
ORDER BY total_revenue DESC;
```

### Step 4 — Power BI Dashboard
Open `customer_behavior_dashboard.pbix` in Power BI Desktop.
- Connect to your local SQL database using the matching credentials
- Refresh the data source
- Explore the interactive dashboard with slicers for Season, Gender, Category, and Subscription Status

---

## 📈 Key Insights

| Theme | Finding |
|-------|---------|
| **Season** | Fall & Winter drive the highest revenue; Spring is the weakest quarter |
| **Gender** | Male customers = ~68% of transactions; female customers show higher avg order value |
| **Age** | 26–35 is the most active segment; 51–70 customers give the highest review ratings |
| **Category** | Clothing leads in volume (45%+); Accessories has the highest revenue per unit |
| **Subscriptions** | Subscribers spend 18–22% more per transaction than non-subscribers |
| **Payment** | Credit Card (38%) and PayPal (27%) dominate; PayPal users have higher avg spend |
| **Promo Codes** | Subscribers use promo codes at 2x the rate of non-subscribers |

---

## 💡 Business Recommendations

1. **Spring Promotions** — Introduce targeted discounts to lift the lowest-performing season
2. **Female Subscriber Campaigns** — Higher per-transaction value makes this a high-ROI target segment
3. **Cross-sell Accessories** — Bundle with Clothing purchases to maximise revenue-per-unit
4. **Loyalty Tiers** — Reward customers with 10+ purchases to reduce churn
5. **Age-Targeted Marketing** — Footwear for 18–25; quality messaging for 51–70

---

## ⚙️ Setup & Run

### Prerequisites
```bash
pip install pandas numpy matplotlib seaborn sqlalchemy psycopg2-binary
```

### Steps
```bash
# 1. Clone the repository
git clone https://github.com/GaneshJag31/Customer_Behaviour_Analysis.git
cd Customer_Behaviour_Analysis

# 2. Open the Jupyter notebook
jupyter notebook Customer_Shopping_Behavior_Analysis.ipynb

# 3. Run all cells (update DB credentials in the SQL connection cell)

# 4. Open customer_behavior_sql_queries.sql in your SQL client

# 5. Open customer_behavior_dashboard.pbix in Power BI Desktop
#    → Update data source credentials → Refresh
```

---

## 📄 Project Report

A detailed PDF project report with full insights, methodology, data dictionary, and business recommendations is included in this repository.

---

## 👤 Author

**Ganesh Jag**
- GitHub: [@GaneshJag31](https://github.com/GaneshJag31)

---

## 📝 License

This project is open source and available under the [MIT License](LICENSE).

---

*Built as a Data Analyst portfolio project — demonstrating end-to-end analytics skills with Python, SQL, and Power BI.*

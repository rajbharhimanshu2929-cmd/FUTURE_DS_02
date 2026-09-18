# FUTURE_DS_02 — Customer Retention & Churn Analysis

**Track:** Data Science & Analytics (DS) — Task 2
**Author:** _<your name>_
**Tools used:** Python (pandas, matplotlib, seaborn), SQL (SQLite), Jupyter Notebook

---

## 📌 Task

Analyze customer data for a subscription-based business to identify churn patterns, key
retention drivers, and customer lifetime trends.

**Deliverable:** A retention analysis dashboard/report highlighting churn reasons, retention
trends, and actionable recommendations to reduce customer loss.

## 🗂️ Repository Structure

```
FUTURE_DS_02/
├── data/
│   └── customers.csv                     # 5,000-row synthetic subscription customer dataset
├── notebooks/
│   └── 01_churn_retention_analysis.ipynb # full analysis, step by step, with charts + narrative
├── sql/
│   ├── schema.sql                        # table definition
│   ├── analysis_queries.sql              # 9 business-question SQL queries
│   └── churn.db                          # SQLite database preloaded with the dataset
├── src/
│   ├── generate_data.py                  # synthetic data generator (documents all assumptions)
│   ├── analysis.py                       # reusable analysis functions (KPIs, cohorts, drivers)
│   └── dashboard.py                      # builds the static dashboard image below
├── images/
│   └── churn_retention_dashboard.png     # one-page visual dashboard (see below)
└── README.md
```

## 📊 Dashboard

![Customer Retention & Churn Dashboard](images/churn_retention_dashboard.png)

## 🧾 Dataset

Since no client dataset was provided for this task, a **realistic synthetic dataset** of 5,000
subscription customers was generated (`src/generate_data.py`) with built-in, documented patterns
that mirror real-world subscription-business behavior:

- Plan tier (Basic / Standard / Premium), contract type (Monthly / Annual), acquisition channel,
  region, age
- Engagement signals: support tickets, NPS score, average logins/month
- Outcome fields: `churned`, `churn_date`, `churn_reason`, `tenure_months`,
  `customer_lifetime_value_usd`
- Churn probability is deliberately modeled to depend on plan, contract type, engagement, and
  NPS — so the analysis below reflects genuine, interpretable relationships rather than random
  noise. All modeling assumptions are documented as comments in `generate_data.py`.

> To use this project with a real dataset, replace `data/customers.csv` with your own file that
> matches the schema in `sql/schema.sql`, then re-run the notebook/scripts.

## 🔎 Methodology

1. **Data generation / ingestion** — `src/generate_data.py`
2. **Exploratory & cohort analysis** — `notebooks/01_churn_retention_analysis.ipynb`
   - Overall churn KPIs
   - Cohort retention curves (% of each signup-month cohort still active over time)
   - Churn rate by plan, contract type, and acquisition channel
   - Top stated reasons for cancellation
   - Customer Lifetime Value (CLV) by plan
   - Churn driver ranking (correlation-based)
3. **SQL layer** — `sql/analysis_queries.sql` reproduces the core KPIs and segment breakdowns
   directly in SQL against `sql/churn.db`, including a query that generates a **high-risk active
   customer list** for a retention campaign.
4. **Dashboard** — `src/dashboard.py` renders the full one-page dashboard image above.

## 💡 Key Findings

- Overall churn rate is **~23%**, with **~$130K** in CLV lost to churn in this dataset.
- Churn is heavily front-loaded — most cancellations happen in a customer's **first 1–3 months**.
- **Basic plan** and **Monthly contract** customers churn at roughly **2x** the rate of
  Premium/Annual customers.
- **Paid Social** is the highest-churn acquisition channel; **Referral** and **Direct** are the
  stickiest.
- **Tenure, NPS score, and login frequency** are the strongest predictors of retention.
- **Billing disputes, low engagement, and technical issues** together account for over 40% of
  stated cancellation reasons.

## ✅ Recommendations

1. Build a structured **first-90-days onboarding flow** targeted at Basic/Monthly customers.
2. Trigger a **proactive re-engagement campaign** for customers with <4 logins/month.
3. **Audit the billing experience** — billing disputes are a top-3 stated churn reason and are
   typically a fixable process issue.
4. **Rebalance acquisition spend** away from Paid Social toward Referral/Direct.
5. **Incentivize annual contracts** for engaged Standard/Premium customers.

## ▶️ How to Run

```bash
# 1. Install dependencies
pip install pandas numpy matplotlib seaborn

# 2. Regenerate the dataset (optional — customers.csv is already included)
python src/generate_data.py

# 3. Run the analysis
python src/analysis.py

# 4. Rebuild the dashboard image
python src/dashboard.py

# 5. Or open the full notebook
jupyter notebook notebooks/01_churn_retention_analysis.ipynb

# 6. Run SQL queries directly
sqlite3 sql/churn.db < sql/analysis_queries.sql
```

## 🧠 Skills Demonstrated

Retention analysis · Cohort analysis · Customer lifetime value (CLV) modeling ·
SQL querying · Data visualization · Insight-driven decision making

---

*Part of the Data Science & Analytics track — Task 2. See also: [FUTURE_DS_03 — Marketing Funnel
& Conversion Performance Analysis](../FUTURE_DS_03).*

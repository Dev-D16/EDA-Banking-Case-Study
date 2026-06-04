[README.md](https://github.com/user-attachments/files/28599693/README.md)
# 🏦 Banking Risk Analysis & Interactive Dashboard

An end-to-end exploratory data analysis (EDA) project on a retail banking
portfolio, culminating in an **interactive risk dashboard** built with
`ipywidgets`. The notebook investigates what drives a client's **Risk
Weighting (1 = Low → 5 = High)** and surfaces the high-risk profile the
bank should monitor.

---

## 📋 Table of Contents

- [Overview](#-overview)
- [Dataset](#-dataset)
- [Tech Stack](#-tech-stack)
- [Notebook Walkthrough](#-notebook-walkthrough)
- [Interactive Dashboard](#-interactive-dashboard)
- [Key Findings](#-key-findings)
- [Getting Started](#-getting-started)
- [Project Structure](#-project-structure)
- [Future Improvements](#-future-improvements)

---

## 🔎 Overview

A bank wants to understand **which customers are most likely to default** and
**which segments pose the largest debt exposure risk**. This notebook
answers those questions by:

1. Cleaning and engineering new financial features (Total Debt, Total Liquid
   Assets, Debt-to-Income Ratio).
2. Running univariate, bivariate, and correlation analyses on the client
   portfolio.
3. Comparing high-risk (Risk = 5) vs. low-risk (Risk = 1) customer profiles.
4. Delivering an **interactive filterable dashboard** for non-technical
   stakeholders to slice the data by Risk Level, Occupation, and Income
   Range.

---

## 📊 Dataset

| Item | Detail |
|------|--------|
| **File** | `Banking.csv` |
| **Rows** | ~2,999 clients |
| **Location in notebook** | `/content/Banking.csv` (Colab default) |

### Key Columns

| Column | Type | Description |
|--------|------|-------------|
| `Age` | int | Client age |
| `Estimated Income` | float | Reported/estimated annual income |
| `Risk Weighting` | int (1–5) | **Target** — internal bank risk score |
| `Bank Loans` | float | Outstanding loan balance |
| `Credit Card Balance` | float | Outstanding credit-card balance |
| `Bank Deposits` | float | Deposit balances |
| `Saving Accounts` | float | Savings balances |
| `Checking Accounts` | float | Checking balances |
| `Superannuation Savings` | float | Retirement / super balance |
| `Properties Owned` | int | Number of properties (collateral) |
| `Occupation` | cat | Job title (~190 unique occupations) |
| `Fee Structure` | cat (`Low`/`Mid`/`High`) | Client fee tier |
| `Amount of Credit Cards` | int | Number of cards held |
| `Joined Bank` | date | Date client became a customer |

### Engineered Features

- `Total Debt = Bank Loans + Credit Card Balance`
- `Total Liquid Assets = Bank Deposits + Saving Accounts + Checking Accounts + Superannuation Savings`
- `Debt_to_Income_Ratio = Total Debt / Estimated Income` (with divide-by-zero guard)

---

## 🛠 Tech Stack

- **Python 3**
- **Pandas / NumPy** — data wrangling
- **Matplotlib / Seaborn** — static visualizations
- **ipywidgets + IPython.display** — interactive dashboard
- **Jupyter / Google Colab** — runtime

No external ML models are trained; this is a **pure analytics** notebook.

---

## 🚶 Notebook Walkthrough

| # | Cell | What it does |
|---|------|--------------|
| 0 | Imports | Loads NumPy, Pandas, Matplotlib, Seaborn |
| 1–2 | Load + Inspect | Reads `Banking.csv`, prints head & `df.info()` |
| 3–4 | Null check + `describe()` | Confirms no missing values, prints summary stats |
| 5 | Markdown | Introduces **Univariate Analysis** |
| 6–9 | Feature engineering & column setup | Builds `Total Debt`, `Total Liquid Assets`, `Debt_to_Income_Ratio`; prunes high-cardinality ID columns from plots |
| 10 | Count plot | Credit cards owned by gender |
| 11 | Target distribution | Counts of each `Risk Weighting` level (1–5) |
| 12 | Box plot | Income distribution per risk level (log scale) |
| 13 | Box plot | Total Debt distribution per risk level (log scale) |
| 14 | Crosstab + count plot | Properties Owned vs Risk Weighting |
| 15 | Count plot | Fee Structure vs Risk Weighting |
| 16 | Heatmap | Full correlation matrix of the 15 numeric features |
| 17–18 | High-risk vs low-risk profile | Side-by-side means for Income, Debt, Assets, Properties |
| 19 | Analyst summary | Bullet-point conclusions for the bank |
| **20** | **Interactive Dashboard** | The `ipywidgets` filterable dashboard (see below) |
| 21 | Output | Live rendering of the dashboard |

---

## 🎛 Interactive Dashboard

Built with **`ipywidgets.interactive_output`**, the dashboard lets users
filter the entire client base on three dimensions simultaneously:

| Control | Widget | Effect |
|---------|--------|--------|
| **Risk Level** | `SelectMultiple` (1–5) | Keep only the selected risk tiers |
| **Occupation** | `Dropdown` (~190 jobs + `All Occupations`) | Drill into a single profession |
| **Income Range** | `IntRangeSlider` (`min`–`max` of income, step 5,000) | Filter by income band |

For every filter change, the dashboard recomputes and renders:

- 📌 **Summary metrics** — Total Clients, Avg Risk Score, Total Debt Exposure
- 📊 **Bar chart** — Distribution of Risk Levels within the filtered slice
- 🔵 **Scatter plot (log–log)** — Estimated Income vs Total Debt, color-coded
  by Risk Weighting (red = high risk, green = low risk)
- 📋 **Data table preview** — Top 10 matching clients (Name, Occupation,
  Risk, Income, Total Debt)

The function lives in a single `update_dashboard()` callback so any widget
change automatically re-filters and re-plots.

---

## 💡 Key Findings

> These are the analyst takeaways baked into the notebook (see Cell 19).

1. **Income ↔ Risk** — Lower income bands are disproportionately represented
   in higher Risk Weighting tiers; the box plot on log-scaled income
   confirms the spread narrows as risk rises.
2. **Debt Leverage** — High `Total Debt` clusters at risk levels 4 and 5;
   the scatter plot in the dashboard makes the concentration visible in
   one glance.
3. **Collateral Matters** — Clients with **0 properties owned** are
   over-represented in the High Risk group, suggesting collateral is a
   protective factor.
4. **Fee Structure Signal** — The `Low`-fee segment is skewed toward
   higher-risk clients — possible cross-sell / re-pricing opportunity.
5. **Headline numbers from the dashboard sample run** (2,999 clients):
   - Avg Risk Score ≈ **2.25 / 5**
   - Total Debt Exposure ≈ **$1.78 B**

---

## 🚀 Getting Started

### 1. Clone / download

```bash
git clone <your-repo-url>
cd banking-risk-analysis
```

### 2. Install dependencies

```bash
pip install pandas numpy matplotlib seaborn ipywidgets
```

> `ipywidgets` is needed for the dashboard at the bottom of the notebook.
> If you run in **JupyterLab**, also run:
> `jupyter labextension install @jupyter-widgets/jupyterlab-manager`

### 3. Add the data

Place `Banking.csv` in the path the notebook expects, or update Cell 1:

```python
df = pd.read_csv('Banking.csv')   # change to your local path
```

### 4. Run the notebook

```bash
jupyter notebook 5c2c403a__03ca9614-31df-4bff-b648-f97e9127dad8.ipynb
```

Or upload to **Google Colab** and run as-is (the path `/content/Banking.csv`
is Colab's default upload location).

---

## 🗂 Project Structure

```
.
├── README.md
├── 5c2c403a__03ca9614-31df-4bff-b648-f97e9127dad8.ipynb   # main notebook
└── Banking.csv                                            # input data (you provide)
```

---

## 🔭 Future Improvements

- **Predictive model** — Train a classifier (e.g., Random Forest, XGBoost)
  to *predict* `Risk Weighting` from the engineered features.
- **SHAP / feature importance** — Quantify which features drive risk the most.
- **Export dashboard** — Convert the `ipywidgets` UI into a **Streamlit** or
  **Voila** app for non-Jupyter users.
- **Time-series view** — Use `Joined Bank` to study risk evolution as the
  portfolio ages.
- **Data quality flags** — Add automated checks for outliers, negative
  balances, and unrealistic `Debt_to_Income_Ratio` values.
- **Segmentation** — K-Means / RFM-style clustering to find natural client
  segments beyond the single risk score.

---

## 📝 License & Credits

- Data: `Banking.csv` (not included in this repo — provide your own).
- Analysis & dashboard: built as an exploratory analytics exercise.
- Feel free to fork, adapt, and extend.

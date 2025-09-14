# Marketing Mix Modeling with Mediation

## 📌 Project Overview

This project implements a **Marketing Mix Modeling (MMM)** framework with a mediation layer.
Focus on the causal pathway where **social/display ads (TikTok, Instagram, Snapchat)** influence **Google Spend** (mediator), which then drives **Revenue**.
A **two-stage regression approach** ensures causality is respected and avoids post-treatment bias.

The notebook provides:

* Data preparation with seasonality and transformations
* A causal DAG framing the mediation
* Two-stage regression (ElasticNet and LightGBM)
* Validation with blocked time-series cross-validation
* Residual and sensitivity analyses
* Actionable marketing insights

---

## 📂 Repository Structure

```
MMM_Mediation_Assessment/
├── MMM_Mediation_Model.ipynb   # Main Jupyter notebook (full analysis)
├── weekly_data.csv             # Weekly dataset
├── requirements.txt            # Dependencies for reproducibility
├── README.md                   # Project documentation
└── .gitignore                  # Ignore venv, checkpoints, etc.

```

---

## ⚙️ Setup Instructions

### 1. Clone the repo

```bash
git clone <your-repo-link>
cd <your-repo-name>
```

### 2. Install dependencies

```bash
pip install -r requirements.txt
```

Minimal requirements:

* pandas
* numpy
* matplotlib
* seaborn
* scikit-learn
* lightgbm
* causalgraphicalmodels

### 3. Run the notebook

```bash
jupyter notebook notebook.ipynb
```

---

## 🚀 Workflow

### 1. Data Preparation

* Parsed `week` as datetime.
* Added features: week-of-year, year, month, Fourier terms for weekly seasonality.
* Replaced zero spends with a small positive value, log-transformed spends and revenue.
* Converted `promotions` to binary.
* Log-transformed followers if skewed.
* Checked for missing values.

### 2. Causal Framing

* Built a **Directed Acyclic Graph (DAG)**:

  * Social/Display → Google Spend → Revenue.
  * Direct effects from Social/Display → Revenue also included.
* Designed model to block backdoor paths and avoid leakage.

### 3. Two-Stage Regression

* **Stage 1**: Predicted `log(Google Spend)` from TikTok, Instagram, Snapchat, and controls.
* **Stage 2**: Predicted `log(Revenue)` using predicted Google spend, price, promotions, email, SMS, followers, and seasonality controls.
* Models: **ElasticNetCV** (regularized regression for interpretability) and **LightGBM** (nonlinear benchmark).

### 4. Validation & Diagnostics

* Blocked **time-series cross-validation** to respect temporal ordering.
* Reported **RMSE and R²** for each fold.
* Residual analysis: checked bias/seasonality in errors.
* Sensitivity analysis: varied price & promotions, observed revenue response.

### 5. Insights & Recommendations

* **Price elasticity is negative** → higher price reduces revenue.
* **Promotions have a positive effect** on revenue.
* **Social/Display ads indirectly boost revenue via Google Spend** (mediated effect).
* Risks: multicollinearity, diminishing returns, unobserved confounding.
* Recommendation: optimize balance between social/display and search ads while monitoring promo costs.

---

## 📊 Example Outputs

* Revenue vs. Media Spend over time (line plot).
* Residuals over time (diagnostics).
* Sensitivity curve showing impact of price changes on revenue.

---

## ✍️ Author

Prepared as part of **Marketing Mix Modeling Assessment**.

---


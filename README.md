# 📈 Portfolio Optimisation with Linear Regression (CAPM & Quadratic Programming)

This project applies the **Capital Asset Pricing Model (CAPM)** and **Quadratic Programming** techniques to perform **portfolio optimisation**.  
The main objective is to determine the **optimal portfolio weights** that minimise risk for a given level of expected return, and to visualise the **efficient frontier** illustrating the risk-return trade-off.

---

## 🧠 Overview

Modern Portfolio Theory (MPT) proposes that investors can achieve optimal diversification by balancing **expected return** and **risk**.  
In this notebook, we estimate **expected returns** and **systematic risk** using **CAPM linear regression**, and solve a **quadratic programming** optimisation problem to build portfolios that lie on the **efficient frontier**.

---

## ⚙️ Methodology

### 1. Data Collection
- **Data Source:** Yahoo Finance via the `yfinance` library  
- **Period:** 5 years of daily data  
- **Assets:**  AAPL, AMT, AMZN, GOOGL, JPM, LIN, NEE, PG, RTX, UNH, XOM

- **Benchmark:** S&P 500 Index (`^GSPC`)  
- **Risk-Free Rate:** Based on annualised return of short-term government securities (252 trading days/year).

---

### 2. CAPM Linear Regression
For each stock *i*, excess returns were regressed on market excess returns:

\[
R_i - R_f = \alpha_i + \beta_i (R_m - R_f) + \varepsilon_i
\]

Where:
- \( R_i \): Asset return  
- \( R_m \): Market return  
- \( R_f \): Risk-free rate  
- \( \alpha_i, \beta_i \): CAPM regression coefficients  
- \( \varepsilon_i \): Idiosyncratic error  

Using the **StatsModels** library, we estimated:
- **Alpha (α):** Excess return over expected market return  
- **Beta (β):** Market sensitivity  
- **R²:** Goodness of fit  
- **Idiosyncratic variance:** Residual variance of the model  

---

### 3. Covariance Matrix Construction
The asset covariance matrix was built as the sum of:
- Systematic risk (via betas and market variance)  
- Idiosyncratic risk (diagonal residual variance terms)

\[
\Sigma = \beta \beta^T \sigma_m^2 + D
\]

---

### 4. Portfolio Optimisation
The optimisation problem was defined as a **Quadratic Program (QP)** using `cvxopt`:

\[
\text{Minimise } \frac{1}{2} w^T \Sigma w
\]

Subject to:
- \( r^T w = r_{target} \) (target return constraint)  
- \( \sum w_i = 1 \) (fully invested portfolio)  
- \( w_i \ge 0 \) (no short-selling)

---

### 5. Efficient Frontier
By iterating over 20 target returns between the minimum and maximum expected returns, we computed the **efficient frontier**, which visualises the trade-off between risk and return.

---

## 📊 Results

### CAPM Regression Summary
- **High-Beta (Growth) Stocks:** AAPL, AMZN, GOOGL, JPM  
- **Low-Beta (Defensive) Stocks:** PG, NEE, UNH  
- **All β p-values < 0.001**, confirming significant relationships with the market.  
- **Positive Alpha Stocks:** AAPL, XOM (slight outperformance vs. CAPM expectations)

### Portfolio Insights
| Portfolio Type | Key Allocations | Characteristics |
|----------------|-----------------|-----------------|
| Low-Risk | PG (47%), LIN (12%), UNH (11%) | Stable, defensive portfolio |
| Medium-Risk | Balanced exposure | Mix of growth and stability |
| High-Risk | AAPL (36%), GOOGL (28%), JPM (18%) | High-return, volatile portfolio |

### Efficient Frontier
- Concave upward curve showing optimal portfolios.  
- **Minimum-risk portfolio:** Concentrated in defensive assets.  
- **Maximum Sharpe portfolio:** Balanced mix, best risk-adjusted return.  
- **High-risk portfolios:** Dominated by growth assets with higher volatility.

---

## 💡 Discussion

- Small changes in expected returns significantly shift optimal weights.  
- The efficient frontier demonstrates clear diversification benefits.  
- Conservative investors may prefer low-risk portfolios; aggressive investors can target higher-return portfolios near the right end of the frontier.

---

## 🧩 Technologies Used

| Category | Libraries |
|-----------|------------|
| Data Handling | `pandas`, `numpy` |
| Financial Data | `yfinance` |
| Regression Analysis | `statsmodels` |
| Optimisation | `cvxopt` |
| Visualisation | `matplotlib`, `seaborn` |

---


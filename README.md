# 📊 Spread Locator: A Statistical Distribution Analysis Model

> **Mathematics & Advanced Statistics | Project PR #3**
> Statistical distribution analysis of e-commerce customer transactions using Python.

---

## 📌 Project Overview

An e-commerce platform wants to understand customer purchase behaviour from its **daily transaction amounts**. Management asked three things:

1. Do transactions follow known statistical distributions?
2. How should skewed transaction data be handled?
3. What probability insights can be derived (e.g. how likely is a transaction above ₹5,000)?

This project answers these questions by fitting and testing probability distributions, running normality diagnostics (Q-Q plot), applying a Box-Cox transform, and computing Z-score based probabilities.

## 🗂️ Repository Structure

```
Spread-Locator/
├── Spread_Locator_Analysis.ipynb        # Part B: full analysis notebook (code + charts + interpretations)
├── spread_locator_analysis.py           # Same analysis as a runnable Python script
├── Spread_Locator_Theory_Concepts.pdf   # Part A: theory notes with definitions
├── spread_locator_dataset.csv           # Dataset (220 transactions)
├── requirements.txt
└── README.md
```

All charts are embedded in the notebook; running the notebook or script also saves them as PNG files in an `images/` folder.

## 🧩 Dataset

220 transactions, 31 days (1 to 31 January 2023), no missing values or duplicates.

| Field | Type | Description |
|---|---|---|
| `transaction_id` | String (UUID) | Unique transaction identifier |
| `customer_id` | String | Unique customer identifier |
| `transaction_amount` | Float | Transaction value in ₹ |
| `transaction_date` | Date | Date of the transaction |
| `transaction_count` | Int | Transactions by the customer in a given week |
| `region` | String | North / South / East / West |
| `transaction_status` | String | Success / Fail |

## 🔬 What Was Done

**Part A: Theory** (see the PDF): statistical distributions, Q-Q plot, discrete vs continuous, Bernoulli, Binomial, Log-Normal, Power Law, Box-Cox, Poisson (with worked example), Z-score probability, PDF vs CDF.

**Part B: Analysis** (see the notebook):

| # | Task | Method |
|---|---|---|
| 1 | Bernoulli & Binomial fit | MLE for p; χ² goodness-of-fit; exact binomial test |
| 2 | Poisson fit (transactions/day) | MLE for λ; dispersion test |
| 3 | Log-Normal & Power Law for amounts | MLE, AIC/BIC, Kolmogorov-Smirnov; Clauset-style x_min search for the power-law tail |
| 4 | Q-Q plot | `statsmodels` Q-Q plots, Shapiro-Wilk, D'Agostino K² |
| 5 | Box-Cox transform | Optimal λ with 95 % CI, skewness/variance comparison, Levene test |
| 6 | Z-scores & P(amount > ₹5,000) | Empirical vs Normal vs Log-Normal vs Box-Cox methods |
| 7 | PDF & CDF | Histogram/KDE/PDF and empirical vs fitted CDFs, percentile table |
| 8 | Conclusion | Model comparison and business recommendations |

## 📈 Key Results

| Question | Result |
|---|---|
| Transaction success rate (Bernoulli p) | **0.445** (55.5 % of transactions fail) |
| Weekly transaction count (Binomial n=9, p=0.317) | Rejected by χ² test (data are over-dispersed: variance 3.23 vs 1.95) |
| Transactions per day (Poisson) | **λ ≈ 7.1**, dispersion index 0.75, Poisson not rejected |
| Best model for `transaction_amount` | **Log-Normal** (μ = 8.00, σ = 0.47): lowest AIC (3,821), KS p = 0.90 |
| Normal model | Rejected (KS p = 0.0003); Shapiro-Wilk p ≈ 10⁻¹⁸ on raw data |
| Power law | Poor on the whole range; fits the upper tail (≥ ₹3,248) with α ≈ 4.15, equal in likelihood to a truncated Log-Normal |
| Box-Cox λ | **−0.18** (95 % CI −0.40 to 0.03, includes 0, so a log transform is appropriate); skewness 3.73 → −0.01 |
| P(amount > ₹5,000) | Empirical **11.4 %**, Log-Normal 13.8 %, Box-Cox 13.9 %, plain Z-score (Normal) **20.5 %** |
| Outliers (\|z\| > 3) | 3 transactions; largest ₹20,463 (z = 8.6) |

## ✅ Conclusion & Business Insights

* **The Log-Normal distribution best fits `transaction_amount`.** Use the median and percentiles rather than the mean, and analyse the log/Box-Cox scale for statistical modelling.
* **Do not apply plain Z-score/Normal probabilities to raw amounts**: they overstate the chance of a >₹5,000 order (20.5 % vs 11.4 % observed).
* **Review extreme orders** (3 transactions with |z| > 3) for fraud or B2B/bulk handling.
* **Payment failures are the biggest lever**: 55.5 % of transactions fail.
* **Capacity planning:** daily volume is well approximated by Poisson(λ ≈ 7.1); about 18 % chance of a ≥10-transaction day.
* **Customer activity is uneven** (weekly counts over-dispersed), so segment light vs heavy buyers.

> **Limitation:** only 220 records over one month, so tail estimates (power law, 99th percentile) are uncertain.

## ▶️ How to Run

```bash
# 1. clone the repository
git clone https://github.com/<your-username>/Spread-Locator.git
cd Spread-Locator

# 2. (optional) create a virtual environment
python -m venv venv && source venv/bin/activate      # Windows: venv\Scripts\activate

# 3. install dependencies
pip install -r requirements.txt

# 4a. run the notebook
jupyter notebook Spread_Locator_Analysis.ipynb

# 4b. or run the script (prints the results and saves all charts to ./images)
python spread_locator_analysis.py
```

## 🛠️ Tech Stack

Python 3.9+, NumPy, Pandas, SciPy, Statsmodels, Matplotlib, Seaborn, Jupyter.

## 👤 Author

*Darshan Patil*

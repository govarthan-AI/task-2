# Data Quality and Exploratory Analysis: Customer Churn

**Track:** Data Analytics | **Author:** Govarthan

This project audits and cleans a customer churn dataset, records every data-quality decision, and looks at the first patterns linked to customers leaving.

## Files in this repository

| File | What it is |
|---|---|
| `customer_churn_analysis.ipynb` | Notebook with the full analysis (profiling, cleaning, validation, statistics, charts) |
| `customer_churn_sample.csv` | Original dataset (synthetic customer records supplied with the task) |
| `customer_churn_clean.csv` | Cleaned dataset (15 rows, 13 columns) |
| `data_dictionary.csv` | Meaning, type and range of every column |
| `images/` | Charts used in this summary |

## Data quality summary

The dataset has **15 customers and 11 columns**: customer ID, gender, age, tenure, subscription type, monthly and total charges, contract type, support tickets, payment method and churn.

| Check | Result |
|---|---|
| Missing values | None in any column |
| Duplicate rows / duplicate CustomerID | 0 / 0 |
| Data types | Correct (numbers are numeric, text is text) |
| Text values | No spelling variations, extra spaces or mixed case |
| Outliers (IQR rule) | None in Age, TenureMonths, MonthlyCharges, TotalCharges or SupportTickets |
| Billing consistency | TotalCharges equals TenureMonths x MonthlyCharges for all 15 rows |
| Plan prices | One price per plan: Basic 49.99, Pro 79.99, Enterprise 149.99 |

## Cleaning decisions and assumptions

The data was already in good shape, so **no rows were removed and no values were changed**. The cleaning steps are safe standardisation, so the process can be repeated if the data is refreshed:

1. Trimmed spaces and standardised text case (Payment method "UPI" was kept in capitals).
2. Kept `CustomerID` as text because it is an identifier.
3. Added `ChurnFlag` (Yes = 1, No = 0) to calculate churn rates; the original `Churn` column is kept.
4. Added `AvgChargePerMonth` (TotalCharges / TenureMonths) as a billing cross-check.

**Validation:** 12 automatic checks run in the notebook (no missing values, unique IDs, valid ranges, allowed category values, billing consistency, one price per plan). All 12 passed.

## Summary statistics

| Column | Mean | Min | Max |
|---|---|---|---|
| Age | 39.1 | 23 | 60 |
| TenureMonths | 18.8 | 2 | 48 |
| MonthlyCharges | 84.66 | 49.99 | 149.99 |
| SupportTickets | 2.5 | 0 | 6 |

![Distributions](images/distributions.png)

## First patterns

Overall churn rate: **46.7% (7 of 15 customers)**.

![Churn patterns](images/churn_patterns.png)

- **Contract type:** All 7 Month-to-Month customers churned, and none of the 8 customers on One Year or Two Year contracts did.
- **Plan:** Basic churned at 71.4% (5 of 7), Pro at 50% (2 of 4), and Enterprise at 0% (0 of 4).
- **Support tickets:** Customers who left raised 4.3 tickets on average, against 1.0 for those who stayed. Every customer with 3 or more tickets churned, and none with 2 or fewer did.
- **Tenure:** Churned customers had been subscribed for 7.0 months on average, against 29.1 months for those who stayed.
- **Payment method:** Bank Transfer customers did not churn (0 of 3), while Debit Card (2 of 2) and UPI (3 of 4) had the highest churn.

## Limitations

- With only 15 customers, these are first impressions and not statistically reliable findings.
- The data is synthetic, so real-world conclusions should not be drawn from it.
- Contract type, plan, tenure and support tickets overlap (for example, Basic plan customers are mostly on Month-to-Month contracts), so the effect of each one cannot be separated with this sample.
- A link between two columns does not show that one causes the other.

## How to run

1. Install Python with `pandas`, `matplotlib` and `jupyter`.
2. Keep all files in one folder and open `customer_churn_analysis.ipynb`.
3. Run all cells. The notebook recreates `customer_churn_clean.csv`, `data_dictionary.csv` and the charts.

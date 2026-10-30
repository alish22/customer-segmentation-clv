# Customer Segmentation and 90-Day Customer Value (Online Retail II)

An analysis in Python of two years of invoices from a UK online gift shop. It answers three questions: who are the best customers, who is slipping away, and which customers will bring the most money in the next 90 days?

The notebook with all charts and results is here: [`customer_segmentation_clv.ipynb`](customer_segmentation_clv.ipynb)

## Main results

- **Segments (RFM):** the 25% of customers called *Champions* bring **70% of net revenue**. The 26% called *Lost* bring 4%.
- **Win-back target:** the *At risk* group (14% of customers, 9% of revenue) bought about 5 orders each but has been away for about a year. It is the best group to try to win back.
- **Second method:** k-means clustering finds the same main groups (for example 71% of Champions fall in one cluster, and 95% of Lost customers fall in another).
- **Prediction of the next 90 days:** a two-part model (will the customer buy? how much if they do?) was tested on a period it had never seen. It ranks customers a little better than simple rules (rank correlation 0.565 against 0.478 and 0.532; AUC for "will they buy" 0.795). The gain over a simple rule is small. Recency (days since the last purchase) is the strongest signal.
- **Honest limit:** the test period is the autumn peak, so the model's pound totals are about 40% too low. Use the scores to **rank** customers, not as a money forecast.

## What the notebook does

1. **Cleaning with logged rules:** duplicates, missing customer IDs, cancellations (subtracted from net spend), zero prices, and non-product lines (postage, fees, adjustments). A table shows how many rows each rule removes.
2. **RFM segmentation:** Recency, Frequency and Monetary value, scored 1 to 5, and six named segments from simple rules.
3. **K-means check:** silhouette scores for k = 2 to 8, and a comparison with the RFM segments.
4. **Customer value model:** features built only from the past, a training window and a separate unseen test window, two simple baselines, MAE, rank correlation, top-10% capture, a calibration chart and permutation importance.
5. **Exports for a dashboard** (see `outputs/`).

## Outputs

| File | Content |
|---|---|
| `outputs/customers_segments_and_value.csv` | One row per customer: RFM numbers and scores, segment, cluster, probability of buying and predicted spend in the next 90 days |
| `outputs/segment_summary.csv` | Customers, net spend and averages per segment |
| `outputs/monthly_revenue.csv` | Purchases per month |

These files can be loaded into a dashboard tool such as Looker Studio or Power BI.

## Data

The data is the public **Online Retail II** dataset from the UCI Machine Learning Repository (invoice lines from December 2009 to December 2011, 1,067,371 rows). Download it from the source page, check its licence terms there, and put `online_retail_II.xlsx` (or a CSV with the same columns, named `online_retail_II.csv`) in the `data/` folder. The data file is not stored in this repository.

## How to run

```bash
pip install -r requirements.txt
jupyter notebook customer_segmentation_clv.ipynb
```

Then choose **Run all**. It takes about half a minute with the CSV file (a bit longer with the Excel file).

## Limits

- About 23% of invoice lines have no customer ID, so they are not in the analysis.
- The data covers one shop for two years, so the model has seen only one autumn.
- Many customers are wholesalers, so the results may not hold for shops that sell to consumers.
- The model is a clear starting point and was not tuned much.

## Tools

Python, pandas, NumPy, SciPy, scikit-learn, Matplotlib, Jupyter.

# eniac-discount-strategy-analysis

A data-driven investigation into whether discounting helps or hurts Eniac's e-commerce business — built to settle a real disagreement between two stakeholders.

## The Business Question

- **Marketing** believes discounts drive customer acquisition, satisfaction, and retention.
- **The Board** recent quarters show *orders up, revenue down*, and wants Eniac positioned in the quality segment rather than competing on price.

This project uses transaction-level data to find out who the evidence supports — and what Eniac should actually do about it.

## Key Findings

| Question | Answer |
|---|---|
| How much of the business relies on discounting? | **92.8%** of all transactions involved a discount. **96.2%** of products that ever sold were discounted at least once. |
| How deep are the discounts? | Mean **23.2%**, median **19.5%** off list price — substantial, not token. ~25% of discounted sales are marked down 30%+. |
| Does Black Friday mean deeper discounts? | No. Discount depth during Black Friday week (**23.38%**) is nearly identical to the rest of the year (**23.23%**). The revenue spike is driven by order volume and basket size, not markdowns. |
| Does discounting drive revenue? | **No meaningful relationship.** Daily-level correlation r = 0.086; product-level r = **-0.125** (weak negative). Deeper discounts do not reliably produce more revenue. |
| Which categories benefit most/least from discounting? | Correlation ranges from **+0.21** (Software) to **-0.19** (Apple Devices) — the effect isn't uniform across the catalog. |

**Bottom line:** discounting is already the steady-state condition of the business, not an occasional lever — and the data does not support deepening it further. See `AA_Analysis_Recommendation.ipynb` for the full write-up and category-level recommendations.

## Data Pipeline

The analysis is built across four notebooks, each one producing the input for the next:

```
1_Dataset_Exploration.ipynb          Raw data → cleaned data
   ├─ Fixes dtypes, drops duplicates/nulls, resolves price-format errors
   └─ Outputs: orders_cl.csv, orderlines_cl.csv, products_cl.csv, brands_cl.csv
        │
2_Quality_Assessment.ipynb           Cleaned data → analysis-ready data
   ├─ Filters to Completed orders only (226,904 → 46,605 → 40,985 final orders)
   ├─ Reconciles order totals against line-item totals (IQR outlier removal)
   └─ Outputs: orders_qu.csv, orderlines_qu.csv
        │
3_Product_Category.ipynb             Products → categorized products
   ├─ Maps ~100 opaque product-type codes to 39 subcategories → 8 broad categories
   ├─ Infers product condition (new / refurbished) from name patterns
   └─ Outputs: products_category.csv
        │
4_Analysis_Recommendation.ipynb      Final analysis → business recommendations
   ├─ Discount depth, seasonality, correlation, price distribution
   └─ Outputs: charts, findings, executive summary
```

> Note: notebook filenames in this repo may still carry their original working titles (`AA_Eniac_Dataset_Exploration.ipynb`, `AA_Quality_Assessment_challenge.ipynb`, etc.) — the numbering above reflects execution order.

## Data Quality Notes

A few issues were discovered and documented along the way rather than silently patched:

- **`promo_price` was corrupted** (decimal-shifted, e.g. €499.899 recorded for a €59.99 item) — discounts were rebuilt from actual transaction prices instead.
- **`type` was an opaque numeric code** with no lookup table — manually mapped and verified against real product records.
- **`price` is a current snapshot, not a history** — comparing it against old transactions can misstate discount direction; documented as a known limitation.
- **An unexplained ~99% revenue collapse, Feb 23 – Mar 2017** — flagged for follow-up rather than exclusion.

## Tech Stack

- Python (pandas, numpy)
- matplotlib, seaborn
- Jupyter / JupyterLab (conda environment: `pandas_project`)

## Repository Structure

```
├── notebooks/
│   ├── 1_Dataset_Exploration.ipynb
│   ├── 2_Quality_Assessment.ipynb
│   ├── 3_Product_Category.ipynb
│   └── 4_Analysis_Recommendation.ipynb
├── outputs/ presentation deck
└── README.md
```

*File paths in the notebooks may need updating to relative paths (e.g. `orders.csv`) depending on your folder setup.*

## Author

**Sree Malathi**
Data Analytics with AI, WBS Coding School
[LinkedIn](https://linkedin.com/in/sree-malathik)

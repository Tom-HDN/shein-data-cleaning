# Shein E-Commerce Data Cleaning & Exploration

Cleaning, validating, and exploring a multi-category Shein product listings dataset — turning inconsistent, scraped, per-category CSVs into a single analysis-ready dataset, then using it to look at how price, discount, and category relate to sales volume and revenue.

## Dataset

Source: [Shein E-Commerce Data](https://www.kaggle.com/datasets/oleksiimartusiuk/e-commerce-data-shein) (Kaggle), by Oleksii Martusiuk.

The raw data ships as one CSV per product category (e.g. `womens_cloth`, `mens_clothing`, `pet_supplies`, `jewelry_and_accessories`, `beauty_and_health`, `appliances`, `shoes`, `curve`, `sports_and_outdoors`). It's not committed to this repo — see [`data/README.md`](data/README.md) for how to fetch it.

## What this project does

- **Unifies inconsistent schemas** across per-category files, since not every category has the same columns (a parameterized `clean_df()` function checks for column presence before transforming, rather than assuming a fixed schema).
- **Parses messy string fields into numeric data**, including prices with currency symbols, discount percentages, and marketing-style sales counts (e.g. `"1.5k sold"`, `"500+ sold"`) via regex extraction.
- **Handles missing data column-by-column**, with the imputation (or intentional non-imputation) choice for each column explained and justified inline — e.g. `color_count` NaNs filled with 1 (assumes single color option), `sub_cat_rank` NaNs left as-is (assumes the item isn't ranked) rather than imputed.
- **Validates the cleaning**, checking for leftover currency/percent symbols, incorrect dtypes, and impossible negative values after transformation — rather than assuming the cleaning worked.
- **Merges the cleaned per-category data** into one dataframe and explores it: category-level distributions of price, discount, and sales; Pearson/Spearman correlations between discount depth and quantity sold / revenue (with `log1p` normalization applied where distributions are skewed); outlier handling via IQR bounds.
- **Exports the cleaned, merged dataset** as `shein_cleaned.csv` for reuse elsewhere (a dashboard, a modeling notebook, etc.).

## Key findings

- Revenue and quantity-sold vary a lot by category — `womens_cloth` leads on both average revenue and volume (and has the most listings); `pet_supplies` is lowest on both.
- `jewelry_and_accessories` and `beauty_and_health` rank 2nd/3rd in volume sold but drop lower in average revenue — high volume, lower price point.
- `mens_clothing`, `curve`, and `shoes` are the only categories where average revenue exceeds average volume sold.
- `pet_supplies` has the lowest average sales but the *highest* average discount-per-unit-sold ratio.
- Pooled correlations between price, discount, and sales are weak (|r| well under 0.3 in all pairs tested) — category-level dynamics appear to matter more than any single overall price/discount/sales relationship.

## Limitations

- `qty_sold` is inferred from a marketing label (e.g. `"500+ sold"`), not verified transaction data — treat it as an ordinal signal of popularity, not an exact count.
- ~33.76% of cleaned rows were dropped for the sales-focused analysis (rows without a usable `qty_sold`); `appliances` was reduced the most.
- `img_url` was entirely empty across the dataset and excluded from analysis.
- Outlier handling was applied inconsistently across visualizations, so headline averages are sensitive to a small number of extreme listings.

## How to run it

```bash
git clone https://github.com/<your-username>/shein-data-cleaning.git
cd shein-data-cleaning
pip install -r requirements.txt
jupyter notebook notebooks/Shein_Data_Cleaning.ipynb
```

The dataset downloads automatically at runtime via `kagglehub` — no manual download needed (a free Kaggle account/API token is required the first time; see `data/README.md`).

## Tech stack

Python · pandas · numpy · matplotlib · seaborn · scipy.stats · kagglehub

## What I'd do differently

- Extract the repeated per-category plotting code into a single reusable function earlier — I refactored it once I noticed five near-identical blocks, but doing it on the first duplication would have kept the notebook shorter throughout.
- Apply outlier trimming consistently across all visualizations rather than selectively, so summary statistics are comparable across sections.

## License

MIT — see [LICENSE](LICENSE).

# Data

The raw dataset is **not committed to this repository** — it's pulled at runtime by the notebook itself, and Kaggle's terms don't clearly permit redistributing the raw files elsewhere.

## Source

[Shein E-Commerce Data](https://www.kaggle.com/datasets/oleksiimartusiuk/e-commerce-data-shein) — Oleksii Martusiuk, via Kaggle.

The dataset consists of one CSV per product category (`womens_cloth.csv`, `mens_clothing.csv`, `pet_supplies.csv`, `jewelry_and_accessories.csv`, `beauty_and_health.csv`, `appliances.csv`, `shoes.csv`, `curve.csv`, `sports_and_outdoors.csv`), scraped listing data including product title, price, discount, rating, color count, sub-category rank, and a marketing-style sales count string (e.g. `"1.5k sold"`).

## How to get it

The notebook downloads it automatically via [`kagglehub`](https://pypi.org/project/kagglehub/):

```python
import kagglehub
path = kagglehub.dataset_download("oleksiimartusiuk/e-commerce-data-shein")
```

This requires a free Kaggle account and API credentials the first time you run it:

1. Create a Kaggle account at [kaggle.com](https://www.kaggle.com) if you don't have one.
2. Go to **Account → Settings → API → Create New Token**. This downloads a `kaggle.json` file containing your credentials.
3. Place it at `~/.kaggle/kaggle.json` (Linux/Mac) or `C:\Users\<you>\.kaggle\kaggle.json` (Windows).
4. Run the notebook — `kagglehub` handles the download and local caching automatically after that.

## Output

The cleaning notebook writes a single merged, cleaned file, `shein_cleaned.csv`, to the project root once it finishes running. That file also isn't committed here by default (see `.gitignore`) — regenerate it by running the notebook, or remove the `*.csv` line from `.gitignore` if you'd like to commit a snapshot of it for others to inspect without running the full pipeline.

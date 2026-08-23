# Robot Project — Business Performance Data Cleaning

Jupyter notebooks that clean and explore the operational datasets behind the Southeast Robot Delivery project: delivery cost/reliability, customer satisfaction and behavior, predictive maintenance, scaling forecasts, sales, and orders.

## ⚠️ Notebook names don't map 1:1 to datasets

These notebooks grew organically while Noah worked through each new CSV as it arrived, so several notebooks cover more than one dataset, and a couple of datasets are cleaned in more than one place. Each notebook now has a markdown header (and section headers, where it covers multiple datasets) explaining exactly what it does — read those before assuming a notebook's name tells you its full scope.

| Notebook | Datasets covered |
| --- | --- |
| `Customer_Satisfaction.ipynb` | `delivery_cost_comparison.csv`, `customer_satisfaction_metrics.csv`, `customer_behavior.csv`, `predictive_maintenance.csv`, `personalized_orders.csv` |
| `Delivery_Reliability.ipynb` | `delivery_reliability.csv`, `scaling_forecasts.csv` (a second, independent pass — see caveat below), `delivery_financials.csv` |
| `Delivery_Routes.ipynb` | `demand_forecasts.csv`, `delivery_routes.csv` |
| `Scaling_Forecasts.ipynb` | `scaling_forecasts.csv` (primary cleaning pipeline) |
| `Sales_Data Visualization.ipynb` | `sales_data.csv` |

### Known data-quality caveats (flagged inline in the notebooks, not silently fixed)

- **`Customer_Satisfaction.ipynb`, section 4** loads `customer_behavior.csv` but writes the cleaned result to `delivery_reliability_PROFESSIONAL.csv`. That output filename doesn't match its actual source data — it looks like a copy-paste mix-up from `Delivery_Reliability.ipynb`. Verify intent before relying on that file.
- **`scaling_forecasts.csv` is cleaned independently in two places** — `Scaling_Forecasts.ipynb` (the primary pipeline) and a second pass inside `Delivery_Reliability.ipynb`. They aren't guaranteed to produce the same result; treat `Scaling_Forecasts.ipynb` as canonical unless you've verified otherwise.

## 🗂️ Expected data layout

Raw CSVs are personal downloads and are **not committed to this repo** (see `.gitignore`). Before running a notebook, create this layout at the repo root:

```
data/
├── raw/          # place the original CSVs here, unmodified
│   ├── delivery_cost_comparison.csv
│   ├── delivery_financials.csv
│   ├── delivery_reliability.csv
│   ├── delivery_routes.csv
│   ├── demand_forecasts.csv
│   ├── customer_satisfaction_metrics.csv
│   ├── customer_behavior.csv
│   ├── predictive_maintenance.csv
│   ├── personalized_orders.csv
│   ├── scaling_forecasts.csv
│   └── sales_data.csv
└── processed/    # cleaned outputs are written here by the notebooks
```

Each notebook only needs the raw file(s) it covers (see the table above) — you don't need all eleven to run any single notebook.

## ⚙️ Setup

```bash
pip install pandas numpy matplotlib scipy jupyter
jupyter notebook
```

## 🧹 What the cleaning pipelines generally do

Most notebooks follow the same pattern per dataset:
1. Preview the raw file and confirm its columns.
2. Deduplicate (by a natural key where one exists, e.g. `delivery_id`, `order_id`).
3. Drop rows missing essential identifiers.
4. Coerce numeric columns and replace outliers (via the IQR method) with the column median.
5. Save the cleaned result to `data/processed/` and print a before/after audit (row count, file size).

## 👤 Author

**Noah Asgodom**
📧 noahasgodom104@gmail.com
🔗 [LinkedIn](http://linkedin.com/noah-asgodom/)

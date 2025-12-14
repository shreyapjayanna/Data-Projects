# Customer Analysis – Online Retail II (K-Means Clustering)

Customer segmentation on the UCI **Online Retail II** dataset using K-Means. The notebook cleans the transactional data, engineers RFM-style features, and clusters customers to guide retention, re-engagement, and growth strategies.

## Project Contents
- `online-retail-data-clustering.ipynb` — full workflow: EDA, cleaning, feature engineering, clustering, and visuals.
- `data/online_retail_II.xlsx` — raw dataset (UCI Online Retail II). Keep zipped/ignored if size is an issue.
- `requirements.txt` — Python dependencies.

## Environment & Setup
```bash
python -m venv venv
venv\Scripts\activate   # Windows
pip install -r requirements.txt
```

## How to Run
Open the notebook and run all cells:
```bash
jupyter notebook online-retail-data-clustering.ipynb
```
The notebook expects the data file at `data/online_retail_II.xlsx`.

## Workflow (Notebook Highlights)
1. **Imports & sanity checks**: pandas, seaborn/matplotlib, sklearn; confirms working directory and data presence.
2. **Load data**: `pd.read_excel("data/online_retail_II.xlsx")`.
3. **Exploration**: `head`, `info`, `describe`, missing values, negative quantities, zero prices.
4. **Data cleaning**:
   - Invoice and StockCode coerced to string and filtered via regex (valid invoice/stock patterns).
   - Drop rows with missing `Customer ID`.
   - Remove zero/negative prices.
5. **Feature engineering**:
   - `SalesLineTotal = Quantity * Price`.
   - Aggregate to customer level: `MonetaryValue` (sum), `Frequency` (unique invoices), `LastInvoiceDate` (max).
   - `Recency` = days since last invoice (relative to dataset max date).
6. **Outlier handling**: IQR-based outlier detection on Monetary and Frequency; outliers kept with special cluster tags.
7. **Scaling & clustering**:
   - Standardize Monetary, Frequency, Recency.
   - Elbow (inertia) and silhouette curves for k=2..12.
   - Chosen model: K-Means with k=4 on non-outliers; outliers tagged separately (-1, -2, -3).
8. **Labeling & interpretation**:
   - Core clusters: 0=RETAIN, 1=RE-ENGAGE, 2=NURTURE, 3=REWARD.
   - Outlier buckets: -1=PAMPER, -2=UPSELL, -3=DELIGHT.
   - Summary of cluster strategies in notebook markdown.
9. **Visuals**:
   - Histograms/boxplots for R/F/M.
   - 3D scatter of scaled features by cluster.
   - Violin plots per cluster.
   - Cluster size vs. average feature lines.

## Results (summary)
- Four primary segments with distinct Recency/Frequency/Monetary profiles; k=4 chosen as a balance of inertia drop and slightly better silhouette than k=5.
- Additional outlier segments identified for high-value / high-frequency edge cases.

## Business Impact
- Targets retention and re-engagement: Segments (RETAIN, RE-ENGAGE, NURTURE, REWARD) show where to focus loyalty perks vs. win-back offers, lifting repeat revenue.
- Prioritizes spend efficiently: High-value and high-frequency groups get personalized rewards; low-frequency groups get promos tuned to bring them back without over-discounting.
- Reduces churn risk: Recency tracking flags fading customers early, enabling timely nudges before they lapse.
- Informs inventory and pricing: Cluster spend/recency patterns highlight products and price points that resonate with each segment, guiding stock and promotional planning.
- Improves marketing ROI: Tailored messaging per segment reduces blanket campaigns and channels budget to the audiences most likely to convert.
- Executive visibility: Clear visuals (elbow/silhouette, 3D scatter, violins, cluster size vs. averages) translate model outputs into actionable segment narratives.


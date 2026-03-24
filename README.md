# Customer Segmentation using K-Means Clustering

> Unsupervised machine learning applied to real retail transaction data to identify distinct customer segments using the RFM framework.

---

## Project Overview

This project applies the **K-Means clustering algorithm** to segment customers of an online retail store based on their purchasing behavior. The analysis follows the industry-standard **RFM model**:

| Dimension | Description |
|---|---|
| **Recency** | How recently did the customer make a purchase? |
| **Frequency** | How often do they buy? |
| **Monetary (Total Value)** | How much do they spend in total? |

Each customer is represented as a single point in 3D RFM space, and K-Means groups them into meaningful segments that a business can act on.

---

## Project Structure

```
customer-segmentation-kmeans/
├── notebook.ipynb              ← Full analysis notebook
├── README.md                   ← You are here
├── requirements.txt            ← Python dependencies
├── data/
│   └── online_retail_II.csv   ← Dataset (download instructions below)
└── outputs/
    ├── transactions_over_time.png
    ├── distributions.png
    ├── rfm_distributions.png
    ├── elbow_silhouette.png
    ├── cluster_3d.png
    ├── cluster_bar_charts.png
    ├── cluster_heatmap.png
    ├── segment_pie.png
    └── rfm_aggregated.csv
```

---

## Dataset

**UCI Online Retail II** — Real transactional data from a UK-based online gift retailer (2009–2011).

- ~500,000 rows · 8 columns
- Source: [Kaggle — UCI Online Retail II](https://www.kaggle.com/datasets/mashlyn/online-retail-ii-uci)

**Download steps:**
1. Visit the Kaggle link above
2. Download `online_retail_II.csv`
3. Place it inside the `data/` folder

---

## How to Run

### 1. Clone the repository
```bash
git clone https://github.com/YOUR_USERNAME/customer-segmentation-kmeans.git
cd customer-segmentation-kmeans
```

### 2. Install dependencies
```bash
pip install -r requirements.txt
```

### 3. Add the dataset
Place `online_retail_II.csv` inside the `data/` folder.

### 4. Run the notebook
```bash
jupyter notebook notebook.ipynb
```

Run all cells from top to bottom. Outputs (plots + CSV) will be saved automatically to `outputs/`.

---

## Methodology

### Step 1 — Data Cleaning
- Removed cancelled orders (invoices starting with `C`)
- Dropped rows with missing `Customer ID` or `Description`
- Removed entries with non-positive `Quantity` or `Price`

### Step 2 — Feature Engineering
- Mapped raw columns to a clean schema: `Client_ID`, `Transaction_Number`, `Item_Name`, `Item_Price`, `Quantity`, `Total_Amount`
- Computed `Total_Amount = Quantity × Price`

### Step 3 — RFM Aggregation
- Aggregated to one row per customer
- Computed `Recency` as days since last purchase relative to the dataset's latest date

### Step 4 — Preprocessing
- Capped outliers using the **IQR method** (1.5× fence)
- Normalized all features using **StandardScaler** — essential for distance-based algorithms

### Step 5 — Choosing K
- **Elbow Method**: Plotted inertia for K = 2–10
- **Silhouette Score**: Validated cluster separation quality
- **Chosen K = 4** based on the elbow inflection and peak silhouette score

### Step 6 — K-Means Clustering
- Algorithm: `KMeans(init='k-means++', n_init=10, random_state=42)`
- `k-means++` initialization reduces the risk of poor centroid initialization

---

## 📈 Results

Four distinct customer segments were identified:

| Segment | Total Value | Frequency | Recency | Strategy |
|---|---|---|---|---|
|  Champions | High | High | Low | Reward & retain |
|  Loyal Customers | Medium-High | Medium | Low-Medium | Upsell & engage |
|  At Risk | Medium | Low | High | Re-engage with offers |
|  Dormant | Low | Very Low | Very High | Win-back or deprioritize |

---

## Key Business Insights

1. **Champions generate disproportionate revenue** — prioritize them in loyalty programs
2. **At Risk customers are the highest ROI re-engagement target** — they were once active and can be won back
3. **Recency is the strongest churn predictor** — any customer inactive for 60+ days should trigger an alert
4. **Dormant customers are costly to reactivate** — redirect that budget to acquiring new customers

---

## Tech Stack

| Tool | Purpose |
|---|---|
| Python 3.10+ | Core language |
| Pandas | Data manipulation |
| NumPy | Numerical operations |
| Scikit-learn | K-Means, StandardScaler, Silhouette Score |
| Matplotlib / Seaborn | Visualization |
| Jupyter Notebook | Interactive analysis environment |

---

## License

This project is open-source under the [MIT License](LICENSE).

---

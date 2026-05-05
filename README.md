# E-Commerce Customer Lifetime Value & Churn Analysis

Brazilian Olist Dataset · 100,000+ Orders · Python · Power BI

## Table of Contents

- [About The Project](#about-the-project)
- [Key Findings](#key-findings)
- [Dashboard](#dashboard)
- [Built With](#built-with)
- [Getting Started](#getting-started)
- [Dataset](#dataset)
- [Methodology](#methodology)
- [Project Structure](#project-structure)
- [Results](#results)
- [Lessons Learned](#lessons-learned)
- [Future Work](#future-work)
- [Acknowledgements](#acknowledgements)
- [Contact](#contact)

## About The Project

Olist is Brazil's largest department store marketplace, connecting small businesses to customers through a single contract. With 100,000+ orders across 93,350 unique customers (2016-2018), the platform scaled rapidly to R$15.4M in revenue.

But behind the growth curve sat a critical problem: 80% of customers never came back after their first purchase.

Without visibility into which customers were most likely to churn, and which of those were actually worth saving, retention spend was being wasted on customers who would never return while high-value buyers quietly disappeared.

This project builds a complete customer analytics pipeline that answers three questions:

| Question | Method | Output |
|---|---|---|
| Who are our most valuable customers? | RFM Segmentation | 5 customer segments with CLV rankings |
| Who is about to leave? | Random Forest Classifier | Churn probability score per customer |
| Where should we focus retention spend? | Revenue-at-Risk Analysis | Filterable action list of 15,238 at-risk customers |

The final deliverable is a 2-page interactive Power BI dashboard that translates model output into business language - instead of "recall = X%", it says "here are 15,238 customers worth R$6.74M that will leave in the next 90 days."

## Key Findings

- 97% of customers are one-time buyers - the single most important insight. Loyalty programs and post-purchase engagement are critically needed.
- 15,432 Champion customers generate 30.7% of total revenue (R$4.73M) despite being only 16.5% of the customer base. Each Champion is worth R$307 - 5.6x more than a Potential customer (R$55).
- 15,238 high-value customers are at risk of churning (churn probability > 60%, top 20% spenders), representing R$6.74M in projected revenue loss - 43.7% of total revenue.
- 8,857 Champions are flagged as high-risk, with R$2.78M in revenue exposure. These are the highest-priority saves.
- Revenue grew steadily from Oct 2016 through a peak in Nov 2017, then plateaued - suggesting the platform hit a retention ceiling well before acquisition slowed.

## Dashboard

The interactive Power BI dashboard tells the complete story across two pages:

### Page 1 - Customer Revenue & Churn Overview

| Visual | What It Shows |
|---|---|
| 4 KPI Cards | R$15.42M revenue, 93.35K customers, 80.09% churn, R$12.52M at risk |
| Monthly Revenue Trend | Growth trajectory from Oct 2016 through 2018 plateau |
| Revenue by Segment | Champion-dominated revenue distribution |
| Churn Probability Distribution | Histogram with 0.6 risk threshold line |
| Segment & Churned Slicers | Interactive filtering across all visuals |

### Page 2 - Customer Triage & Retention Intelligence

| Visual | What It Shows |
|---|---|
| Revenue Vulnerability Scatter | Every dot = one customer; upper-right = danger zone |
| RFM Segment Donut | Distribution across 5 segments |
| Average CLV by Segment | Champions at R$307 vs Potential at R$55 |
| Days Since Last Purchase | Lost at 472 days vs Champions at 90 days |
| Retention Action List | Filterable table of at-risk customers sorted by churn probability |

Live dashboard link: add the Power BI publish URL here when available.

## Built With

| Category | Tools |
|---|---|
| Language | Python 3.x |
| Data Processing | Pandas, NumPy |
| Machine Learning | Scikit-Learn (Random Forest, StandardScaler, train_test_split) |
| Visualization | Matplotlib, Seaborn |
| Geospatial | Folium, Folium HeatMap plugin |
| Dashboard | Microsoft Power BI Desktop (DAX measures, cross-dataset relationships) |
| Version Control | Git, GitHub |

## Getting Started

### Prerequisites

- Python 3.8+ installed from [python.org](https://www.python.org/)
- Jupyter Notebook or JupyterLab for running `.ipynb` files
- Power BI Desktop (free, Windows only) for the dashboard, or Tableau Public on Mac
- Git for cloning the repository

### Installation

1. Clone the repository

```
git clone https://github.com/dangolre/ecommerce-clv-retention-analysis.git
cd ecommerce-clv-retention-analysis
```

2. Create a virtual environment (recommended)

```
python -m venv venv

# On Windows
venv\Scripts\activate

# On macOS/Linux
source venv/bin/activate
```

3. Install dependencies

```
pip install pandas numpy matplotlib seaborn scikit-learn folium wordcloud jupyter
```

The notebooks use:

- pandas>=1.5.0
- numpy>=1.23.0
- matplotlib>=3.6.0
- seaborn>=0.12.0
- scikit-learn>=1.1.0
- folium>=0.14.0
- wordcloud>=1.8.0
- jupyter>=1.0.0

4. Download the dataset

Download all 9 CSV files from the Olist Brazilian E-Commerce Dataset on Kaggle and place them in the `data/raw/` directory:

```
data/raw/
├── olist_orders_dataset.csv
├── olist_customers_dataset.csv
├── olist_order_items_dataset.csv
├── olist_order_payments_dataset.csv
├── olist_products_dataset.csv
├── olist_geolocation_dataset.csv
├── olist_order_reviews_dataset.csv
├── olist_sellers_dataset.csv
└── product_category_name_translation.csv
```

### Running the Notebooks

Launch Jupyter in the `notebooks/` folder, then run the notebooks in sequential order. Each notebook produces output files consumed by the next:

```
jupyter notebook notebooks/
```

| Order | Notebook | Input | Output | Runtime |
|---|---|---|---|---|
| 1 | 01_data_exploration.ipynb | 9 raw CSVs | customer_heatmap.html | ~2 min |
| 2 | 02_data_cleaning_merging.ipynb | 9 raw CSVs | golden_dataset.csv | ~3 min |
| 3 | 03_rfm_segmentation.ipynb | golden_dataset.csv | rfm_scores.csv | ~1 min |
| 4 | 04_feature_engineering.ipynb | golden_dataset.csv + rfm_scores.csv | features.csv | ~1 min |
| 5 | 05_churn_model.ipynb | features.csv | final_analytics.csv + 6 charts | ~2 min |
| 6 | 06_final_export.ipynb | Optional placeholder | No required output | Optional |

Verify each notebook runs cleanly:

```
# Run all notebooks top-to-bottom from the command line (optional)
jupyter nbconvert --to notebook --execute notebooks/01_data_exploration.ipynb
jupyter nbconvert --to notebook --execute notebooks/02_data_cleaning_merging.ipynb
jupyter nbconvert --to notebook --execute notebooks/03_rfm_segmentation.ipynb
jupyter nbconvert --to notebook --execute notebooks/04_feature_engineering.ipynb
jupyter nbconvert --to notebook --execute notebooks/05_churn_model.ipynb
```

After running the pipeline notebooks, your `outputs/` folder will contain:

```
outputs/
├── final_analytics.csv       ← Main analytics file for Power BI
├── customer_heatmap.html     ← Interactive map (open in browser)
├── rfm_segments.png
├── monthly_revenue.png
├── churn_distribution.png
├── feature_importance.png
├── clv_by_segment.png
└── at_risk_scatter.png
```

### Reproducing the Dashboard

1. Open Power BI Desktop.
2. Click Get Data -> Text/CSV -> select `outputs/final_analytics.csv` -> Load.
3. Repeat for `data/processed/golden_dataset.csv` (needed for monthly revenue trend).
4. Go to Model view -> drag `golden_dataset.customer_unique_id` onto `final_analytics.customer_unique_id` to create a Many-to-One relationship.
5. Return to Report view and build visuals following the layout in the Dashboard section.
6. Create these DAX measures:

```
-- KPI: Churn Rate
Churn Rate % = AVERAGE(final_analytics[churned]) * 100

-- KPI: Revenue at Risk (customers with >60% churn probability)
Revenue At Risk =
CALCULATE(
    SUM(final_analytics[monetary]),
    final_analytics[churn_prob] > 0.6
)
```

## Dataset

Source: Brazilian E-Commerce Public Dataset by Olist

This is real commercial data from Brazil's largest department store marketplace, anonymized by the original publisher. It covers 100K+ orders from 2016-2018 across multiple Brazilian marketplaces. All store and partner names have been replaced with Game of Thrones house names.

| File | Rows | Purpose |
|---|---|---|
| olist_orders_dataset.csv | 99,441 | Order backbone - timestamps, status, delivery dates |
| olist_customers_dataset.csv | 99,441 | Customer identity, city, state, zip code |
| olist_order_items_dataset.csv | 112,650 | Line items - product, seller, price, freight per item |
| olist_order_payments_dataset.csv | 103,886 | Payment values, installments, and methods |
| olist_products_dataset.csv | 32,951 | Product categories and dimensions |
| olist_geolocation_dataset.csv | 1,000,163 | Zip code -> latitude/longitude coordinates |
| product_category_name_translation.csv | 71 | Portuguese -> English category translation |
| olist_order_reviews_dataset.csv | 99,224 | Review scores and customer comments |
| olist_sellers_dataset.csv | 3,095 | Seller identity and location |

Important data notes:

- An order can have multiple items, each potentially from a different seller.
- The payments table has multiple rows per order (one per installment) - must aggregate before merging.
- Geolocation file has 1M+ rows - group by zip code prefix before joining to avoid memory issues.

## Methodology

### Phase 1 - Data Exploration

Loaded all 9 tables and profiled each for shape, missing values, and data types. Built an interactive Folium heatmap of customer density across Brazil by merging customer zip codes with geolocation coordinates. Cleaned ocean-coordinate outliers from the geolocation data before mapping.

### Phase 2 - Data Cleaning & Merging

Converted all 5 timestamp columns to datetime. Filtered to delivered orders only (~96K rows). Engineered two time-difference features: `purchased_delivered` (days from purchase to delivery) and `delivered_estimated` (days between actual vs estimated delivery). Aggregated the payments table at the order level using `groupby('order_id').sum()` to prevent revenue inflation from installment rows. Performed the master merge across orders, customers, payments, and items into a single golden dataset. Printed `.shape` after every merge to catch row explosions from duplicate join keys.

### Phase 3 - RFM Segmentation

Calculated three metrics per customer using the dataset's max date as the snapshot, not today's date:

- Recency - days since last purchase
- Frequency - count of unique orders
- Monetary - total spend

Scored each dimension on a 1-5 quintile scale and mapped composite scores to 5 business segments:

| Segment | Rule | Count | Avg CLV | Churn Rate |
|---|---|---|---|---|
| Champion | R >= 4, M >= 4 | 15,432 | R$307 | 51% |
| Loyal | R >= 3, M >= 3 | 18,728 | R$178 | 80% |
| Potential | R >= 3, M <= 2 | 21,976 | R$55 | 67% |
| At-Risk | R = 2 | 18,576 | R$168 | 100% |
| Lost | R = 1 | 18,638 | R$163 | 100% |

### Phase 4 - Feature Engineering

Defined churn as no purchase within 90 days of the dataset's end date, chosen for business explainability over arbitrary thresholds. Built a customer-level feature matrix with 7 predictive features: total orders, total spend, average order value, average freight cost, average price, average delivery days, and max installments.

Critical decision: excluded recency from model features to prevent data leakage. Since recency > 90 was used to define the churn label, including recency as a predictor would leak the target into the feature space, producing artificially inflated accuracy (~99%) that collapses in production.

### Phase 5 - Churn Prediction Model

Trained a Random Forest Classifier with the following configuration:

```
RandomForestClassifier(
    n_estimators=100,
    class_weight='balanced',
    random_state=42
)
```

Used a 70/30 stratified train-test split. Evaluated with `classification_report` focusing on recall for the churned class because correctly identifying customers who will leave matters more than precision. Applied churn probabilities to all 93,350 customers using `predict_proba`. Identified the intersection of high churn probability (> 0.6) and high lifetime value (top 20% spenders) as the retention action zone: 15,238 customers representing R$6.74M.

### Phase 6 - Dashboard & Visualization

Built a 2-page Power BI dashboard importing both `final_analytics.csv` (customer-level) and `golden_dataset.csv` (order-level) with a cross-dataset relationship on `customer_unique_id`. Created DAX measures for Churn Rate % and Revenue at Risk. Designed Page 1 for executive overview (KPIs, trends, distributions) and Page 2 for operational triage (scatter plot, segment analysis, filterable retention list).

## Project Structure

```
ecommerce-clv-retention-analysis/
│
├── data/
│   ├── raw/                          ← 9 original CSVs from Kaggle (gitignored)
│   ├── processed/
│   │   ├── golden_dataset.csv        ← Master merged dataset (~96K orders)
│   │   ├── rfm_scores.csv           ← RFM scores + segment labels per customer
│   │   └── features.csv             ← Model-ready feature matrix with churn labels
│   └── schema/                       ← Table relationship notes
│
├── notebooks/
│   ├── 01_data_exploration.ipynb     ← EDA, profiling, interactive heatmap
│   ├── 02_data_cleaning_merging.ipynb ← Timestamp parsing, aggregation, master merge
│   ├── 03_rfm_segmentation.ipynb     ← R/F/M calculation, quintile scoring, segment mapping
│   ├── 04_feature_engineering.ipynb   ← Churn definition (90-day), feature matrix construction
│   ├── 05_churn_model.ipynb          ← Random Forest training, evaluation, all 6 visualizations
│   └── 06_final_export.ipynb         ← Optional export placeholder
│
├── outputs/
│   ├── final_analytics.csv           ← Complete customer analytics file for Power BI
│   ├── customer_heatmap.html         ← Interactive Folium heatmap (open in browser)
│   ├── rfm_segments.png              ← Customer segment bar chart
│   ├── monthly_revenue.png           ← Revenue trend line chart
│   ├── churn_distribution.png        ← Churn probability histogram
│   ├── feature_importance.png        ← Top 10 features driving churn
│   ├── clv_by_segment.png            ← Average CLV by segment
│   └── at_risk_scatter.png           ← High-value at-risk scatter plot
│
├── dashboard/
│   └── ecommerce_dashboard.pbix      ← Power BI dashboard file
│
├── .gitignore                        ← Excludes data/raw/, checkpoints, HTML maps
├── README.md
└── requirements.txt
```

## Results

### Revenue & Growth

- Total Revenue: R$15,421,082.85 across 93,350 unique customers
- Growth Peak: Nov 2017 - revenue plateaued afterward, signaling a retention ceiling
- Champions alone generate R$4.73M (30.7% of total revenue)

### Customer Behavior

- 97% of customers are one-time buyers (90,549 out of 93,350)
- Average customer value: R$165 overall, but R$307 for Champions vs R$55 for Potential (5.6x gap)
- Engagement gap: Champions last purchased 90 days ago; Lost customers at 472 days (5.2x difference)

### Churn & Revenue Risk

- Overall churn rate: 80.09%
- 68,686 customers (73.6%) fall in the Critical churn band (probability > 0.8)
- 15,238 high-value customers at risk - R$6.74M in projected revenue loss (43.7% of total)
- 8,857 Champions at risk - R$2.78M in revenue exposure (highest-priority retention targets)

### Business Recommendations

- Implement post-purchase engagement within 7 days - with a 97% single-purchase rate, the first week after delivery is the critical window to drive a second purchase.
- Focus retention budget on Champions - each Champion saved is worth R$307 vs R$55 for a Potential customer; prioritize the 8,857 Champions flagged as high-risk.
- Investigate delivery experience - average delivery days emerged as a significant churn predictor; late deliveries correlate with higher churn probability.
- Build a loyalty or rewards program - the current marketplace model incentivizes one-time transactions; a points or cashback system could shift repeat purchase behavior.

## Lessons Learned

- 97% single-purchase rate is not a bug - it's the business model. Most e-commerce analyses assume repeat buying. This marketplace required adapting the entire methodology to a one-time-buyer-dominant dataset where Frequency as a feature was nearly useless (97% of values = 1).
- Always aggregate payments before merging. The payments table has multiple rows per order (one per installment). Forgetting to `groupby('order_id').sum()` inflates every customer's revenue by their installment count. Always print `.shape` before and after to verify.
- Recency cannot be a feature when it defines the target. Using recency > 90 days to label churn and then feeding recency into the model creates data leakage. This produces artificially inflated accuracy (~99%) that falls apart in production.
- Print shape after every merge. The orders table has ~96K rows. If a merge suddenly produces 300K+ rows, a join key has duplicates. Catching this early prevents cascading errors through every downstream notebook.
- Snapshot date must come from the data, not the system clock. Using `datetime.today()` as the RFM snapshot makes every customer's recency ~2,500+ days and renders the entire segmentation meaningless. Always use `df['order_purchase_timestamp'].max()`.

## Future Work

- Add SHAP values for model interpretability and feature-level explanations
- Build geographic revenue analysis using customer state/city data
- Implement time-series forecasting for monthly revenue projections
- Add Portuguese review sentiment analysis using NLP
- Test additional models (XGBoost, Logistic Regression) for comparison
- Deploy the interactive heatmap via GitHub Pages for README embedding
- Integrate with the Olist Marketing Funnel Dataset for customer acquisition cost analysis

## Acknowledgements

Dataset: Olist Brazilian E-Commerce Public Dataset - real commercial data generously provided by Olist and published on Kaggle.

## Contact

Reeya Dangol

LinkedIn: [linkedin.com/in/reeya-dangol](https://www.linkedin.com/in/reeya-dangol)

GitHub: [github.com/dangolre](https://github.com/dangolre)

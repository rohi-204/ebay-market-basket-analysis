# 🛒 Market Basket Analysis with Python – eBay

A Python-based customer behavior and market basket analysis on eBay survey data, covering data cleaning, descriptive analysis, customer segmentation, recommendation insights, and visualization.

---

## 📁 Project Structure

```
ebay-market-basket-analysis/
│
├── eBay.csv                    # Raw survey dataset
├── ebay_analysis.ipynb         # Main analysis notebook
└── README.md
```

---

## 📋 Dataset

**File:** `eBay.csv`
**Records:** 800 customer survey responses

**Key Columns:**
- `age`, `Gender`, `Purchase_Frequency`, `Purchase_Categories`
- `Browsing_Frequency`, `Product_Search_Method`, `Cart_Abandonment_Factors`
- `Shopping_Satisfaction`, `Rating_Accuracy`, `Customer_Reviews_Importance`
- `Review_Helpfulness`, `Recommendation_Helpfulness`, `Review_Reliability`

---

## ✅ Tasks Overview

### Task 1 – Data Cleaning and Preparation
- Removed duplicate survey entries
- Standardized categorical fields: purchase frequency (High/Medium/Low), browsing frequency, cart abandonment factors
- Encoded ordinal fields numerically (e.g., Review_Helpfulness: No=0, Sometimes=1, Yes=2)
- Handled missing values in `Product_Search_Method`, `Cart_Abandonment_Factors`, and `Improvement_Areas`
- Stripped trailing spaces and removed duplicate column names
- Converted rating columns (`Shopping_Satisfaction`, `Rating_Accuracy`, `Customer_Reviews_Importance`) to numeric types

### Task 2 – Descriptive Behavior Analysis
- **Demographics:** 800 customers, average age 35.7 (range: 3–67); balanced gender distribution across Male (191), Female (198), Others (209), Prefer not to say (202)
- **Purchase Frequency:** Medium (332) > High (296) > Low (172)
- **Top Categories:** Groceries and Gourmet Food (366), Beauty and Personal Care (230), Clothing and Fashion (122)
- **Browsing Methods:** Keyword (175), Others (164), Unknown (161), Categories (158), Filter (142)
- **Cart Abandonment:** High shipping costs (208), Changed mind (204), Others (204), Better price elsewhere (184)
- **Satisfaction Stats:** Mean ≈ 2.87, Median = 3.0 | Rating Accuracy Mean ≈ 3.09

### Task 3 – Customer Segmentation and Profiling
- **Rule-based segments:**
  - *Frequent Buyers* (109): High frequency + satisfaction ≥ 4 → avg satisfaction 4.48
  - *Occasional Shoppers* (187): Medium frequency + satisfaction 3–4 → avg satisfaction 3.95
  - *At-Risk Customers* (504): Low satisfaction (≤ 2) or Price Issue abandonment → avg satisfaction 2.12
- **K-Means Clustering (k=3):** Behavioral grouping on `Shopping_Satisfaction`, `Customer_Reviews_Importance`, `Rating_Accuracy` → Cluster 1: 322, Cluster 2: 245, Cluster 0: 233

### Task 4 – Recommendation and Review Insights
- Correlation between recommendation helpfulness and satisfaction ≈ **−0.0005** (negligible)
- Review reliability vs. rating accuracy correlation ≈ **0.065** (weak positive)
- Personalized recommendation engagement: No (34.9%), Yes (32.6%), Sometimes (32.5%)
- **Actionable Suggestions:** Improve personalization accuracy, use price-based recommendations, leverage trusted reviews, apply segment-specific strategies, increase recommendation transparency

### Task 5 – Visualization and Reporting
- Bar chart: Most purchased product categories
- Bar chart: Browsing frequency distribution
- Bar chart + Pie chart: Shopping satisfaction level distribution
- Heatmap: Correlation between recommendation helpfulness and satisfaction
- Key insight: Customer engagement is strong, but personalized recommendations and reviews currently have limited impact on satisfaction

---

## 🛠️ Tech Stack

- **Language:** Python
- **Libraries:** pandas, numpy, matplotlib, scikit-learn (KMeans, StandardScaler)

---

## ▶️ How to Run

1. Clone the repository
2. Place `eBay.csv` in the root directory
3. Open and run `ebay_analysis.ipynb` in Jupyter Notebook or JupyterLab

---

## 💡 Key Findings

- Most customers are medium-to-high frequency buyers with moderate satisfaction
- Groceries and Beauty are the most purchased categories
- High shipping costs are the leading cause of cart abandonment
- Personalized recommendations show near-zero correlation with satisfaction — significant room for improvement
- At-Risk Customers make up 63% of the dataset, highlighting a retention opportunity

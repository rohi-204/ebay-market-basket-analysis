# eBay Customer Behavior & Market Basket Analysis

## Project Overview

This project analyzes **800 e-commerce customer survey records** to
understand purchasing behavior, customer satisfaction, recommendation
and review perceptions, customer segments, and multi-category purchasing
patterns.

The analysis was rebuilt with a focus on correct data preparation,
transparent assumptions, interpretable customer segmentation,
statistical validation, and business-oriented findings.

> **Data-source note:** The supplied project materials contain
> inconsistent marketplace naming (eBay/Amazon). This repository uses
> **eBay** in the project title, but marketplace-specific conclusions
> should be interpreted according to the original dataset/source
> documentation.

## Project Objectives

1.  Data Cleaning and Preparation
2.  Descriptive Behavior Analysis
3.  Customer Segmentation and Profiling
4.  Recommendation and Review Insights
5.  Visualization and Reporting
6.  Market Basket Analysis / Association Rules

## Dataset

-   **Records:** 800
-   **Original fields:** 24
-   **Unique transaction IDs:** 800
-   **Exact duplicate rows:** 0
-   **Product-category selections:** 2,045
-   **Unique product categories:** 5

## Task 1 --- Data Cleaning and Preparation

### Work Performed

-   Audited column names and removed hidden trailing whitespace.
-   Resolved the duplicate `Personalized_Recommendation_Frequency` name
    by retaining the second 1--5 field as
    `Personalized_Recommendation_Frequency_2`.
-   Verified **800 unique transaction IDs** and **0 exact duplicate
    rows**.
-   Identified **161 missing values (20.12%)** in
    `Product_Search_Method` and preserved them as `Unknown` rather than
    distorting the distribution with mode imputation.
-   Standardized 91 `"."` responses in `Service_Appreciation` and 50 in
    `Improvement_Areas` as `Not Specified`.
-   Standardized inconsistent capitalization in `Product_Search_Method`.
-   Validated numeric 1--5 scales for customer-review importance,
    recommendation frequency, rating accuracy, and shopping
    satisfaction.

### Result

After cleaning: **800 records retained, 0 true missing values, 0
duplicate records, 0 duplicate transaction IDs, and 0 duplicate column
names**.

### Data Limitation

Age ranged from **3--67**. **179 respondents (22.38%)** were under 18.
Because the source supplied no adult-only restriction, these records
were retained rather than removed using an unsupported assumption.

## Task 2 --- Descriptive Behavior Analysis

### Demographics

Mean age was **35.73 years** and median age was **37**. Respondents aged
35--64 represented **50.12%** of the sample.

  Age Group     Count   Percentage
  ----------- ------- ------------
  Under 18        179       22.38%
  18--24           86       10.75%
  25--34          103       12.88%
  35--44          132       16.50%
  45--54          141       17.62%
  55--64          128       16.00%
  65+              31        3.88%

Gender representation was relatively balanced:

  Gender                Count   Percentage
  ------------------- ------- ------------
  Others                  209       26.12%
  Prefer not to say       202       25.25%
  Female                  198       24.75%
  Male                    191       23.88%

The largest-to-smallest gender difference was only **2.24 percentage
points**.

### Purchase Frequency

  Purchase Frequency         Count   Percentage
  ------------------------ ------- ------------
  Less than once a month       172       21.50%
  Few times a month            172       21.50%
  Once a month                 160       20.00%
  Multiple times a week        148       18.50%
  Once a week                  148       18.50%

**37% of respondents purchased at least weekly.**

### Product Categories

Multi-category responses were split and exploded so secondary selections
were not lost.

  Product Category               Respondents   \% of Respondents
  ---------------------------- ------------- -------------------
  Clothing and Fashion                   450              56.25%
  Home and Kitchen                       421              52.63%
  Beauty and Personal Care               418              52.25%
  Others                                 390              48.75%
  Groceries and Gourmet Food             366              45.75%

There were **2,045 category selections across 800 respondents**,
averaging **2.56 categories per respondent**. Percentages exceed 100% in
total because customers could select multiple categories.

### Browsing and Search

Browsing frequency was balanced: Rarely **26.25%**, Multiple times a day
**24.88%**, Few times a week **24.88%**, Few times a month **24.00%**.

Product search methods: Keyword **21.88%**, Others **20.50%**, Unknown
**20.12%**, Categories **19.75%**, Filter **17.75%**. The 20.12%
originally missing responses limit strong conclusions about search
preferences.

### Cart Abandonment

High shipping costs accounted for **26%**, other reasons **25.5%**,
changed mind/no longer needed **25.5%**, and found a better price
elsewhere **23%**. Combined price/shipping-related reasons represented
**49% of reported abandonment reasons**.

### Satisfaction and Rating Measures

  Metric                       Scale     Mean   Median
  ---------------------------- ------- ------ --------
  Shopping Satisfaction        1--5      2.87        3
  Rating Accuracy              1--5      3.09        3
  Recommendation Helpfulness   1--3      1.99        2

Recommendation helpfulness was encoded `No=1`, `Sometimes=2`, `Yes=3`
while retaining the original field.

## Task 3 --- Customer Segmentation and Profiling

### Rule-Based Segmentation

Purchase frequency was scored from 1 (less than once a month) to 5
(multiple times a week).

-   **At-Risk Customers:** Satisfaction ≤ 2
-   **Frequent Buyers:** Purchase score ≥ 4 and Satisfaction ≥ 4
-   **Occasional Shoppers:** Purchase score 2--3 and Satisfaction ≥ 3
-   **Other Customers:** Remaining combinations

  Segment                 Count   Percentage
  --------------------- ------- ------------
  At-Risk Customers         351       43.88%
  Occasional Shoppers       187       23.38%
  Other Customers           153       19.12%
  Frequent Buyers           109       13.63%

`At-Risk` is a rule-based analytical label and does **not** mean 43.88%
will churn.

### Segment Profiles

  -------------------------------------------------------------------------------
  Segment           Avg Age     Purchase   Satisfaction       Rating       Review
                                   Score                    Accuracy   Importance
  ------------ ------------ ------------ -------------- ------------ ------------
  At-Risk             36.88         2.95           1.46         3.13         3.00

  Frequent            33.39         4.54           4.48         2.94         2.98
  Buyers                                                             

  Occasional          34.98         2.49           3.95         3.07         3.03
  Shoppers                                                           

  Other               35.67         2.25           3.63         3.09         2.98
  Customers                                                          
  -------------------------------------------------------------------------------

High shipping costs were reported by **28.34% of Occasional Shoppers**,
while Frequent Buyers had the highest share reporting a better price
elsewhere (**26.61%**).

### K-Means Clustering

K-Means used four standardized behavioral features: Purchase Frequency
Score, Shopping Satisfaction, Customer Reviews Importance, and Rating
Accuracy.

Silhouette scores from K=2 through K=8 ranged from **0.1894 to 0.2305**.
Although K=8 had the highest tested silhouette score, separation
remained weak overall. The elbow curve showed diminishing improvements
around 4--5 clusters, so **K=4 was retained as an interpretable
solution**, not claimed as a mathematically proven optimum.

  -----------------------------------------------------------------------------------------
  Cluster         Size   Purchase   Satisfaction       Review     Rating Profile
                                                   Importance   Accuracy 
  --------- ---------- ---------- -------------- ------------ ---------- ------------------
  0                194       3.98           1.55         3.76       3.09 Frequent but
              (24.25%)                                                   Dissatisfied

  1                203       4.09           4.16         2.55       3.16 Satisfied Frequent
              (25.37%)                                                   Buyers

  2                210       1.96           2.19         1.67       3.14 Low-Engagement
              (26.25%)                                                   At-Risk

  3                193       1.69           3.57         4.17       2.94 Satisfied
              (24.12%)                                                   Review-Conscious
                                                                         Shoppers
  -----------------------------------------------------------------------------------------

**91.24% of Cluster 0 overlapped with the rule-based At-Risk segment.**
This is overlap between two segmentation approaches, not model accuracy.

## Task 4 --- Recommendation and Review Insights

### Recommendation Helpfulness vs Satisfaction

  Helpfulness     Customers   Mean Satisfaction   Median
  ------------- ----------- ------------------- --------
  No                    276                2.90        3
  Sometimes             259                2.80        3
  Yes                   265                2.90        3

Spearman **ρ ≈ 0.000, p = 0.9894**, showing no detectable monotonic
association in this sample.

### Review Reliability vs Rating Accuracy

Spearman **ρ = 0.068, p = 0.0549**. This was only a very weak positive
association and was not statistically significant at the conventional
0.05 level.

### Review Helpfulness vs Rating Accuracy

Mean rating accuracy was 3.03 for No, 3.07 for Sometimes, and 3.16 for
Yes. Spearman **ρ = 0.036, p = 0.3065**, indicating no meaningful
monotonic association.

### Personalized Recommendation Engagement

Recommendation exposure was nearly balanced: No **34.88%**, Yes
**32.62%**, Sometimes **32.50%**.

Frequency vs helpfulness: **χ² = 7.247, df = 4, p = 0.1234, Cramér's V =
0.067**. The association was not statistically significant and the
effect size was very weak.

### Actionable Insights

1.  **Test recommendation relevance rather than simply increasing
    exposure.** Frequency alone was weakly related to perceived
    helpfulness.
2.  **Investigate price and shipping friction.** Price/shipping-related
    reasons represented **49%** of reported abandonment reasons.
3.  **Prioritize the Frequent but Dissatisfied profile for retention
    investigation.** This cluster represented **24.25%** of respondents
    with purchase score **3.98/5** but satisfaction only **1.55/5**.
4.  **Treat review-system changes as hypotheses to test.** Review
    reliability/helpfulness showed only weak relationships with rating
    accuracy.

These are areas for testing and investigation; the survey does not
establish causal effects.

## Task 5 --- Visualization and Reporting

Notebook visualizations include age and gender distributions, purchase
frequency, popular categories, browsing frequency, search methods,
cart-abandonment factors, satisfaction, recommendation helpfulness vs
satisfaction, customer segments, Elbow Method, Silhouette Scores, and a
Spearman correlation heatmap.

The behavioral correlation matrix showed all observed off-diagonal
correlations approximately between **-0.06 and +0.07**, so no strong
pairwise monotonic relationships were observed.

## Task 6 --- Market Basket Analysis

The multi-category `Purchase_Categories` field is correctly preserved
for association-rule analysis.

Current preparation:

-   **800 respondents**
-   **2,045 category selections**
-   **5 unique categories**
-   **2.56 categories per respondent on average**

The next MBA stage should calculate **frequent itemsets, support,
confidence, lift, and association rules**.

> **Status:** Final support/confidence/lift findings are intentionally
> not claimed yet because Task 6 association rules have not been
> executed.

## Key Project Findings

-   Validated **800 records** with **100% unique transaction IDs**.
-   Handled **161 missing search-method responses (20.12%)**.
-   Preserved **2,045 multi-category selections** instead of losing
    secondary product categories.
-   **37%** of respondents purchased at least weekly.
-   Clothing & Fashion was selected by **56.25%** of respondents.
-   Price/shipping-related factors represented **49%** of reported
    cart-abandonment reasons.
-   Rule-based At-Risk segment represented **43.88%** of respondents.
-   K-Means produced four balanced profiles spanning approximately
    **24.1%--26.3%** of respondents.
-   Identified a **24.25% Frequent but Dissatisfied cluster** with
    purchase score **3.98/5** and satisfaction **1.55/5**.
-   Recommendation helpfulness showed virtually no monotonic association
    with satisfaction (**ρ ≈ 0.000, p = 0.9894**).
-   Recommendation frequency and helpfulness showed a very weak,
    non-significant association (**Cramér's V = 0.067, p = 0.1234**).

## Technologies

**Python · Pandas · NumPy · Matplotlib · SciPy · Scikit-learn · Jupyter
Notebook**

Techniques: data cleaning, descriptive statistics, rule-based
segmentation, K-Means, StandardScaler, Elbow Method, Silhouette Score,
cross-tabulation, Spearman correlation, Chi-square testing, Cramér's V,
and Market Basket / Association Rule Analysis.

## Limitations

-   Survey results show associations, not causal effects.
-   Under-18 respondents were retained because no minimum-age rule was
    supplied.
-   `Product_Search_Method` originally had 20.12% missing responses.
-   K-Means silhouette scores were low, indicating weak natural cluster
    separation.
-   Rule-based segments depend on explicitly defined thresholds and are
    not ground-truth churn labels.
-   Supplied project materials contain inconsistent marketplace naming;
    confirm the authoritative source before external publication.
-   MBA support/confidence/lift results must be added after Task 6 is
    completed.

## Suggested Repository Structure

``` text
eBay-Customer-Behavior-Market-Basket-Analysis/
├── README.md
├── notebooks/
│   └── ebay_customer_behavior_analysis.ipynb
├── data/
│   └── dataset.csv
├── outputs/
│   ├── charts/
│   └── tables/
└── requirements.txt
```

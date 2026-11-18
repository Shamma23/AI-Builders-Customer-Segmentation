# Customer Segmentation & Product Recommendation

### Unsupervised Learning Capstone \| Online Retail Dataset

## Project Overview

This project applies unsupervised machine learning techniques to online
retail transaction data to identify customer segments, discover
purchasing patterns, and develop a product recommendation approach.

The goal is to transform raw transaction data into meaningful business
insights that can support customer retention, targeted marketing,
cross-selling, and personalized product recommendations.

## 1. Business Problem

An online retail business has a large volume of historical transaction
data but limited understanding of its customers' purchasing behavior.

The business needs to answer four important questions:

-   Who are the different types of customers?
-   Which products are frequently purchased together?
-   What products should be recommended to individual customers?
-   How can these insights support better business decisions?

Since customer segments and purchasing patterns are not predefined,
unsupervised learning techniques are appropriate for discovering hidden
patterns in the data.

## 2. Dataset Description

**Dataset:** UCI Online Retail Dataset

The dataset contains historical transactions from a UK-based online
retail business selling gifts and household products.

**Period:** December 2010 -- December 2011

**Original dataset size:** 541,909 rows and 8 columns.

### Main Features

  Feature       Description
  ------------- -------------------------------
  InvoiceNo     Transaction or invoice number
  StockCode     Product identifier
  Description   Product description
  Quantity      Number of items purchased
  InvoiceDate   Transaction date and time
  UnitPrice     Price per item
  CustomerID    Customer identifier
  Country       Customer's country

The dataset includes missing values, duplicate records, cancelled
transactions, returns, and invalid values. Data exploration and cleaning
were therefore important parts of the project.

### Dataset Access

The original dataset is not included in this repository. Download the
Online Retail dataset from the source provided with the capstone
materials and place the Excel file at:

`data/online_retail.xlsx`

The notebook expects this relative file path.

## 3. Techniques Used

### Data Exploration and Preprocessing

-   Examined dataset structure, distributions, missing values, and
    duplicates.
-   Investigated cancelled transactions, returns, negative quantities,
    and invalid prices.
-   Applied appropriate filtering and cleaning decisions for
    customer-level and product-level analysis.

### Customer Segmentation --- RFM and K-Means

Created three customer-level features:

-   **Recency:** How recently a customer made a purchase, measured in
    days.
-   **Frequency:** How often a customer purchased, measured as the
    number of distinct invoices.
-   **Monetary:** How much a customer spent during the analysis period.

Applied log transformation and standardization before using K-Means
clustering.

The number of clusters was evaluated using:

-   Elbow Method
-   Silhouette Score

The Elbow Method suggested a reasonable range around four to six
clusters. Although K=2 achieved the highest silhouette score, four
clusters were selected to provide more detailed and actionable customer
profiles for business interpretation. This choice prioritised business
usefulness while considering cluster separation.

### PCA Visualization

Applied Principal Component Analysis (PCA) to visualize customer
segments in two dimensions.

The first two principal components retained **93.88% of the total
variance**. PCA was used for visualization; the K-Means model was fitted
using all three standardized RFM features.

### Market Basket Analysis

Used the Apriori algorithm and association rules to discover products
frequently purchased together.

The analysis examined:

-   Support
-   Confidence
-   Lift

### Product Recommendation System

Developed a hybrid recommendation approach combining:

-   Personalized association-rule recommendations.
-   Popular-product recommendations as a fallback.

This approach also addresses the cold-start problem when a customer has
no previous purchase history.

### Recommendation Evaluation

Used a chronological train-test split to evaluate recommendation
performance through:

-   Precision@5
-   Recall@5
-   Recommendation coverage

## 4. Main Findings

### Customer Segmentation

The analysis identified four customer segments among **4,338
customers**.

  --------------------------------------------------------------------------------
  Customer Segment          Customers        Average        Average        Average
                                      Recency (days)      Frequency Monetary Value
                                                         (invoices)          (GBP)
  -------------------- -------------- -------------- -------------- --------------
  Recent, Low-Value               808          17.06           2.12        £540.35
  Customers                                                         

  Less-Recent,                  1,642         179.43           1.32        £347.61
  Low-Activity                                                      
  Customers                                                         

  High-Value, Frequent            685          10.39          13.99      £8,290.35
  Customers                                                         

  Moderate-Activity,            1,203          67.80           4.21      £1,829.55
  Mid-Value Customers                                               
  --------------------------------------------------------------------------------

*Recency is the average number of days since the customer's last
purchase at the dataset reference date. Frequency is the average number
of distinct invoices. Monetary value is the average customer spend
across the analysis period.*

The largest segment consists of less-recent, low-activity customers,
representing an opportunity for customer reactivation campaigns.

High-value, frequent customers represent an important group for
retention and personalized marketing.

### Purchasing Patterns

Market Basket Analysis identified strong associations between several
product combinations.

Examples include:

  -----------------------------------------------------------------------
  Product Association                Confidence                      Lift
  ------------------- ------------------------- -------------------------
  Green Regency                          75.72%                     14.19
  Teacup and Saucer →                           
  Roses Regency                                 
  Teacup and Saucer                             

  Pink Regency Teacup                    82.61%                     16.28
  and Saucer → Green                            
  Regency Teacup and                            
  Saucer                                        

  Pink Regency Teacup                    78.17%                     14.65
  and Saucer → Roses                            
  Regency Teacup and                            
  Saucer                                        
  -----------------------------------------------------------------------

These associations suggest opportunities for complementary product
recommendations and product bundles. Association rules indicate
purchasing relationships; they do not establish causation.

### Recommendation Performance

The hybrid recommendation approach was compared with an association-rule
baseline using a chronological test period.

  Evaluation Metric                            Association-Rule Baseline   Hybrid Approach
  ------------------------------------------ --------------------------- -----------------
  Precision@5                                                      7.03%             7.53%
  Recall@5                                                         1.02%             1.13%
  Customers receiving five recommendations                         1,432             1,893

The hybrid approach achieved modest improvements in Precision@5 and
Recall@5 and provided five recommendations to all 1,893 evaluated
returning customers.

These are offline evaluation results and require further testing with
actual customers.

### Example: Personalized Product Recommendations

For the selected customer (**CustomerID 12748**), the recommendation
approach generated the following five products:

1.  JAM JAR WITH PINK LID
2.  PINK POLKADOT BOWL
3.  RED STRIPE CERAMIC DRAWER KNOB
4.  RED SPOT CERAMIC DRAWER KNOB
5.  WHITE SPOT BLUE CERAMIC DRAWER KNOB

These recommendations were generated from association rules matched
against the customer's purchase history. They are an illustrative
customer-level output, not a general popularity ranking or a guarantee
that the customer will purchase the products.

## 5. Business Recommendations

Based on the analysis, the following actions are recommended:

**1. Customer Retention**

-   Prioritize high-value, frequent customers.
-   Provide personalized offers and early access to selected products.
-   Monitor purchasing activity to help retain valuable customers.

**2. Customer Reactivation**

-   Target less-recent, low-activity customers with win-back campaigns.
-   Offer relevant promotions to encourage repeat purchases.

**3. Encourage Repeat Purchases**

-   Target recent, low-value customers with complementary product
    suggestions.
-   Use personalized promotions to encourage additional purchases.

**4. Product Bundling and Cross-Selling**

-   Test bundles using strong product associations.
-   Display complementary products on product pages and during checkout.

**5. Personalized Recommendations**

-   Pilot the hybrid recommendation approach.
-   Use popular products when customer purchase history is unavailable.
-   Measure recommendation clicks, conversion rates, repeat purchases,
    and revenue.

## 6. Limitations

-   The analysis is based on historical transactions from a single year.
-   Customer segments represent observed purchasing behavior during the
    analysis period.
-   Association rules identify relationships, not causation.
-   Recommendation evaluation used one chronological test period and
    returning customers.
-   Offline recommendation performance does not guarantee improved sales
    or engagement.
-   The recommendation evaluation does not establish the effect of
    recommendations on actual customer behavior or incremental revenue.

Further testing with live customer interactions is required before full
business implementation.

## 7. Tools and Technologies

-   Python
-   Pandas
-   NumPy
-   Matplotlib
-   Seaborn
-   Scikit-learn
-   MLxtend
-   Jupyter Notebook

## 8. Project Structure

``` text
AI_BUILDERS_Customer_Segmentation/
│
├── data/
│   └── online_retail.xlsx
│
├── venv/
├── .gitignore
├── README.md
├── requirements.txt
└── Shamma_Unsupervised_Capstone.ipynb
```

The local virtual environment should not be committed to GitHub. The
dataset is also excluded from the repository unless its redistribution
permissions have been confirmed.

## 9. How to Run the Project

1.  Clone or download the repository.

2.  Create and activate a Python virtual environment.

3.  Install the required packages:

    ``` bash
    pip install -r requirements.txt
    ```

4.  Place the dataset in the `data/` folder and name it `online_retail.xlsx`.

5.  Open `Shamma_Unsupervised_Capstone.ipynb` in Jupyter
    Notebook or VS Code.

6.  Select the project's Python environment as the notebook kernel.

7.  Run the notebook cells from beginning to end.


## 10. Project Outcome

This project demonstrates an end-to-end unsupervised learning workflow,
from data exploration and preprocessing to customer segmentation,
purchasing-pattern discovery, recommendation development, evaluation,
and business interpretation.

It highlights how machine learning can support practical retail business
decisions through customer understanding and data-driven
recommendations.

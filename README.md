# E-commerce Customer Segmentation & Prediction 🛒

*A Clustering & Machine Learning Analysis of Online Retail Data*

**Note:** *This repository contains the executive presentation and documentation of the analysis. The raw Jupyter notebooks have been omitted to focus on business insights and high-level technical architecture.*

## 📖 Project Overview
E-commerce businesses frequently struggle to decode diverse customer purchasing behaviors, which often results in generic marketing, decreased retention, and inefficient inventory management.

The objective of this capstone project is to implement a data-driven segmentation approach. By classifying customers based on their real purchasing patterns, businesses can enable personalized marketing, maximize retention, and optimize resource allocation.

## 🗄️ Dataset Overview
The analysis utilizes approximately one year of online retail transaction data (December 1, 2010 – December 9, 2011).
*   **Raw Data:** 541,909 transaction line items.
*   **Cleaned Data:** 392,692 transaction lines representing 4,338 unique customers.
*   **Scope:** 38 countries, with the United Kingdom accounting for the large majority of transactions.

### Key Features
| Column | Description | Data Type |
| :--- | :--- | :--- |
| `InvoiceNo` | Unique identifier for each transaction | Object |
| `StockCode` | Unique product code | Object |
| `Description` | Name/description of the product | Object |
| `Quantity` | Number of items purchased | Integer |
| `InvoiceDate` | Date and time of the transaction | Object |
| `UnitPrice` | Price per unit of the product | Float |
| `CustomerID` | Unique identifier for each customer | Float |
| `Country` | Country where the transaction occurred | Object |

## 🛠️ Methodology
The project pipeline moves from raw data processing to unsupervised clustering, and finally to supervised predictive modeling and explainability:

1.  **Data Cleaning:** Handled missing `CustomerID`s, removed cancelled orders (representing returns), filtered out non-positive quantities/prices, and engineered a `TotalPrice` feature (`Quantity` × `UnitPrice`).
2.  **Feature Engineering (RFM):** Transformed transaction data into customer profiles based on:
    *   **Recency:** Days since the customer's last purchase (Avg: 93 days).
    *   **Frequency:** Total number of distinct orders placed (Avg: 4 orders).
    *   **Monetary:** Total amount spent across all transactions (Avg: £2,049).
3.  **Outlier Handling:** Instead of deleting valuable extreme wholesale orders, outliers were capped at the Interquartile Range (IQR) bounds to prevent distortion in distance-based clustering algorithms.
4.  **Clustering:** Evaluated K-Means, Hierarchical (Ward linkage), and DBSCAN. **K-Means (k=3)** was selected as the optimal algorithm, achieving the highest Silhouette Score (0.510).
5.  **Dimensionality Reduction:** Used Principal Component Analysis (PCA) to reduce the 3 RFM features into 2 dimensions for clear cluster visualization, retaining 94.2% of the variance.

## 📊 Customer Segments Identified
The K-Means algorithm grouped the customer base into three distinct, actionable segments:

| Segment | Customer Count | Avg Recency | Avg Frequency | Avg Monetary |
| :--- | :--- | :--- | :--- | :--- |
| 🥇 **Best Customers** | 964 (22%) | 27 days | 8.2 | £2,999.1 |
| 🛍️ **Occasional Shoppers** | 2,328 (54%) | 49 days | 2.5 | £726.1 |
| ⚠️ **At-Risk / Churned** | 1,046 (24%) | 245 days | 1.5 | £426.6 |

## 🤖 Predictive Modeling & Explainability
To classify *new* customers on the fly, supervised machine learning models were trained using the K-Means cluster labels as the ground truth.

### Model Performance
*   **Logistic Regression:** 99.2% Accuracy
*   **Random Forest:** 98.8% Accuracy
*   **XGBoost:** 98.7% Accuracy

*The exceptionally high accuracy confirms that the RFM segments are highly distinct, clear, and well-defined, rather than random noise.*

### Feature Importance (SHAP)
SHAP (SHapley Additive exPlanations) was used to extract game-theoretic explainability from the model:
*   **Recency** is the most important feature overall, serving as the strongest predictor for identifying At-Risk/Churned customers.
*   **Monetary** and **Frequency** are the primary drivers for classifying a user into the "Best Customers" segment.

## 💡 Business Recommendations
Based on the data, the following targeted marketing strategies are recommended:

*   **Best Customers:** Enroll in VIP/loyalty programs, offer early access to products, and focus on long-term retention rather than just upselling.
*   **Occasional Shoppers:** Utilize targeted cross-selling and personalized incentives (e.g., "Spend X more to unlock Y") to increase their purchase frequency.
*   **At-Risk / Churned Customers:** Launch aggressive win-back campaigns, prioritizing customers who previously showed high potential. Optimize marketing spend by deprioritizing long-dormant, low-value accounts.

---

## 👨‍💻 Author
**Jeet Sarkar** 
* [LinkedIn](https://www.linkedin.com/in/jeetsarkar0004/)

# Project Background
This project focuses on customer segmentation using big data from Amazon electronics product reviews. It integrates product metadata with customer review data using Spark SQL joins and applies feature engineering and unsupervised learning to identify behavioral customer segments.

## Approach
The project includes an end-to-end workflow: 
- ETL
- Exploratory analysis and data cleaning with PySpark
- Feature engineering using RFM (Recency, Frequency, Monetary) metrics
- Clustering via K-Means using MLlib
- Business recommendations based on both cluster and RFM analysis

Technical implementation is documented in two separate Jupyter Notebooks:  
- **EDA Notebook** – for exploration, SQL querying, and data profiling  
- **Modeling Notebook** – for feature engineering, clustering, and segment interpretation

## Tools & Technologies
- **PySpark** – for distributed data cleaning, joining, and feature engineering
- **Spark SQL** – for scalable querying, including complex joins and aggregations
- **MLlib** – for K-Means clustering and unsupervised segmentation
- **Pandas, Matplotlib, Seaborn** – for additional analysis and visualization, using colorblind-friendly templates

## Data
- The dataset contains information Amazon product user reviews and item metadata between May 1996 to September 2023.
- The data was scraped from Amazon by [McAuley Lab at UC San Diego.](https://huggingface.co/datasets/McAuley-Lab/Amazon-Reviews-2023) While there are other product categories available, this project will use electronics product data.
- The dataset contains 43,886,944 rows and 10 columns in the product reviews dataset and 1,610,012 rows and 16 columns in the product metadata dataset.

## Links
- [EDA Jupyter Notebook](https://github.com/nvpham12/Amazon-Big-Data-Customer-Segmentation/blob/main/EDA_Amazon_Big_Data.ipynb)
- [Segmentation Machine Learning Model Jupyter Notebook](https://github.com/nvpham12/Amazon-Big-Data-Customer-Segmentation/blob/main/Customer_Segmentation_Amazon_Big%20Data.ipynb)
- [Full Technical Report](https://github.com/nvpham12/Amazon-Big-Data-Customer-Segmentation/blob/main/Technical%20Report.md)

# Executive Summary
## Insights
- Some customer segments appear to be incorrectly placed within clusters (e.g., Champions grouped with One-Time Big Spenders, or Lost customers under New Customers), suggesting imperfect alignment between RFM segmentation and K-Means clustering.
- Considering the K-Means Clustering has a moderate Silhouette Score, it may be making poor predictions and causing the irregularities. These inconsistencies may indicate that the chosen number of clusters (k=4) is insufficient or that the K-Means algorithm is not capturing the underlying structure of the data.
- Most of Amazon's customers fall under churn-related segments (e.g., About to Sleep, Hibernating, Lost), highlighting opportunities for targeted re-engagement and retention strategies.

## Recommendations
- Those in the **New Customers** cluster or in the **About to Sleep**, **Needs Attention**, **At Risk**, and **Can't Lose Them** segments should also be targeted with an offer of 1 month free/trial Amazon Prime if they don't have a subscription already and discounted items. This could be done by showing discounted products in their product recommendations (on the front page) or sending an email or message offering a month of free Amazon Prime.
- Marketing campaigns should be launched to find **New Customers** and to reach out to **Churned Customers** in an attempt to rekindle interest.
- Consider implementing a loyalty program providing points based on per dollar spending that can be exchanged for rewards could be implemented. This would reward **Loyal Customers**, potentially retain **New Customers**, and incentivize more customer spending.
- Consider implementing an enhanced cash back program for Loyal Customers. Currently, Amazon offers cashback only to customers who make purchases using an Amazon Chase Visa. Amazon should consider rolling out a cashback program to users without the Amazon credit card at lower rates such as 1% for all customers or 2% for customers with Prime. To prevent abuse, the cashback could be given to customers after the return periods for the item closes. Amazon could also try setting different cashback rates according to spending levels such as Bronze, Silver, Gold, and Diamond tiers.

## Next Steps
- Test additional cluster values (e.g., k=6–10) to evaluate whether deeper segmentation yields more interpretable or actionable groupings.
- Develop a predictive churn model using supervised learning to identify at-risk customers based on historical behavior.
- Perform sentiment analysis on review text and map sentiment trends to customer segments for richer behavioral insight.

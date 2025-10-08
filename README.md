# Project Background
This project demonstrates distributed computing, querying, and customer segmentation on big data. K-Means Clustering and RFM (Recency, Frequency, Monetary) Analysis are used to group customers based on spending habits. The K-Means model had questionable performance, so segmentation based on RFM Analysis was done. Data analysis for this project focuses on these segments instead of the clusters.

## Links
### EDA Jupyter Notebook
- [EDA Notebook](https://github.com/nvpham12/Amazon-Big-Data-Customer-Segmentation/blob/main/EDA%20Amazon%20Electronics%20Big%20Data.ipynb)

### K-Means Clustering
- [K-Means Clustering Notebook](https://github.com/nvpham12/Amazon-Big-Data-Customer-Segmentation/blob/main/K-Means%20Clustering%20Notebook.ipynb)
- [K-Means Clustering Technical Report](https://github.com/nvpham12/Amazon-Big-Data-Customer-Segmentation/blob/main/K-Means%20Clustering%20Technical%20Report.md)

### RFM Customer Segmentation
- [RFM Segmentation Notebook](https://github.com/nvpham12/Amazon-Big-Data-Customer-Segmentation/blob/main/RFM%20Segmentation%20Amazon%20Electronics.ipynb)
- [RFM Segmentation Technical Report](https://github.com/nvpham12/Amazon-Big-Data-Customer-Segmentation/blob/main/RFM%20Segmentation%20Technical%20Report.md)

### Data Analytics
- [Data Analytics Report](https://github.com/nvpham12/Amazon-Big-Data-Customer-Segmentation/blob/main/Data%20Analytics%20Report.md)

## Tools & Technologies
- **PySpark**: distributed computing and big data manipulation
- **Spark SQL**: SQL style querying within PySpark framework
- **MLlib**: K-Means clustering and unsupervised learning
- **Pandas**: data manipulation and extracting samples from PySpark dataframe for visualization
- **Matplotlib and Seaborn**: data visualization

## Approach
- An ETL process is used to obtain the data from the Hugging Face Repository, transform it to parquet file type, and load it into PySpark.
- Exploratory Data Analysis and cleaning are done through querying within PySpark.
- K-Means clustering and RFM Analysis are applied for customer segmentation.
- Customer segments and data distributions are visualized and analyzed.

## Data
- The data consists of Amazon product user reviews and item metadata between May 1996 to September 2023.
- The reviews dataset contains 43,886,944 rows and 10 columns.
- The product metadata contains 1,610,012 rows and 16 columns.

## Project Limitations & Mitigations
- **Review timing:** Reviews aren’t necessarily left at the time of purchase.
  - **Mitigation:** Used review dates from verified customers as a proxy for purchase dates to compute recency.  
- **Volunteer bias:** Not all customers leave reviews; analysis reflects only engaged, reviewing customers.  
- **Frequency calculation:** Does not account for multiple quantities per purchase.  
- **Monetary value estimation:** Prices of reviewed products may not reflect actual amounts paid due to discounts or price changes.
  - **Mitigation:** Used product prices at the time of scraping as a proxy for spending.  
- **Subset of customers:** Only verified reviewers are included. Analysis focuses on this engaged subset.  
- **Missing purchase info:** Data lacks actual purchase amounts and dates.
  - **Mitigation:** Review dates and product prices serve as proxies to approximate recency, frequency, and monetary features for RFM analysis.

---

# Executive Summary
## Insights
- **Can't Lose Them** customers should be prioritized since they have on average, high levels of spending per capita.
- **About to Sleep** customers have good spending per capita and are the second most important group among customers at risk of churning.
- **Needs Attention** and **At Risk** customers are customers at risk of churning. However, they have low levels of spending.
- 36% of customers are regular or high value customers (**Champions**, **Loyalists**, and **Potential Loyalists**)
- 33% of customers are at risk of churn but haven't churned yet (**About to Sleep**, **At Risk**, **Can't Lose Them**, and **Needs Attention**).
- 30% of customers have already churned (**Lost** and **Hibernating**).

## Recommendations
- Customers in the **About to Sleep** and **Can't Lose Them** segments should be targeted with discounts and promotions. This could be done by showing discounted products in their product recommendations (on the front page) or offering these customers a 1-month trial of Amazon Prime.
- Customers categorized as **Can't Lose Them** should be focused on, since they are at risk of churning and have high levels of spending.
- Customers in the **Needs Attention** and **At Risk** segments require more careful consideration. They are at risk of churning, but have low average and total spending. The benefits of retaining them may not be worth the costs of doing so.

---

# Data Source
Dataset: Amazon Reviews 2023

Authors: McAuley Lab at University of California, San Diego

Source: [Hugging Face Repository](https://huggingface.co/datasets/McAuley-Lab/Amazon-Reviews-2023)

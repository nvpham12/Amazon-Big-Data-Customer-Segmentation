# Project Background
This project focuses on customer segmentation using big data from Amazon electronics product reviews. It integrates product metadata with customer review data using PySpark and Spark SQL joins, applying feature engineering and unsupervised learning to identify behavioral customer segments.

## Approach
The project includes an end-to-end workflow: 
- ETL
- Exploratory analysis and data cleaning with PySpark
- Feature engineering using RFM (Recency, Frequency, Monetary) metrics
- Clustering via K-Means using MLlib
- RFM segmentation
- Business recommendations based on both cluster and RFM analysis

Technical implementation is documented in two separate Jupyter Notebooks:  
- **EDA Jupyter Notebook** – for exploration, SQL querying, data profiling and cleaning  
- **Modeling JupyterNotebook** – for feature engineering, clustering, and segment interpretation

## Tools & Technologies
- **PySpark** – for distributed data cleaning, joining, and feature engineering
- **Spark SQL** – for scalable querying, including complex joins and aggregations
- **MLlib** – for K-Means clustering and unsupervised segmentation
- **Pandas, Matplotlib, Seaborn** – for additional analysis and visualization, using colorblind-friendly templates

## Data
- The dataset contains information Amazon product user reviews and item metadata between May 1996 to September 2023.
- The data was scraped from Amazon by [McAuley Lab at UC San Diego.](https://huggingface.co/datasets/McAuley-Lab/Amazon-Reviews-2023) While there are other product categories available, this project will use electronics product data.
- The product reviews dataset has 43,886,944 rows and 10 columns.
- The product metadata dataset has 1,610,012 rows and 16 columns.

## Links
- [EDA Jupyter Notebook](https://github.com/nvpham12/Amazon-Big-Data-Customer-Segmentation/blob/main/EDA_Amazon_Big_Data.ipynb)
- [Segmentation Machine Learning Model Jupyter Notebook](https://github.com/nvpham12/Amazon-Big-Data-Customer-Segmentation/blob/main/Customer_Segmentation_Amazon_Big%20Data.ipynb)

# ETL
The data was first extracted from the [source](https://huggingface.co/datasets/McAuley-Lab/Amazon-Reviews-2023) using Hugging Face's datasets library, then converted into parquet storage format. McAuley Lab scraped the data from Amazon. The transformation to parquet was required as the data was too large for Pandas, direct PySpark dataframe creation, or arrow tables. The parquet files were then read into PySpark.

# Exploratory Data Analysis (EDA)
Before modeling, exploratory SQL queries and visualizations were conducted to better understand product and review dynamics. Misplaced product categories and invalid rating values were discovered and cleaned.

![2023_popular_products](https://github.com/user-attachments/assets/2c738685-86dd-4cf9-9e15-a9a735bede04)
> In this project, product popularity is approximated using the number of user reviews as a proxy.
- Small electronic devices were the most popular in 2023. Wireless audio devices (earbuds/headphones) were by far the most popular.
- Other popular electronics in 2023 were flash drives and various home improvement products such as cameras and Echo Dots.

---
![rating_distribution](https://github.com/user-attachments/assets/a686da60-1d6a-4fb6-999b-7f49dfa784f8)
- Surprisingly, most product reviews come from users with verified purchases. This was unexpected given that there is no cost requirement for a user to make a review without a verified purchase.
- Users have a tendency to leave ratings at the extreme values (1 and 5).
- Most reviews are 5 star reviews, indicating that users tend to leave a review when they are satisfied with a product.

# Data Cleaning
- Converted missing value strings such as 'none' and 'n/a' into nulls.
- Dropped duplicates.
- Filtered data to last 2 years of observations (2022 and 2023) and only reviews with verified purchases.
- Removed incorrect observations and columns.
- Converted data types to proper format such as timestamp to seconds and price to double (a type of integer in PySpark).

# Feature Engineering
Recency, Frequency, and Monetary (RFM) metrics will be used as the features for the model. Since the data does not contain customer purchase dates, the traditional RFM metrics cannot be obtained. However, a work around is to use review date as a proxy for purchase time. Because the data includes an indicator for verified purchases, reviewers can be screened to find verified customers. The RFM features in this project will be represented as follows:
- Recency will measure how recent the review left by the customer is as a proxy for how recent a customer made a purchase.
- Frequency will measure how often a customer leaves reviews as a proxy for how often a customer made purchases.
- Monetary will measure how much a customer spends based on prices of the product they reviewed.

Analysis is subject to the following limitations:
- Reviews aren't necessarily left at time of purchase, but often left some time after the purchase.
- Not everyone leaves a review (volunteer bias).
- Frequency does not account for purchases of multiple quantities.
- The monetary feature is based on the price of the product reviewed, which may not perfectly reflect the price paid for the product. Products can change in price over time (or receive discounts), which isn't accounted for when scraping the data.

# Data Preprocessing
- The data is filtered further to a 1 year range between 9/13/2022 to 9/13/2023 based on the date of the most recent review in the data.
- Applied a log transformation on skewed features.
- Scaled the data using Standard Scaler.

# Modeling
The model will use K-Means Clustering, an unsupervised machine learning algorithm. K-Means initializes a selected k number of points called means or centroids. It then finds the nearest distances between each data point the nearest centroid (using Euclidean distance) updating the positions of centroids. The process is repeated until we have unchanging centroids or cluster assignments, or until we have reached the maximum number of iterations. Before fitting the model, K needs to be chosen. 2 commonly used techniques for this are the Elbow Method and Silhouette Method.

## Elbow Method
![wcss_elbow](https://github.com/user-attachments/assets/c30687ac-34f8-4e9c-8390-744c6dd61dec)
- The Elbow Method involves plotting within-cluster sum of squares (WCSS) against k and looking for the elbow, the point where the curve forms a kink or angle at some k.
- The angles appear in the curve at values of 3 and 4 for k.
- Since the curve appears linear after 4, choosing $k=4$ is the better option per Elbow Method guidlines.

## Silhouette Method
![silhoutte_score](https://github.com/user-attachments/assets/82dfa82a-c228-41d0-ab94-b378bad8cd7a)
- The value of $k$ with the highest Silhouette Score is 2.
- The Silhouette Scores where $k=4$ and $k=6$ are lower, but not too far off from the score for $k=2$.
- Choosing $k=2$ would be best, but $k=4$ and $k=6$ can be chosen as well per Silhouette Method guidelines.

## Choice of k Clusters
From analysis of results from both methods, we would choose $k=4$ clusters, since it is the best choice from the Elbow Method and the Silhouette Score is not too far off from the highest score at $k=2$.

# K-Means Cluster Segments
Clusters were labeled based on analysis of the average values for each RFM metric in each cluster. The 4 clusters are Churned, New Customers, Loyal Customers, and One Time Big Spenders. 

![segment_pie](https://github.com/user-attachments/assets/695491ee-89f4-48f9-8c35-3beee28d7890)
- **New Customers** make up around 30% of the total customers. 
- **Churned Customers** and **One Time Big Spenders** sum up to around half the total customers.
- **Loyal Customers** make up 15% of total customers and is the segment with the fewest customers.

# K-Means Clustering Validation
- The clusters are not significantly imbalanced and cluster sizes are acceptable.
- The Silhouette Score for the clustering is around 0.49, which is moderate clustering quality and indicates room for improvement.
- The true optimal k may have been a higher value like 8 or 10, but such values were not tested due to high computational requirements and runtime. Common practice is said to be picking a value for $k$ between 2 to 6.

# RFM Segments
In addition to K-Means, customers could also be segmented using RFM metrics. The method is to set rules or thresholds based on RFM scores and assign the customer to a segment. The customers have been segmented according to their RFM scores using common labels in marketing as follows:

- **Champions**: These are the best customers, with the most recent and frequent purchases and with the most dollar spendings.

- **Loyal Customers**: Customers who have made some less recent purchases and spend a little less than champions. But these customers have still made very recent and frequent purchases with sizeable spendings.

- **Potential Loyalist**: Customers who made recent purchases, but spent moderate amounts with moderate frequency.

- **New Customers**: Customers who made the most recent purchases, but have minimal frequency and spending.

- **Promising**: Customers who made very recent purchases but have low frequency and spending.

- **About To Sleep**: Customers who have moderate spending but haven't made recent or frequent purchases.

- **Hibernating**: Customers who haven't made a purchase in a while and have spent infrequently and little. They have probably churned.

- **Can't Lose Them**: Customers who haven't made recent purchases but have previously made frequent purchases with high spendings.

- **At Risk**: Customers who haven't made recent purchases but have lower spendings and frequent purchases than those under Can't Lose Them.

- **Needs Attention**: Customers who have moderate frequency and spending, but it's been a while since they made a new purchase.

- **Lost**: Customers who have churned.

![rfm_segment_count](https://github.com/user-attachments/assets/1d9bd0ca-d0c6-470a-94cc-d73edf45cbea)

- Amazon had few **New Customers** and **Promising Customers**, so they weren't shown on the plot. This should not come as a surprise since Amazon is an established giant in E-commerce. 
- There are a large number of **Loyal Customers** and **Champions**, but there are even more customers who may be starting to churn.

# Executive Summary
## Insights
- Some customer segments appear to be incorrectly placed within clusters (e.g., **Champions** grouped with **One-Time Big Spenders**, or **Lost customers** under **New Customers**), suggesting imperfect alignment between RFM segmentation and K-Means clustering.
- Considering the K-Means Clustering has a poor Silhouette Score, it may be making poor predictions and causing the irregularities. These inconsistencies may indicate that the chosen number of clusters (k=4) is insufficient or that the K-Means algorithm is not capturing the underlying structure of the data.
- Most of Amazon's customers fall under churn-related segments (e.g., **About to Sleep**, **Hibernating**, **Lost**), highlighting opportunities for targeted re-engagement and retention strategies.

## Recommendations
- Those in the **New Customers** cluster or in the **About to Sleep**, **Needs Attention**, **At Risk**, and **Can't Lose Them** segments should also be targeted with an offer of 1 month free/trial Amazon Prime if they don't have a subscription already and discounted items. This could be done by showing discounted products in their product recommendations (on the front page) or sending an email or message offering a month of free Amazon Prime.
- Marketing campaigns should be launched to find **New Customers** and to reach out to **Churned Customers** in an attempt to rekindle interest.
- Consider implementing a loyalty program providing points based on per dollar spending that can be exchanged for rewards could be implemented. This would reward **Loyal Customers**, potentially retain **New Customers**, and incentivize more customer spending.
- Consider implementing an enhanced cash back program for Loyal Customers. Currently, Amazon offers cashback only to customers who make purchases using an Amazon Chase Visa. Amazon should consider rolling out a cashback program to users without the Amazon credit card at lower rates such as 1% for all customers or 2% for customers with Prime. To prevent abuse, the cashback could be given to customers after the return periods for the item closes. Amazon could also try setting different cashback rates according to spending levels such as Bronze, Silver, Gold, and Diamond tiers.

## Next Steps
- Test higher numbers of clusters (higher values for $k$) to evaluate whether deeper segmentation yields more interpretable or actionable groupings.
- Develop a predictive churn model using supervised learning to identify at-risk customers based on historical behavior.
- Perform sentiment analysis on review text and map sentiment trends to customer segments for richer behavioral insight.

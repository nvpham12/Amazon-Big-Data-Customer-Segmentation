# Project Overview
This a project on big data customer segmentation using Amazon electronic product data and leveraging tools such as PySpark, MLlib, K-Means Clustering, and RFM Analysis. Refer to the Jupyter Notebook file for more technical details.

# Data
The dataset contains information Amazon product user reviews and item metadata between May 1996 to September 2023. The data was scraped from Amazon by McAuley Lab at UC San Diego. While there are other product categories available, this project will use electronics product data.

# ETL Process
The data was first extracted from the source (https://huggingface.co/datasets/McAuley-Lab/Amazon-Reviews-2023) using Hugging Face's datasets library, then converted into parquet storage format. McAuley Lab scraped the data from Amazon. The transformation to parquet was required as the data was too large for Pandas, direct PySpark dataframe creation, or arrow tables. The parquet files were then read into PySpark.

# Data Cleaning
- Converted missing value strings such as 'none' and 'n/a' into nulls
- Dropped duplicates
- Filtered data to last 2 years of observations and only reviews with verified purchases
- Removed incorrect observations and columns
- Converted data types to proper format such as timestamp to seconds and price to double (a type of integer in PySpark)

# Feature Engineering
Recency, Frequency, and Monetary (RFM) metrics will be used as the features for the model. Since we do not have customer purchase dates in the data, the traditional RFM metrics can't be obtained. However, a work around is to use review date as a proxy for purchase time. Because the data includes an indicator for verified purchases, reviewers can be screened to find verified customers. The RFM features in this project will be represented as follows:
- Recency will measure how recent the review left by the customer is as a proxy for how recent a customer made a purchase
- Frequency will measure how often a customer leaves reviews as a proxy for how often a customer made purchases
- Monetary will measure how much a customer spends based on prices of the product they reviewed

Analysis is subject to the following limitations:
- Reviews aren't necessarily left at time of purchase, but often left some time after the purchase
- Not everyone leaves a review (volunteer bias)
- Frequency does not account for purchases of multiple quantities
- The monetary feature is based on the price of the product reviewed, which may not perfectly reflect the price paid for the product. Products can change in price over time (or receive discounts), which isn't accounted for when scraping the data.

# Data Preprocessing
- The data is filtered further to a 1 year range between 9/13/2022 to 9/13/2023 based on the date of the most recent review
- Applied a log transformation on skewed features
- Scaled the data using Standard Scaler

# Modeling
The model will use K-Means Clustering, an unsupervised machine learning algorithm. K-Means initializes a selected k number of points called means or centroids. It then finds the nearest distances between each data point the nearest centroid (using Euclidean distance) updating the positions of centroids. The process is repeated until we have unchanging centroids or cluster assignments, or until we have reached the maximum number of iterations. Before fitting the model, K needs to be chosen. 2 commonly used techniques for this are the Elbow Method and Silhouette Method.

## Elbow Method
![wcss_elbow](https://github.com/user-attachments/assets/c30687ac-34f8-4e9c-8390-744c6dd61dec)
The Elbow Method involves plotting within-cluster sum of squares (WCSS) against k and looking for the elbow, the point where the curve forms a kink or angle at some k.
The angles appear in the curve at values of 3 and 4 for k. Since the curve appears linear after 4, k=4 is the better option.

## Silhouette Method
![silhoutte_score](https://github.com/user-attachments/assets/82dfa82a-c228-41d0-ab94-b378bad8cd7a)
The value of k with the highest Silhouette Score is 2. However, the Silhouette Scores for k=4 and k=6 are not too far off from the score for k=2. 

## Choice of k Clusters
Combining the results, we would choose k=4, since it is the best choice from the Elbow Method and the Silhouette Score is not too far off from the highest score at k=2.

# K-Means Cluster Segments
Clusters were labeled based on analysis of the average values for each RFM metric in each cluster. The 4 clusters are Churned, New Customers, Loyal Customers, and One Time Big Spenders. 

![segment_pie](https://github.com/user-attachments/assets/695491ee-89f4-48f9-8c35-3beee28d7890)
New customers make up around 30% of the total customers. The churned and one time big spenders combine to around half the total customers.

# Recommendations for K-Means Cluster Segments
- New customers should also be targeted with a month of free Amazon Prime if they don't have a subscription already.  
- Generally, marketing campaigns are launched to find New Customers and to try and reach out to Churned Customers to try and rekindle interest and get them to return.
- Amazon could try launching an enhanced cash back program for Loyal Customers. Currently, Amazon offers cashback only to customers who make purchases using an Amazon Chase Visa. Amazon could try rolling out a cashback program to users without the Amazon credit card at lower rates such as 1% for all customers or 2% for customers with Prime. To prevent abuse, the cashback could be given to customers after the return periods for the item closes. Amazon could also try setting different cashback rates according to spending levels. One reference for this is Hilton's Honors Program which sets account levels of Member, Silver, Gold, and Diamond based on number of nights or stays.
- Amazon could also try implementing a loyalty program providing points based on per dollar spending that can be exchanged for rewards. This would make Loyal Customers happy and could be attractive to New Customers.

# RFM Segments
In addition to K-Means, customers could also be segmented using RFM metrics. The method is to set rules or thresholds based on RFM scores and assign the customer to a segment. The following are some common marketing labels that will be used in this project:

Champions
- These are the best customers, with the most recent and frequent purchases and with the most dollar spendings.

Loyal Customers
- Customers who have made some less recent purchases and spend a little less than champions. But these customers have still made very recent and frequent purchases with sizeable spendings.

Potential Loyalist
- Customers who made recent purchases, but spent moderate amounts with moderate frequncy.

New customers
- Customers who made the most recent purchases, but have minimal frequency and spending.

Promising
- Customers who made very recent purchases but have spent have low frequency and spending.

Needs Attention
- Customers who have moderate frequency and spending, but it's been a while since they made a new purchase.

About To Sleep
- Customers who have moderate spending but haven't made recent or frequenct purchases.

Can't Lose Them
- Customers have haven't made recent purchases but have previously made frequent purchases with high spendings.

Hibernating
- Customers who haven't made a purchase in while and have spent infrequently and little. They have probably churned.

Lost
- Customers who have churned.
- 
![rfm_segment_count](https://github.com/user-attachments/assets/1d9bd0ca-d0c6-470a-94cc-d73edf45cbea)

This is a plot of the number of members in each segment. Amazon had too few new or promising customers, so they weren't shown on the plot. This is probably due to Amazon being an established giant in E-commerce. However, even though Amazon has a large number of Loyal Customers and Champions, there are larger number of customers who may be starting to churn.

# Recommendations for RFM Segments
- Customers who are in the About to Sleep, Needs Attention, At Risk, and Can't Lose Them segments should be targeted with discounts and promotions. These could be showing discounted products in their product recommendations (on the front page) or Amazon could offer these customers a month of free Amazon Prime through a message or email.
- Promising customers should also be targeted with a month of free Amazon Prime if they don't have a subscription already.  
- Generally, marketing campaigns are launched to find New Customers and to try and reach out to Hibernating and Lost customers to try and rekindle interest and get them to return.
- For Champions, Loyal Customers, and Potential Loyalists Amazon could benefit from enhanced cash back programs. 
- Amazon could also try implementing a loyalty program providing points based on per dollar spending that can be exchanged for rewards. This would make Champions and Loyal Customers happy and could be attractive to New Customers or Potential Loyalists.

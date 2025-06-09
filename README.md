# Project Overview
This a customer segmentation project on Amazon electronic product reviews and metadata leveraging K-Means Clustering and Recency, Frequency, and Monetary (RFM) Analysis.

# ETL Process
The data was first extracted from the source (https://huggingface.co/datasets/McAuley-Lab/Amazon-Reviews-2023), then converted in parquet storage format. McAuley Lab scraped the data from Amazon. The transformation was a necessity as the data was too large for Pandas, direct PySpark dataframe creation, or arrow tables. The parquet files were then read into PySpark.

# Data Cleaning
- Converted missing value strings such as 'none' and 'n/a' into nulls
- Dropped duplicates
- Filtered year to most recent year and only reviews with verified purchases
- Removed incorrect observations and columns
- Converted data types such as timestamp in milliseconds to seconds and price to double type (a type of integer in PySpark)

# Feature Engineering
Recency, Frequency, and Monetary (RFM) analysis is a method commonly used in marketing and analytics to segment or analyze customers. Traditionally, the features represent the following:
- Recency measures how recent a customer made a purchase
- Frequency measures how often a customer makes purchases
- Monetary measures how much a customer spends

We will be using RFM as the features for the model. Since we do not have customer purchase dates in the data, we will work around this by using review date as a proxy for purchase time. Because the data includes an indicator for verified purchases, we can screen whether reviewers are customers in the data. The RFM features will represent the following in this project:
- Recency will measure how recent the review left by the customer is
- Frequency will measure how often a customer leaves reviews
- Monetary will measure how much a customer spends based on prices of the product they reviewed

The analysis is subject to the following limitations:
- Reviews aren't necessarily left at time of purchase, but often left some time after the purchase
- Not everyone leaves a review (volunteer bias)
- Frequency does not take into account purchases of multiple quantities
- The monetary feature is based on the price of the product reviewed, which may not perfectly reflect the price paid for the product. Products can change in price over time (or receive discounts), which isn't accounted for in this project. The scrape will only take into account the price of the product when it was scraped from Amazon.

# Data Preprocessing
- Applied a log transformation on skewed features
- Scaled the data with Standard Scaler

# Modeling
The model will use K-Means Clustering, an unsupervised machine learning algorithm. K-Means initializes a selected k number of points called means or centroids. It then finds the nearest distances between each data point the nearest centroid (using Euclidean distance) updating the positions of centroids. The process is repeated until we have unchanging centroids or cluster assignments, or until we have reached the maximum number of iterations.

Before fitting the model, K needs to be chosen. 2 commonly used techniques for this are the Elbow Method and Silhouette Method.

## Elbow Method
![wcss_elbow](https://github.com/user-attachments/assets/c30687ac-34f8-4e9c-8390-744c6dd61dec)
The kinks or angles appear in the curve at values of 3 and 4 for K. Since the curve appears linear after 4, we k=4 is the better option.

## Silhouette Method
![silhoutte_score](https://github.com/user-attachments/assets/b3c89fe7-0f4f-48ab-9f02-fa99a4e7ecf6)
The value of K with the highest Silhouette Score is 2. However, the Silhouette Scores at k=4 and k=6 are not too far off from the score for k=2. 

## Choice of K
Combining the results, we would choose k=4, since it is the best choice from the Elbow Method and the Silhouette Score is not too far off from the highest score at k=2.

# Segments

![segment_pie](https://github.com/user-attachments/assets/695491ee-89f4-48f9-8c35-3beee28d7890)

# Recommendations

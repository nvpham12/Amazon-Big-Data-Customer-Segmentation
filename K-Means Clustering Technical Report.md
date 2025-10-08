# Project Background
This project demonstrates distributed computing, querying, and customer segmentation on big data. K-Means Clustering and RFM (Recency, Frequency, Monetary) Analysis are used to group customers based on spending habits.
This report focuses on the K-Means Clustering Model used for segmentation.

## Links
### EDA Jupyter Notebook
- [EDA Notebook](https://github.com/nvpham12/Amazon-Big-Data-Customer-Segmentation/blob/main/EDA%20Amazon%20Electronics%20Big%20Data.ipynb)

### K-Means Clustering
- [K-Means Clustering Notebook](https://github.com/nvpham12/Amazon-Big-Data-Customer-Segmentation/blob/main/K-Means%20Clustering%20Notebook.ipynb)

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
- The dataset contains information Amazon product user reviews and item metadata between May 1996 to September 2023.
- The product reviews dataset has 43,886,944 rows and 10 columns.
- The product metadata dataset has 1,610,012 rows and 16 columns.

## Limitations
This project and its analysis is subject to the following limitations:
- Reviews aren't necessarily left at time of purchase, but often left some time after the purchase.
- Not everyone leaves a review (volunteer bias).
- Frequency does not take into account purchases of multiple quantities.
- The Monetary feature is based on the price of the product reviewed, which may not perfectly reflect the price paid for the product. Products can change in price over time (or receive discounts), which isn't accounted for in this project. The scrape will only take into account the price of the product when it was scraped from Amazon.
- While this project uses verified customers, this pool of customers does not include all customers as not every customer leaves a review. However, analysis can still provide insights into the behavior of engaged and verified buyers, a valuable subset of the overall customer base.

---

# Exploratory Data Analysis (EDA)
Before modeling, exploratory SQL queries and visualizations were conducted to better understand product and review dynamics. Misplaced product categories and invalid rating values were discovered and cleaned.

![2023_popular_products](https://github.com/user-attachments/assets/2c738685-86dd-4cf9-9e15-a9a735bede04)
> Product popularity is approximated using the number of user reviews as a proxy.
- Small electronic devices were the most popular in 2023. Wireless audio devices (earbuds/headphones) were by far the most popular.
- Other popular electronics in 2023 were flash drives and various home improvement products such as cameras and Echo Dots.

---
![rating_distribution](https://github.com/user-attachments/assets/a686da60-1d6a-4fb6-999b-7f49dfa784f8)
- Most product reviews come from users with verified purchases. This was unexpected given that there is no cost requirement (and therefore, a barrier) for users to make reviews without a verified purchases. 
- Users have a tendency to leave ratings at the extreme values (1 and 5).
- Most reviews are 5 star reviews, indicating that users tend to leave a review when they are satisfied with a product.

---

# Data Cleaning
- Converted missing value strings such as 'none' and 'n/a' into nulls.
- Dropped duplicates.
- Filtered data to last 2 years of observations (2022 and 2023) and only reviews with verified purchases.
- Removed incorrect observations and columns.
- Converted data types to proper format such as timestamp to seconds and price to double (a type of integer in PySpark).

---

## Feature Engineering
Recency, Frequency, and Monetary (RFM) metrics will be used as the features for the model. Since the data does not contain customer purchase dates, the traditional RFM metrics cannot be obtained. However, a work around is to use review date as a proxy for purchase time. Because the data includes an indicator for verified purchases, reviewers can be screened to find verified customers. The RFM features in this project will be represented as follows:
- Recency will measure how recent the review left by the customer is as a proxy for how recent a customer made a purchase.
- Frequency will measure how often a customer leaves reviews as a proxy for how often a customer made purchases.
- Monetary will measure how much a customer spends based on prices of the product they reviewed.

Analysis is subject to the following limitations:
- Reviews aren't necessarily left at time of purchase, but often left some time after the purchase.
- Not everyone leaves a review (volunteer bias).
- Frequency does not account for purchases of multiple quantities.
- The monetary feature is based on the price of the product reviewed, which may not perfectly reflect the price paid for the product. Products can change in price over time (or receive discounts), which isn't accounted for when scraping the data.

---

# Modeling
 K-Means Clustering, an unsupervised machine learning algorithm. K-Means initializes a selected k number of points called means or centroids. It then finds the nearest distances between each data point the nearest centroid (using Euclidean distance) updating the positions of centroids. The process is repeated until we have unchanging centroids or cluster assignments, or until we have reached the maximum number of iterations. Before fitting the model, K needs to be chosen. 2 commonly used techniques for this are the Elbow Method and Silhouette Method.

## Data Preprocessing
- The data is filtered further to a 1 year range between 9/13/2022 to 9/13/2023 based on the date of the most recent review in the data.
- Applied a log transformation on skewed features.
- Scaled the data using Standard Scaler.

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

## Clusters and Segments
Clusters were labeled based on analysis of the average values for each RFM metric in each cluster. The 4 clusters are Churned, New Customers, Loyal Customers, and One Time Big Spenders. 

![segment_pie](https://github.com/user-attachments/assets/695491ee-89f4-48f9-8c35-3beee28d7890)
- **New Customers** make up around 30% of the total customers. 
- **Churned Customers** and **One Time Big Spenders** sum up to around half the total customers.
- **Loyal Customers** make up 15% of total customers and is the segment with the fewest customers.

## Validation
- The clusters are not significantly imbalanced and cluster sizes are acceptable.
- The Silhouette Score for the clustering is around 0.49, which is moderate clustering quality and indicates room for improvement.
- The true optimal k may have been a higher value like 8 or 10, but such values were not tested due to high computational requirements and runtime. Common practice is to select a value for $k$ between 2 to 6.

### Treemap
<img width="1288" height="450" alt="cluster   segment treemap" src="https://github.com/user-attachments/assets/a7ae32c0-c49f-47d1-bf6d-2398034e4617" />

- RFM segments (inner boxes) are mapped to clusters (outer boxes)
- The RFM segments are placed into outer boxes that represent the K-Means Clusters. 
- There are some odd placements into the K-Means Clusters. It could be that more clusters would be better such as 8 or 10. 
- Those weren't tested for when selecting the clusters since clustering using RFM metrics usually uses 2 to 6 clusters.

---

# Executive Summary
## Insights
- A deep dive after RFM segmentation and mapping segments to clusters shows that some customer segments appear to be incorrectly placed within clusters (e.g., **Champions** grouped with **One-Time Big Spenders**, or **Lost customers** under **New Customers**), suggesting imperfect alignment between RFM segmentation and K-Means clustering.
- Considering the K-Means Clustering Model has moderate values for its performance metrics, it may be making poor predictions and causing these irregularities. This may indicate that the chosen number of clusters ($k=4$) is insufficient or that the K-Means algorithm is not suitable for the data.
- Most of Amazon's customers fall under churn-related segments (e.g., **About to Sleep**, **Hibernating**, **Lost**), highlighting opportunities and urgency for targeted re-engagement and retention strategies.

## Recommendations
- Those in the **New Customers** cluster or in the **About to Sleep**, **Needs Attention**, **At Risk**, and **Can't Lose Them** segments should also be targeted with an offer of 1 month free and/or trial Amazon Prime if they don't have a subscription already and discounted items. This could be done by showing discounted products in their product recommendations (on the front page) or sending an email or message offering a month of free Amazon Prime.
- Marketing campaigns should be launched to find **New Customers** and to reach out to **Churned Customers** in an attempt to rekindle interest.
- Consider implementing a loyalty program providing points based on per dollar spending that can be exchanged for rewards could be implemented. This would reward **Loyal Customers**, potentially retain **New Customers**, and encourage higher spending.
- Consider implementing an enhanced cash back program for **Loyal Customers** and **Champions**. Currently, Amazon offers cashback only to customers who make purchases using an Amazon Chase Visa. Amazon should consider rolling out a cashback program to users without the Amazon credit card at lower rates such as 1% for all customers or 2% for customers with Prime. To prevent abuse, the cashback could be given to customers after the return periods for the item closes. Amazon could also try setting different cashback rates according to spending levels such as Bronze, Silver, Gold, and Diamond tiers.

## Next Steps
- Test the K-Means model with more clusters to evaluate whether deeper segmentation yields more interpretable or actionable groupings.
- Explore other algorithms for customer segmentation and compare their performance with the K-Means model.
- Perform sentiment analysis on review text and map sentiment trends to customer segments for richer behavioral insight.

---

# Data Source
Dataset: Amazon Reviews 2023

Authors: McAuley Lab at University of California, San Diego

Source: [Hugging Face Repository](https://huggingface.co/datasets/McAuley-Lab/Amazon-Reviews-2023)

Reference: Hou, Y., Li, J., He, Z., Yan, A., Chen, X., & McAuley, J. (2024). Bridging language and items for retrieval and recommendation. arXiv preprint arXiv:2403.03952. https://arxiv.org/abs/2403.03952

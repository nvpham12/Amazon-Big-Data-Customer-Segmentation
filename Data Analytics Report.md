# Project Background
This project demonstrates distributed computing, querying, and customer segmentation on big data. K-Means Clustering and RFM (Recency, Frequency, Monetary) Analysis are used to group customers based on spending habits. The K-Means model had questionable performance, so segmentation based on RFM Analysis was done. Data analysis for this project are based on these RFM segments instead of the clusters.

## Links
### EDA Jupyter Notebook
- [EDA Notebook](https://github.com/nvpham12/Amazon-Big-Data-Customer-Segmentation/blob/main/EDA%20Amazon%20Electronics%20Big%20Data.ipynb)

### K-Means Clustering
- [K-Means Clustering Notebook](https://github.com/nvpham12/Amazon-Big-Data-Customer-Segmentation/blob/main/K-Means%20Clustering%20Notebook.ipynb)
- [K-Means Clustering Technical Report](https://github.com/nvpham12/Amazon-Big-Data-Customer-Segmentation/blob/main/K-Means%20Clustering%20Technical%20Report.md)

### RFM Customer Segmentation
- [RFM Segmentation Notebook](https://github.com/nvpham12/Amazon-Big-Data-Customer-Segmentation/blob/main/RFM%20Segmentation%20Amazon%20Electronics.ipynb)
- [RFM Segmentation Technical Report](https://github.com/nvpham12/Amazon-Big-Data-Customer-Segmentation/blob/main/RFM%20Segmentation%20Technical%20Report.md)

## RFM Features
Recency, Frequency, and Monetary (RFM) analysis is a method commonly used in marketing and analytics to segment or analyze customers. 
In this project, the RFM features will represent the following:
- **Recency**: how recent the review left by the customer is. 
    
- **Frequency**: how often a customer leaves reviews.

- **Monetary**: how much a customer spends based on prices of the product they reviewed.

## Customer Segmentation
The customers are segmented to the following groups:
- **Champions**: These are the best customers, with the most recent and frequent purchases and with the most dollar spendings.
- **Loyal Customers**: Customers who have made some less recent purchases and spend a little less than champions. But these customers have still made very recent and frequent purchases with sizeable spendings.
- **Potential Loyalists**: Customers who made recent purchases, but spent moderate amounts with moderate frequncy.
- **New customers**: Customers who made the most recent purchases, but have minimal frequency and spending.
- **Promising**: Customers who made very recent purchases but have spent have low frequency and spending.
- **About To Sleep**: Customers who have moderate spending but haven't made recent or frequenct purchases.
- **Hibernating**: Customers who haven't made a purchase in while and have spent infrequently and little. They have probably churned.
- **Can't Lose Them**: Customers who haven't made recent purchases but have previously made frequent purchases with high spendings.
- **At Risk**: Customers who haven't made recent purchases but have lower spendings and frequent purchases than those under Can't Lose Them.
- **Needs Attention**: Customers who have moderate frequency and spending, but it's been a while since they made a new purchase.
- **Lost**: Customers who have churned.

---

# Visualizations
## Popular Products
![2023_popular_products](https://github.com/user-attachments/assets/2c738685-86dd-4cf9-9e15-a9a735bede04)
> Product popularity is approximated using the number of user reviews as a proxy.
- Small electronic devices were the most popular in 2023. Wireless audio devices (earbuds/headphones) were by far the most popular.
- Other popular electronics in 2023 were flash drives and various home improvement products such as cameras and Echo Dots.

---

## Rating Distribution
![rating_distribution](https://github.com/user-attachments/assets/a686da60-1d6a-4fb6-999b-7f49dfa784f8)
- Most product reviews come from users with verified purchases. This was unexpected given that there is no cost requirement (and therefore, a barrier) for users to make reviews without a verified purchases. 
- Users have a tendency to leave ratings at the extreme values (1 and 5).
- Most reviews are 5 star reviews, indicating that users tend to leave a review when they are satisfied with a product.

---

## RFM Segment Counts
<img width="1000" height="600" alt="rfm_segment_count" src="https://github.com/user-attachments/assets/c1e6f055-0cb3-4799-af52-3aae7a9db69c" />


- The segments with the most customers are the **About to Sleep** and **Hibernating** segments, which is bad since these customers are at risk of churn or have churned already. About to Sleep customers haven't churned yet though and may be retained through business strategy.
- The segments with the fewest customers are the **Can't Lose Them** and **At Risk** segments. This is good since a business would want those segments to be least populated.
- There don't appear to be any **New Customers** in the data. This is one of the limitations of using user reviews instead of actual customer purchase information.

---

## RFM Segment Pie
<img width="1000" height="600" alt="rfm_segment_pie" src="https://github.com/user-attachments/assets/5842a65a-881e-4dbd-b9ff-9e3f8817ed5d" />

- 36% of customers are regular or high value customers (**Champions**, **Loyalists**, and **Potential Loyalists**)
- 33% of customers are at risk of churn but haven't churned yet (**About to Sleep**, **At Risk**, **Can't Lose Them**, and **Needs Attention**).
- 30% of customers have already churned (**Lost** and **Hibernating**).

---

## Average Monetary Value by Segment
<img width="1000" height="600" alt="avg_monetary_by_segment" src="https://github.com/user-attachments/assets/075bdf02-53bf-4dbd-9e75-5e205dbc48c2" />

- Based on average Monetary value, customers in the **Can't Lose Them** and **About to Sleep** categories are high value customers that should be targeted for retention.
- **Hibernating**, **Needs Attention**, and **At Risk** customers have the lowest average Monetary value.

---

## Total Monetary Value by Segment
<img width="1000" height="600" alt="total_monetary_by_segment" src="https://github.com/user-attachments/assets/6cea748a-5386-459c-acaa-ca8a4c5bab33" />

- Based on total Monetary value, customers in the **Champions** and **About to Sleep** segments contain the highest value customers.
- **Hibernating**, **Needs Attention**, and **At Risk** customers have the lowest total Monetary value.
- **Can't Lose Them** customers are notable since they have the highest average monetary value but lower total monetary value. This could mean that these customers have few members in the segment, but have high spending per member. This segment has very high value and measures should be taken to retain them.

---

# Executive Summary
## Insights
- **Can't Lose Them** customers should be prioritized since they have on average, high levels of spending per capita.
- **About to Sleep** customers have good spending per capita and are the second most important group among customers at risk of churning.
- **Needs Attention** and **At Risk** customers are customers at risk of churning. However, they have low levels of spending.
- 36% of customers are regular or high value customers (**Champions**, **Loyalists**, and **Potential Loyalists**)
- 33% of customers are at risk of churn but haven't churned yet (**About to Sleep**, **At Risk**, **Can't Lose Them**, and **Needs Attention**).
- 30% of customers have already churned (**Lost** and **Hibernating**).
- Small electronic devices were the most popular in 2023. Wireless audio devices (earbuds/headphones) were by far the most popular.
- Ratings tend to extremes, especially towards 5 star reviews.

## Recommendations
- Customers in the **About to Sleep** and **Can't Lose Them** segments should be targeted with discounts and promotions. This could be done by showing discounted products in their product recommendations (on the front page) or offering these customers a 1-month trial of Amazon Prime.
- Customers categorized as **Can't Lose Them** should be focused on, since they are at risk of churning and have high levels of spending.
- Customers in the **Needs Attention** and **At Risk** segments require more careful consideration. They are at risk of churning, but have low average and total spending. The benefits of retaining them may not be worth the costs of doing so.
- Marketing campaigns should be launched to find **New Customers** and reach **Hibernating** and **Lost** customers in an attempt to rekindle interest and get them to return.
- Consider rewarding **Champions**, **Loyal Customers**, and **Potential Loyalists** for being good customers. Consider offering exclusive perks, discounts, or coupons to these customers. Or create some loyalty program based on spending, which may encourage more purchases from them.
- Consider developing Amazon branded audio devices like earbuds and speakers. Continue development of Fire and Echo products.
- Investigate 5 star reviews for any fake reviews.
---

# Data Source
Dataset: Amazon Reviews 2023

Authors: McAuley Lab at University of California, San Diego

Source: [Hugging Face Repository](https://huggingface.co/datasets/McAuley-Lab/Amazon-Reviews-2023)

Reference: Hou, Y., Li, J., He, Z., Yan, A., Chen, X., & McAuley, J. (2024). Bridging language and items for retrieval and recommendation. arXiv preprint arXiv:2403.03952. https://arxiv.org/abs/2403.03952

# Online Shopping Habits 

In today’s digital landscape, online spending plays a significant role in shaping consumer behaviour and business decisions. By understanding how different demographic groups spend online, companies can better tailor their products and services to meet customer needs. This paper analyses online spending habits using a public dataset from Kaggle, focusing on identifying patterns among different age and salary groups through the use of K-means clustering as described in this online article.

# Data and Methods 

Shopping data was source online from Kaggle 
K-Means Cluster 
K-means clustering is a widely used method for segmenting data into distinct groups. The algorithm works as follows:
1.	Initialisation: Randomly select K initial centroids. 
2.	Assignment: Assign each data point to the nearest centroid, forming K clusters. 
3.	Update: Calculate new centroids by averaging the data points in each cluster. 
4.	Iteration: Repeat the assignment and update steps until the centroids stabilise

# Conclusion

Based on the insight produced using the data there’s some clear conclusions that can be drawn.
-	Generation X age group has the highest number of purchases but actually has the lowest average spend.
-	Middle and Upper middle income individuals have the highest number of purchases with the upper middle income having a significantly higher average spend than the other salary categories. 
-	The silent generation contributes the lowest online purchases but have the highest average spend when they purchase.
-The optimal number of clusters to use in the model was found to be four using the elbow method. 
-	The silhouette score was 0.537 indicating the clusters are well separated and cohesive. (Ankita, 2025)

# Data Engineering

The dataset was sourced from Kaggle and downloaded to CSV form. Utilising the Microsoft gen 2 data flows I was able in ingest the data into a central lakehouse ready to connect to a python notebook for EDA and analysis. The benefits of using a gen2 data flow is that I can load, transform, and specify the destination of the query output, all within one tool.

## 1. Upload into the dataflow
The file was downloaded and then uploaded to the gen2 dataflow as a CSV

![Engineering 1](https://github.com/clowesr/Portfolio/blob/f1c4dfb00c23a7af734bf5aaa0752a0d8dc1fa23/Projects/Shopping%20Habits/Details/Data%20Engineering%201.png)

## 2. Format the data
The headers are promoted and the data types selected then the columns not needed are removed

![Engineering 2](https://github.com/clowesr/Portfolio/blob/3fe8c7cb8250eed481c2d786f699f49dede2cbf4/Projects/Shopping%20Habits/Details/Data%20Engineering%202.png)

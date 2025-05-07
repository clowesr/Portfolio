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


Checking the columns for year of birth and salary, i was able to find null values and remove from the data

![Engineering 3](https://github.com/clowesr/Portfolio/blob/fe5218aa500dcba424d090f7ac085a2279fd601f/Projects/Shopping%20Habits/Details/Data%20Engineering%203.png)


Creating a new column allows for combined amount of online spending

![Engineering 4](https://github.com/clowesr/Portfolio/blob/fe5218aa500dcba424d090f7ac085a2279fd601f/Projects/Shopping%20Habits/Details/Data%20Engineering%204.png)


Removing the individual spend columns keeps the data tidy 

![Engineering 5](https://github.com/clowesr/Portfolio/blob/fe5218aa500dcba424d090f7ac085a2279fd601f/Projects/Shopping%20Habits/Details/Data%20Engineering%205.png)


Due to needing the age and not just the birth year, an additional column was created to calculate the difference between the year of birth and current year.

![Engineering 6](https://github.com/clowesr/Portfolio/blob/fe5218aa500dcba424d090f7ac085a2279fd601f/Projects/Shopping%20Habits/Details/Data%20Engineering%206.png)


Creating the new age column highlighted an anomaly with ages showing above 124 which were removed from the data

![Engineering 7](https://github.com/clowesr/Portfolio/blob/fe5218aa500dcba424d090f7ac085a2279fd601f/Projects/Shopping%20Habits/Details/Data%20Engineering%207.png)


New columns created for age segments and salary bandings to aid insight

![Engineering 8](https://github.com/clowesr/Portfolio/blob/fe5218aa500dcba424d090f7ac085a2279fd601f/Projects/Shopping%20Habits/Details/Data%20Engineering%208.png)
![Engineering 9](https://github.com/clowesr/Portfolio/blob/fe5218aa500dcba424d090f7ac085a2279fd601f/Projects/Shopping%20Habits/Details/Data%20Engineering%209.png)


Using Gen2 data flows allows the destination and schema to be determined once the query has ran.

![Engineering 10](https://github.com/clowesr/Portfolio/blob/fe5218aa500dcba424d090f7ac085a2279fd601f/Projects/Shopping%20Habits/Details/Data%20Engineering%2010.png)
![Engineering 11](https://github.com/clowesr/Portfolio/blob/fe5218aa500dcba424d090f7ac085a2279fd601f/Projects/Shopping%20Habits/Details/Data%20Engineering%2011.png)


The last element of the data engineering was pulling the new table into a python notebook, ready for EDA.

![Engineering 13](https://github.com/clowesr/Portfolio/blob/fe5218aa500dcba424d090f7ac085a2279fd601f/Projects/Shopping%20Habits/Details/Data%20Engineering%2013.png)

# EDA- Exploratory Data Analysis

An additional check was added across all columns to ensure no nulls

![EDA1](https://github.com/clowesr/Portfolio/blob/fe5218aa500dcba424d090f7ac085a2279fd601f/Projects/Shopping%20Habits/Details/EDA1.png)


Using the newly formed age segment column, the pie chart shows generation x customers make up 50% of all the purchases with the silent generation only making up 1.04%.

![EDA2](https://github.com/clowesr/Portfolio/blob/fe5218aa500dcba424d090f7ac085a2279fd601f/Projects/Shopping%20Habits/Details/EDA2.png)


This shows the number of purchases by the salary range. Middle income households made 45% of the purchases with Upper Middle-income households making 37%. High income households made the least number of purchases but could be spending more and single transactions.

![EDA3](https://github.com/clowesr/Portfolio/blob/fe5218aa500dcba424d090f7ac085a2279fd601f/Projects/Shopping%20Habits/Details/EDA3.png)


Following on from the pie charts, the bar chart below shows average spending across the four age segments. While Generation X made the highest number of purchases, their average spend is the lowest. On the other end of the scale, the silent generation made the least number of purchases, but their average spend was almost double that of Gen X. 

![EDA4](https://github.com/clowesr/Portfolio/blob/fe5218aa500dcba424d090f7ac085a2279fd601f/Projects/Shopping%20Habits/Details/EDA4.png)


This compares the average spend across the income categories. Upper middle income shown the second highest volume of sales as well as the highest average spend. Low-income salary provided 16% of the volume of sales but their average spend is significantly lower than the other categories.

![EDA5](https://github.com/clowesr/Portfolio/blob/fe5218aa500dcba424d090f7ac085a2279fd601f/Projects/Shopping%20Habits/Details/EDA5.png)


The histogram displayed shows an almost symmetric distribution with a slight left skew. Most of the data points fall within the 48-55 range which matches the pie chart visual showing 47.9% in the gen x categories.

![EDA6](https://github.com/clowesr/Portfolio/blob/fe5218aa500dcba424d090f7ac085a2279fd601f/Projects/Shopping%20Habits/Details/EDA6.png)


This a histogram showing the distribution of data points for the salary bandings and its showing a reverse bell curve around the lower incomes with a small tail for high income.

![EDA7](https://github.com/clowesr/Portfolio/blob/fe5218aa500dcba424d090f7ac085a2279fd601f/Projects/Shopping%20Habits/Details/EDA7.png)


To investigate the tail shown in the histogram, a scatter chart was produced which clearly shows an outlier of a salary above 600k and some around the 150k mark so these have been removed due to the k mean algorithm being sensitive to outliers. 

![EDA8](https://github.com/clowesr/Portfolio/blob/fe5218aa500dcba424d090f7ac085a2279fd601f/Projects/Shopping%20Habits/Details/EDA8.png)


Once the main outliers were removed it shows a clear linear correlation between salary and total spend suggesting the more you earn, the more you would spend online. All the outliers haven’t been removed to avoid overfitting of the data

![EDA9](https://github.com/clowesr/Portfolio/blob/fe5218aa500dcba424d090f7ac085a2279fd601f/Projects/Shopping%20Habits/Details/EDA9.png)


Now the data is clean and ready for the k means algorithm, the k or amount of clusters required needs identifying. This can be achieved by using the elbow method. When the below starts to level out is the figure that we’re interested in and that happens on the “4” point therefore the algorithm will be coded with four clusters

![EDA10](https://github.com/clowesr/Portfolio/blob/fe5218aa500dcba424d090f7ac085a2279fd601f/Projects/Shopping%20Habits/Details/EDA10.png)


# K Means Analysis

Once the data is cleaned and the number of clusters has been identified, the algorithm can be ran using the python code shown below. You can see the clusters set to 4 within the cell. The k-means++ package completes the full process without the need to run the algorithm multiple times. The manual process would involve running the algorithm, assigning the centroids and then rerunning until the centroids no longer significantly move. This method will loop through that process with a maximum iteration of 300, and conclude when the centroids no longer significantly move.

![Kmean1](https://github.com/clowesr/Portfolio/blob/fe5218aa500dcba424d090f7ac085a2279fd601f/Projects/Shopping%20Habits/Details/Kmean1.png)


Once completed, a check on how many data points have been assigned to each cluster and its fairly consistent across all 4 centroids.

![Kmean2](https://github.com/clowesr/Portfolio/blob/fe5218aa500dcba424d090f7ac085a2279fd601f/Projects/Shopping%20Habits/Details/Kmean2.png)


Finally the visual displays each data point, coloured by the assigned cluster with the centroids marked in the using an “X”. As you can see, there’s still a number of data points that would be considered outliers but you risk over fitting so remained.

![Kmean3](https://github.com/clowesr/Portfolio/blob/fe5218aa500dcba424d090f7ac085a2279fd601f/Projects/Shopping%20Habits/Details/Kmean3.png)


# Conclusion

The process of analysing the data found some interesting insights which can be used to market certain demographics in a specific way.
-	Generation X age group has the highest number of purchases but actually has the lowest average spend.
- Middle and Upper middle income individuals have the highest number of purchases with the upper middle income having a significantly higher average spend than the other salary categories. 
-	The silent generation contributes the lowest online purchases but have the highest average spend when they purchase.
-	The optimal number of clusters to use in the model was found to be four using the elbow method. 
-	The silhouette score was 0.537 indicating the clusters are well separated and cohesive. (Ankita, 2025)

While the clustering was based on age and salary bandings, I believe there could be some interesting insight into the types of products different demographics purchase and the data separates into purchases such as wine or meat products which could be used to specifically target people. The analysis could also support how certain demographics are targeted such as the silent generation being introduced to online shopping. It would be useful to try and interrogate the in store shopping vs online shopping to see if there’s any trends with how people shop based on their age and salary banding.

# Fabric Notebook Cells Found here

Click Here

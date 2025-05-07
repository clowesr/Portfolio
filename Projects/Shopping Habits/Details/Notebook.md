# Import
```
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
from sklearn.preprocessing import LabelEncoder, StandardScaler
from sklearn.cluster import KMeans
import plotly.express as px
sns.set(style="whitegrid")

#Set the path to import the shopping data
openpath="abfss://3d544e48-9167-40c7-ac36-a46cc6926c8c@onelake.dfs.fabric.microsoft.com/f24594d3-2716-4032-b93f-e76386162e8b/Tables/"
openpath= openpath+("Online_spend")

#open the data table
df_spend = spark.read.format("delta").load(openpath)
display(df_spend)

#convert the dataframe from spark to pandas
df_spend = df_spend.toPandas()
```
# Check Missing Values
``` #I need to check of any of the columns have blanks once loaded into notebooks
df_nulls = df_spend.isna().sum().to_dict()
df_nulls = pd.DataFrame(list(df_nulls.items()), columns=['Column', 'Missing_Values'])

fig = px.bar(df_nulls,
       x = 'Column',
       y = 'Missing_Values',
       template = 'plotly_white',
       title = 'Missing Values')
fig.show()
```
# Insight into shopping numnbers by age group
```#I want to see some insight into the age segments and how many data points sit in each

age_segment = df_spend.Age_Segments.value_counts()

fig = px.pie(age_segment, 
             values = age_segment.values, 
             names = age_segment.index,
             color_discrete_sequence=px.colors.sequential.Reds)
fig.update_traces(textposition='outside', textinfo='percent+label', 
                  marker = dict(line = dict(color = 'black', width = 2)))
fig.show()
```
# Insight into shopping numnbers by salary group
```#based on the salary ranges, i want to see how many data points sit in each
salary_segment = df_spend.Salary.value_counts()

fig = px.pie(salary_segment, 
             values = salary_segment.values, 
             names = salary_segment.index,
             color_discrete_sequence=px.colors.sequential.deep)
fig.update_traces(textposition='outside', textinfo='percent+label', 
                  marker = dict(line = dict(color = 'black', width = 2)))
fig.show()
```

# Insight into average spend by age group
```#additional insight added on the average spend for each age segment
agespend = df_spend.groupby('Age_Segments')['Total_spend'].mean().sort_values(ascending=False)
agespend_df = pd.DataFrame(list(agespend.items()), columns=['Age_Segments', 'Total_spend'])

plt.figure(figsize=(20,10))

sns.barplot(data=agespend_df,  x="Age_Segments", y="Total_spend");
plt.xticks( fontsize=12)
plt.yticks( fontsize=12)
plt.xlabel('Age', fontsize=12, labelpad=20)
plt.ylabel('Average Spend', fontsize=12, labelpad=20);
```

# Insight into average spend by salary group
```#added the same details on the average spend by salary group
salspend = df_spend.groupby('Salary')['Total_spend'].mean().sort_values(ascending=False)
salspend_df = pd.DataFrame(list(salspend.items()), columns=['Salary', 'Total_spend'])

plt.figure(figsize=(20,10))

sns.barplot(data=salspend_df,  x="Salary", y="Total_spend");
plt.xticks( fontsize=12)
plt.yticks( fontsize=12)
plt.xlabel('Salary', fontsize=12, labelpad=20)
plt.ylabel('Average Spend', fontsize=12, labelpad=20);
```

# Histogram by age
```#plotted the ages on a histogram to see the data destribution 
plt.figure(figsize=(20,10))
ax = sns.histplot(data = df_spend.Age, color='blue')
ax.set(title = "Age Distribution");
plt.xticks( fontsize=12)
plt.yticks( fontsize=12)
plt.xlabel('Age ', fontsize=12, labelpad=20)
plt.ylabel('Counts', fontsize=12, labelpad=20);
```

# Histogram by salary
```#plotted the salary on a histogram to see the data destribution 
plt.figure(figsize=(20,10))
ax = sns.histplot(data = df_spend.Salary, color='blue')
ax.set(title = "Salary Distribution");
plt.xticks( fontsize=12)
plt.yticks( fontsize=12)
plt.xlabel('Salary ', fontsize=12, labelpad=20)
plt.ylabel('Counts', fontsize=12, labelpad=20);
```

# Scatter chart to check any anomalies
```plt.figure(figsize=(20,10))
sns.scatterplot(x=df_spend.Income, y=df_spend.Total_spend, s=100);

plt.xticks( fontsize=16)
plt.yticks( fontsize=16)
plt.xlabel('Salary', fontsize=20, labelpad=20)
plt.ylabel('Total_spend', fontsize=20, labelpad=20);
```

# Filter dataframe to remove the higher salaries
```# there seems to be a huge outliers missed in the dataflow so i will remove from here
df_spend =df_spend[df_spend.Income<150000]
```

# Replot the scatter chart with the higher salaries removed
```plt.figure(figsize=(20,10))
sns.scatterplot(x=df_spend.Income, y=df_spend.Total_spend, s=100);

plt.xticks( fontsize=16)
plt.yticks( fontsize=16)
plt.xlabel('Salary', fontsize=20, labelpad=20)
plt.ylabel('Total_spend', fontsize=20, labelpad=20);
```

# Create the elbow diagram to find how many clusters to use
```from sklearn.cluster import KMeans
options = range(2,9)
inertias = []
for n_clusters in options:
    model = KMeans(n_clusters, random_state=42).fit(X)
    inertias.append(model.inertia_)
plt.figure(figsize=(20,10))    
plt.title("No. of clusters vs. Inertia")
plt.plot(options, inertias, '-o')
plt.xticks( fontsize=16)
plt.yticks( fontsize=16)
plt.xlabel('No. of Clusters (K)', fontsize=20, labelpad=20)
plt.ylabel('Inertia', fontsize=20, labelpad=20);
```
# Run the k means
```model = KMeans(n_clusters=4, init='k-means++', random_state=42).fit(X)
preds = model.predict(X)
customer_kmeans = X.copy()
customer_kmeans['clusters'] = preds
```

# Check the output of the K means by plotting ghe data points by assigned cluster
```clust = customer_kmeans.clusters.value_counts()
fig = px.pie(clust, 
             values = clust.values, 
             names = clust.index,
             color_discrete_sequence=px.colors.sequential.deep)
fig.update_traces(textposition='outside', textinfo='percent+label', 
                  marker = dict(line = dict(color = 'black', width = 2)))
fig.show()
```

# Plot the data points and assign the colour based on the cluster and add the centroid shown as an X
```import plotly.graph_objects as go
# Get unique clusters
clusters = customer_kmeans['clusters'].unique()
# Create a blank figure
fig = go.Figure()
# Add cluster data points first (one trace per cluster)
for cluster in clusters:
    cluster_data = customer_kmeans[customer_kmeans['clusters'] == cluster]
    fig.add_trace(go.Scatter(
        x=cluster_data['Income'],
        y=cluster_data['Total_spend'],
        mode='markers',
        name=f'Cluster {cluster}',
        marker=dict(size=8),
    ))
# Calculate centroids
centroids = customer_kmeans.groupby('clusters')[['Income', 'Total_spend']].mean()
# Add centroids last so they're on top
for cluster in centroids.index:
    fig.add_trace(go.Scatter(
        x=[centroids.loc[cluster, 'Income']],
        y=[centroids.loc[cluster, 'Total_spend']],
        mode='markers',
        marker=dict(symbol='x', size=14, color='red'),
        name=f'',
       text=[f''],
        textposition='top center'
    ))
# Show the figure
fig.update_layout(title='Clusters with Centroids on Top')
fig.show()
```

# Print the silhouette score to determine how good the points were clustered
```from sklearn.metrics import silhouette_score
# Calculate silhouette score
score = silhouette_score(X, preds)
print(f"Silhouette Score: {score:.3f}")
```

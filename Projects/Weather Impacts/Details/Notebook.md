## Import

```from pyspark.sql.functions import expr, col, date_add, to_timestamp, date_format, when, lit, length
import sempy.fabric as fabric

openpath="abfss://3d544e48-9167-40c7-ac36-a46cc6926c8c@onelake.dfs.fabric.microsoft.com/f24594d3-2716-4032-b93f-e76386162e8b/Tables/"

#as well as identifying the lakehouse raw path, this allows me to control the table name it looks for and future proofs the code

workorderpath= openpath+("WorkOrders")
weatherpath= openpath+("Weather")

#this will take full lakehouse path and load into the notebook for trasnformation

df_WorkOrders = spark.read.format("delta").load(workorderpath)
df_Weather = spark.read.format("delta").load(weatherpath)
```
## Check dataframes

```# Count the number of rows in each DataFrame
rows_work_orders = df_WorkOrders.count()
rows_weather = df_Weather.count()

# Display the results
print(f"Number of rows in df_WorkOrders: {rows_work_orders}")
print(f"Number of rows in df_Weather: {rows_weather}")
```

## Show last 20 records from each table

```# Show the last 20 values of each DataFrame
last_20_work_orders = df_WorkOrders.orderBy(df_WorkOrders.columns[0], ascending=False).limit(20)
last_20_weather = df_Weather.orderBy(df_Weather.columns[0], ascending=False).limit(20)

# Display the results
last_20_work_orders.show(20)
last_20_weather.show(20)
```
## Remove the rows not in the weather data

```# List of dates to be removed
dates_to_remove = ['2025-05-02', '2025-05-01', '2025-04-30']

# Filter out the rows with the specified dates
df_WorkOrders = df_WorkOrders.filter(~df_WorkOrders['created_date'].isin(dates_to_remove))
```
## Show the last 20 again to check

```# Show the last 20 values of each DataFrame
last_20_work_orders = df_WorkOrders.orderBy(df_WorkOrders.columns[0], ascending=False).limit(20)
last_20_weather = df_Weather.orderBy(df_Weather.columns[0], ascending=False).limit(20)

# Display the results
last_20_work_orders.show(20)
last_20_weather.show(20)
```
## Check for nulls across the tables
```from pyspark.sql import functions as F

# Check for nulls and blanks in the DataFrame
null_counts = df_Weather.select([F.count(F.when(F.col(c).isNull() | (F.col(c) == ""), c)).alias(c) for c in df_Weather.columns])

null_counts.show()
null_countsWO = df_WorkOrders.select([F.count(F.when(F.col(c).isNull() | (F.col(c) == ""), c)).alias(c) for c in df_WorkOrders.columns])
null_countsWO.show()
```


## Found values that didn't complete a week so removed them
```# List of dates to be removed
dates_to_remove = ['2025-04-28', '2025-04-29']

# Filter out the rows with the specified dates
df_WorkOrders = df_WorkOrders.filter(~df_WorkOrders['created_date'].isin(dates_to_remove))
df_Weather = df_Weather.filter(~df_Weather['Date'].isin(dates_to_remove))

display(df_WorkOrders)
display(df_Weather)
```

## Combine both DataFrames with the date field and the created date field being the key field
```combined_df = df_WorkOrders.join(df_Weather, df_WorkOrders['created_date'] == df_Weather['Date'], 'inner')

#drop the date field
combined_df=combined_df.drop('Date')\
                    .withColumnRenamed('created_date','Date')\
                    .withColumnRenamed('count',"Jobs")
# Display the results
combined_df.show()
```

## Import the libaries needed 

```import pandas as pd
import numpy as np
import seaborn as sns
import matplotlib.pyplot as plt

from sklearn.model_selection import train_test_split
from sklearn.linear_model import LinearRegression
from sklearn.metrics import mean_squared_error, r2_score
```

## Convert the dataframe to pandas as it works better with the libaries
```combined_df = combined_df.toPandas()```


## Print the data descriptions
```print(combined_df.head())
print(combined_df.info())
print(combined_df.describe())
```
## Drop the second date column from combining
```combined_df = combined_df.drop(columns=['Date'])```


# Weather Impacts 
## Introduction
This report aims to identify whether certain weather conditions affect customer demand within Dyno-Rod. Using a data science approach, the goal is to determine if average rainfall and mean temperatures impact customer demand and could serve as inputs for a future demand prediction project. This document includes the full ETL pipeline, exploratory data analysis, the chosen data science method, and a conclusion on the hypothesis that temperature and/or rainfall significantly affect Dyno-Rod customer demand.

## Background
This report aims to identify whether certain weather conditions affect customer demand within Dyno-Rod. Using a data science approach, the goal is to determine if average rainfall and mean temperatures impact customer demand and could serve as inputs for a future demand prediction project. This document includes the full ETL pipeline, exploratory data analysis, the chosen data science method, and a conclusion on the hypothesis that temperature and/or rainfall significantly affect Dyno-Rod customer demand.

## Data and Methods

### Rainfall (mm)
Average rainfall data was sourced from the Met Office’s publicly available datasets. The data is presented daily, with historical records from the early 1900s. For this analysis, I’ve restricted it to data from 2021 onwards.

### Average Temperature (°C)
Temperature data was also sourced from the Met Office. Like rainfall, it is daily and historically deep, but here too, only data from 2021 onwards is used.

### Job Volumes
Due to a system change in 2020, job volume data before that year is excluded. To align with the weather data, only records from 2021 onwards are included. Job volumes reflect all demand logged in the Dynamics 365 system.

### Multiple Linear Regression
Multiple linear regression is the chosen method to compare datasets, identify correlations, and predict trends (Bevans, 2023). Using notebooks in Microsoft Fabric, I will conduct EDA and run the model. A measure of success is finding a statistically significant correlation between weather and Dyno-Rod job volumes, supported by a t-test assessing the null (no impact) and alternative (impact) hypotheses.

## Conclusion
It was found that there was some variance in data points for the 3 data sets which resulted in exclude 5 dates from the jobs data to ensure consistency. The regression analysis shows that temperature has a statistically significant effect on job volumes, with a coefficient of -29.41 (p < 0.001). This means that for every increase in temperature, the number of jobs decreases. When looking at average rainfall, this has a small, non-significant effect (coefficient = 2.69, p = 0.682), suggesting it doesn’t influence job volumes.
Based on the t-test results, we reject the null hypothesis that weather has no effect on job volumes. However, the model’s R-squared value is 0.032, meaning it explains only about 3.2% of the variation in job volumes. This suggests temperature plays a role, but many other factors are also influencing the number of jobs.

## Recommendations

Based on the linear regression findings, it would be valuable to isolate job volumes by channel and run similar analyses. Industry knowledge suggests certain job types, such as central heating repairs, are highly temperature-dependent.

Since only 3.2% of variance is explained by temperature, further analysis is needed to identify stronger influences—such as marketing spend or business-to-business activity.

## Data Infrastructure
### Organisations, Role, and Data Systems & Tools

Our team oversees the CRM system for customer information as well as Dyno-Rods data and infrastructure




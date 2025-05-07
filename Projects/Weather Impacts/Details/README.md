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

![Infra1](https://github.com/clowesr/Portfolio/blob/4bb15a7b835712563e8aedcd6877d7ef0f90e442/assets/css/Infra1.png)

The data team sits centrally, surrounded by the systems team ensuring CRMs and tools function correctly. We have one-way and two-way interactions across the teams

![Infra2](https://github.com/clowesr/Portfolio/blob/4bb15a7b835712563e8aedcd6877d7ef0f90e442/assets/css/Infra2.png)

The table shows the tools we use as well as the purpose and teams involved 

![Infra3](https://github.com/clowesr/Portfolio/blob/4bb15a7b835712563e8aedcd6877d7ef0f90e442/assets/css/Infra3.png)

Data governance at Centrica is fundamental to our operations, safeguarding us against the threats of cyber-attacks and data breaches. Without robust data governance, we risk significant fines, the compromise of customer information, and a loss of customer trust. Microsoft Fabric which is the chosen data infrastructure ensures everything we do, is secure and well maintained in line with GDPR guidelines.

![Infra4](https://github.com/clowesr/Portfolio/blob/4bb15a7b835712563e8aedcd6877d7ef0f90e442/assets/css/Infra4.png)


## Data Engineering

ETL Pipeline, Processing Data, Technical Overview 
This section outlines the processes of data extraction, transformation, and loading undertaken during this project, beginning with my ETL pipeline diagram. The pipeline shown below utilises Microsoft fabric.

![Engin1](https://github.com/clowesr/Portfolio/blob/4bb15a7b835712563e8aedcd6877d7ef0f90e442/assets/css/Engineering1.png)

This grapic summarises each step of the ETL pipeline process

![Engin2](https://github.com/clowesr/Portfolio/blob/4bb15a7b835712563e8aedcd6877d7ef0f90e442/assets/css/Engineering2.png)


## Technical Elements

### Extraction 

The work orders (jobs) are pulled into the main lake house via a notebook, the weather data is extracted from the met office website using an import CSV function. The entire process is tied together using the pipeline function.

![tech1](https://github.com/clowesr/Portfolio/blob/4bb15a7b835712563e8aedcd6877d7ef0f90e442/assets/css/Tech1.png)
![tech2](https://github.com/clowesr/Portfolio/blob/4bb15a7b835712563e8aedcd6877d7ef0f90e442/assets/css/Tech2.png)
![tech3](https://github.com/clowesr/Portfolio/blob/4bb15a7b835712563e8aedcd6877d7ef0f90e442/assets/css/Tech3.png)

### Transformation

There's a few transformation steps such as creating a count of work orders by date, removing unwanted columns and combining the 3 CSVs on weather 

![tech4](https://github.com/clowesr/Portfolio/blob/4bb15a7b835712563e8aedcd6877d7ef0f90e442/assets/css/Tech4.png)


## Data Visualisation

While there is not any dashboard creation, I have utilised python function to visualise the data used in the linear regression modelling.

![Viz1](https://github.com/clowesr/Portfolio/blob/4bb15a7b835712563e8aedcd6877d7ef0f90e442/assets/css/Viz1.png)
![Viz2](https://github.com/clowesr/Portfolio/blob/4bb15a7b835712563e8aedcd6877d7ef0f90e442/assets/css/Viz2.png)


## EDA (Exploratory Data Analysis)

The EDA process involved further transformation to ensure the data amounts matched as well exploring key metrics such as means and checking there’s no nulls.

First check was to ensure the data frames were the same size

![EDA1](https://github.com/clowesr/Portfolio/blob/4bb15a7b835712563e8aedcd6877d7ef0f90e442/assets/css/ED1.png)

The removed the rows that didn't match to ensure its the same size

![EDA2](https://github.com/clowesr/Portfolio/blob/4bb15a7b835712563e8aedcd6877d7ef0f90e442/assets/css/ED2.png)

Its good practice to show the info and describe of the data

![EDA6](https://github.com/clowesr/Portfolio/blob/4bb15a7b835712563e8aedcd6877d7ef0f90e442/assets/css/ED6.png)

Checking the correlation matrix, it confirms there’s no multiclonality

![EDA7](https://github.com/clowesr/Portfolio/blob/4bb15a7b835712563e8aedcd6877d7ef0f90e442/assets/css/ED7.png)


## Results

The data was split 80:20 with test and train data to avoid overfitting.

The linear regression package used was statsmodels. This was chosen over scikit-learn due to its built statistic outputs which support the need for confirming the null or alternative hypothesis. 

![Res1](https://github.com/clowesr/Portfolio/blob/4bb15a7b835712563e8aedcd6877d7ef0f90e442/assets/css/Result1.png)

The r squared value at 0.032 indicates only 3.2% of the variation in the job volumes can be explained by the weather impacts.

When looking at temp and rainfall impacts, the combination of -6.412 t value and a 0.000 p value (less than the 0.05 set), suggests a significant yet negative impact on job volumes, therefore the null hypothesis of there’s no impact is rejected, and we accept the alternative hypothesis. This indicates the variations isn’t by chance and something else must be impacting.

When looking at the rainfall, the t value of 0.409 showing a weak impact and a p value of 0.682 which is greater than the 0.05 set, leads us to accept the null hypothesis that there’s no impact to job volumes

![Res2](https://github.com/clowesr/Portfolio/blob/4bb15a7b835712563e8aedcd6877d7ef0f90e442/assets/css/Result2.png)


## Conclusion 
In conclusion, based on the multiple linear regression and the results of the t-test, we can determine that the null hypothesis—that temperature has no impact on jobs—is rejected, and the alternative is accepted. However, this does not fully conclude the analysis. While temperature does show some influence, the correlation results indicate that there may be other, more significant factors affecting the variance in job volumes, which will require further investigation. Regarding average rainfall, although the business initially suggested a direct influence on job volumes, the analysis confirms no statistical impact, and we therefore accept the null hypothesis.
This analysis provides a solid foundation for ongoing discussions within the business and supports our goal of identifying meaningful drivers behind customer demand (jobs). I would suggest trying relationships between different job types as well as exploring another regression including t-test on marketing to see if that influences.

## The notebook used for the regression model can be found here

[Click here](https://github.com/clowesr/Portfolio/blob/8bbcb0eb5e3d32edbd663a7cb7bb297865f3fe3b/Projects/Weather%20Impacts/Details/Notebook.md)

## The data used in this analysis can be found below 

[Average Rainfall 1](https://www.metoffice.gov.uk/hadobs/hadukp/data/daily/HadEWP_daily_totals.txt)

[Average Rainfall 2](https://www.metoffice.gov.uk/hadobs/hadukp/data/daily/HadSP_daily_totals.txt)

[Temperature](https://www.metoffice.gov.uk/hadobs/hadcet/data/meantemp_daily_totals.txt)

[Jobs](https://github.com/clowesr/Portfolio/blob/8f560bf41f5a0c1d265b36a29bd2f7f80da04f5b/assets/Jobs.csv)

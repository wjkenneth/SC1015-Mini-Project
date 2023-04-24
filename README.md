# SC1015 Mini-Project

Team 9: Audric & Kenneth

# Table of Contents

- [Welcome Message](#welcome-message)
- [Team 9 Members](#team-9-members)
- [Question/Problem Definition](#questionproblem-definition)
- [Dataset Selection & Preparation](#dataset-selection--preparation)
    - [SingStats](https://www.singstat.gov.sg/find-data/search-by-theme?type=all)
- [Exploratory Data Analysis](#exploratory-data-analysis)
    - [EDA file](https://github.com/wjkenneth/SC1015-Mini-Project/tree/main/EDA)
    - [General Trend](#general-trend)
    - [Causes and Symptoms](#causes-and-symptoms)
    - [Linear Regression](#linear-regression)
    - [Change of Project Path](#change-of-project-path)
    - [Additional EDA](#additional-eda)
- [Machine Learning](#machine-learning)
    - [Preliminary Modelling](#preliminary-modelling)
    - [K Nearest Neighbors](#k-nearest-neighbors)
    - [XGBoost](#xgboost)
    - [Random Forest](#random-forest)
    - [Chosen Model](#chosen-model)
- [Insights of Data & Conclusion](#insights-of-data--conclusion)
    -  [SHAP and LIME](#shap-and-lime)
    -  [Insights and Conclusions](#insights-and-conclusions)
- [Closing Remarks](#closing-remarks)
- [Video and Presentation Slides](#video-and-presentation-slides)

# Welcome Message

Welcome to Team 9's SC1015 Mini-Project. This Mini-Project gave our team the opportunity to venture beyond the syllabus
and gain insightful skills & knowledge through the analysis of real world data.

We would also like to thank our TA Mo Zhan Feng for his consultations during lab and outside of lab timings. This Mini-Project would
not have been possible without his valuable feedback and expertise in the field of Data Science.

# Team 9 Members

| Name              |                     Area of Focus                     |GitHub Acount|
|:---:|:---:|---|
| Audric |        Singstats Dataset, EDA, Machine Learning        |[@audd-ho](https://github.com/audd-ho)|
| Kenneth |     Video Presentation, GitHub Repository, EDA      |[@wjkenneth](https://github.com/wjkenneth)|

# Question/Problem Definition

Singapore's birth rate had been on a decline and is a critical issue many individuals are concerned about. People like to attribute this decline to many
hypothesised causes like HDB prices, inflation rate, the culture in singapore and many more. As such, we would like to truly figure out what is the cause of
this decline in birth numbers and help pinpoint the problem, leading us to the problem we have:

> *What are the key factors contributing to the declining birthrate in Singapore? How can we address this issue and encourage more births in Singapore?*

# Dataset Selection & Preparation

After extensive online searching, our team has identified and collated suitable datasets from all around SingStats for
this Mini-Project on [SingStats](https://www.singstat.gov.sg/find-data/search-by-theme?type=all).

However, the datasets on SingStats are separated
into multiple files spanning across different years and there were quite a lot of missing and irrelevant data. Thus, our team had to
clean up the data by combining all the useful data from different excel and csv files into a their respective [files](https://github.com/wjkenneth/SC1015-Mini-Project/tree/main/Data%20Cleaning) and 
merge the data based on their dates via transposing of dataframes, and converting the year column to datetime format which we could
then use for our Exploratory Data Analysis.

We ended up with 3 different sets of datasets:
- Monthly data
- Yearly/Annual data
- Combined data where the monthly data is scaled to yearly to combine the data together

# Exploratory Data Analysis

For our Exploratory Data Analysis we conducted it in multiple jupyter notebooks for different timelines, in a file
titled [EDA](https://github.com/wjkenneth/SC1015-Mini-Project/tree/main/EDA) and here are our findings:

## General Trend

When analyzing the general trend of `live birth number` , we noticed the main trend which is:

- General Fall in `live birth number` since the start, 1960

## Causes and Symptoms

During our Exploratory Data Analysis, noticed a few variables with strikingly high correlation to the live birth numbers:
- Fertility Rate
- Total Infant Death

![image](https://github.com/wjkenneth/SC1015-Mini-Project/blob/main/images/annual_correlation.png)

![image](https://github.com/wjkenneth/SC1015-Mini-Project/blob/main/images/monthly_correlation.png)

However, we noted that the variables are likely to be symptoms of the live birth number as they are affected and calcuated using live birth numbers.

As such, although their high correlation may indicate their usefulness in predicting future live birth numbers, we noted that they are unlikely to be
causes of the live birth numbers' change and are not areas we should focus on for solving the live birth numbers issue at the end.

We will still keep these data as they will be useful in predicting the live birth number, but will keep in mind that they are likely to just be
symptons and not causes.

Additionally, although monthly data has lower correlation as compared to annual data, due to the larger number of datapoints, we consider them to be
more reliable data and as such, will weigh them, with equal or higher weightage as the annual data even when they have lower correlation.

## Linear Regression

We did some preliminary linear regression to check the usefulness of our data in predicting the future live birth numbers. However, the high MSE and
very low and even negative R^2 values showed that the data may not be as useful when predicting the number of live births in the future.

![image](https://github.com/wjkenneth/SC1015-Mini-Project/blob/main/images/bad%20regression.png)

## Change of Project Path

Due to our linear regression analysis and the lack of number of datapoints, we decided to change our problem to a classification focused one where we would
predict whether the future live birth number would increase or decrease.

This would allow our results to be more reliable and accurate compared to one from a regression modelling. Thus allowing our final results to be more
effectively used and trusted.

## Additional EDA

Now that our focus is a classification problem, we cleaned our data further and made an additional column of variable indicating how the live birth number
have changed, whether it increased or decreased.

![image](https://github.com/wjkenneth/SC1015-Mini-Project/blob/main/images/change%20to%20classification.png)

As such, we did further EDA with boxplot to ensure our data is valid for this classification model. 

![image](https://github.com/wjkenneth/SC1015-Mini-Project/blob/main/images/boxplot_classification.png)

The clear difference in some data when the birth number increased or decreased assured us that this was the right path to go on.

# Machine Learning

We mainly employed 3 different modeling methods.

- K Nearest Neighbors
- Random Forest
- XGBoost

## Preliminary Modelling
After a preliminary modelling with the inital data, we found that the accuracy was very low.

As such, we went back to further modify our dataset to become columns of "Change in xx" instead of the variable itself. This is because we
now wanted to find the change in live birth number, as such, our dataset had to become a relative value as well.

We converted the dataset to both "Change in xx" and "Percentage Change in xx" as we wanted to see which provided a better accuracy. Both datasets
gave an improvement of up to 30% in accuracy for some yearly frequency datasets.

![image](https://github.com/wjkenneth/SC1015-Mini-Project/blob/main/images/change%20to%20percent%20change.png)

As initial findings proved the percentage change dataset to be more accurate, we will focus on the percentage change dataset more while keeping the
flat change in dataset at the side for reference as well.

## K Nearest Neighbors

The K-Nearest Neighbors algorithm, also known as KNN, is a non-parametric, supervised learning classifier, which uses proximity to make 
classifications or predictions about the grouping of an individual data point.

As KNN is not a particularly advanced model with weights for the variables, we had to employ feature engineering to remove the unnecessary variables
which negatively affects the accuracy rate. This helped us to improve the accuracy by about 20% to 30%.

We obtained at best, 79% accuracy for the combined dataset.

## XGBoost

XGBoost stands for “Extreme Gradient Boosting”. It is an ensemble learning method that combines the predictions of multiple weak models to produce a
stronger prediction. 

One of the key features of XGBoost is its efficient handling of missing values, which allows it to handle real-world data with missing values without
requiring significant pre-processing. 

Due to the low inital accuracy with the default parameters, we decided to adjust the parameters accordingly to improve the accuracy, such as:
- Decreasing the max depth of of the yearly datasets to account for its lesser datapoints
- Increasing gamma for the monthly data to prevent overfitting

We also used GridSearchCV.
GridSearchCV is the process of performing hyperparameter tuning in order to determine the optimal values for a given model.

Taking the number of datapoints of each dataset into account, we chose a cross validation of 3 for the yearly dataset and 5 for the monthly dataset.
The parameters obtained was in line and similar with our initial tuning as well.

With the optimal parameters set, we were able to further improve the accuracy score of the model to 95.55% for the combined dataset.

## Random Forest

Random forests is an ensemble learning method for classification which combines multiple decision trees to create a more accurate and robust model.

It reduces overfitting by averaging multiple decision trees and is less sensitive to noise and outliers in the data.

Similarly, we also tried to improve the accuracy via tuning the hyper parameters in similar ways to XGBoost.

Afterwards, we also used GridSearchCV to get the optimal parameters, just like the XGBoost model.

With the optimal parameters set, we were able to further improve the accuracy score of the model to 93.01% for the combined dataset.

## Chosen Model

![image](https://github.com/wjkenneth/SC1015-Mini-Project/blob/main/images/model_comparison.png)

We chose the XGBoost model in the end as it had both the highest accuracy among the different models and the ability to handle the missing values present the best.

This was important as we were working with differing time series data, resulting in some variables having missing NaN values for certain time periods.

The other models required as to fill in missing values with mean data which is unreasonable for time series data, and extrapolated data which may still be naive, 
introducing extrapolation bias.

Hence, XGBoost was the best model for our project between these models.

We further ran the model again without variables that we have noted down to be symptoms, rather than causes of the live birth number to obtain a clearer
understanding of the factors affecting the live birth number.

# Insights of Data & Conclusion

## SHAP and LIME

We used SHAP(SHapeley Additive exPlanations) and LIME (Local Interpretable Model-agnostic Explanations) techniques to draw conclusions on which factors are the most
important in affecting the change in live birth number.

## Insights and conclusions

From this Mini-Project we can draw a few conclusion:

1. Symptoms of the live birth number are useful in predicting how the live birth numbers change in the future. Some of these include:
    - Fertility Rate
    - Infant Death Number
These variables can be added into the model when we want to predict how the live birth number change in the future.

2. Multiple factors all affects the live birth numbers and a few of them include
    - Inflation Rate
    - Median Income
    - Residents Aged 20-39
    - Female Marriage Rate
    - Number of Resale Houses Sold
These variables are the one we should aim to tackle and focus on to improve the live birth number.

We also realised a few fatal limitation in our project which affects the reliablility of our results. They are:
- Lack of data points
- Various Missing Values NaN

The lack of data points due to the start date of the recording of the data cannot be solved. In this case, we could obtain more variables and factors to
provide a more telling and full picture of the situation for the model to work with. This would help improve the accuracy and reliability of our model.

Therefore, going back to our initial question, to determine how to improve the live birth number, we can start by tackling the problems and factors outlined
above. For example, trying to control and keep the inflation rate at a lower amount and increasing the income or providing subsidies for people. Appropriate
solutions to tackle each factor could be provided to help alleviate the birth number crisis that is occuring now.

# Closing Remarks

This Mini-Project was a tough and difficult journey, but fruitful nonetheless. I would like to thank my teammate for his
efforts and collaboration throughout the project and our TA for his advice and quick response to our queries. The semester
has been a great experience, it allowed us to learn many new concepts and ideas. Although, this the end of our Mini-Project,
the takeaways from this project will stay with us for life.

# Video and Presentation Slides
- [1015 Mini Project](https://www.youtube.com/watch?v=O-aoSIKnv8A)
- [Presentation Slides](https://github.com/wjkenneth/SC1015-Mini-Project/blob/main/Presentation%20Slides.pdf)

# References
- https://www.singstat.gov.sg
- https://www.geeksforgeeks.org/xgboost/
- https://www.tibco.com/reference-center/what-is-a-random-forest
- https://www.ibm.com/topics/knn#:~:text=The%20k%2Dnearest%20neighbors%20algorithm%2C%20also%20known%20as%20KNN%20or,of%20an%20individual%20data%20point.

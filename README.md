# Project 4: Tweet-Based Identification of Power Outages


## Problem Statement

The goal of this project is to improve upon current processes that identify power outages; by leveraging social media to decrease the amount of time it takes for emergency management officials to become aware of an outage, response times can be improved.
 

## Executive Summary
### Background:
Twitter is a widely used social-media platform that allows its users to publicly post 280-character messages (140-characters prior to November 2017) on virtually any topic one can imagine. Many of these messages are simply reports of users' reactions to current events, or user status updates during those events. Accordingly, there are a large number of messages, called tweets, generated when widely disruptive events, including power outages, occur. In theory, many of these messages will be sent out by users very quickly upon their becoming aware of an outage, and thus the tweets may be shared to social media before emergency management officials become aware of the outage.

### Notebooks:
- [01. Data Collection](https://git.generalassemb.ly/iceberg/DSI-Client-Project/blob/master/01_Data_Gathering.ipynb)
- [02.1 Twitter Data Cleaning, Preprocessing, and EDA](https://git.generalassemb.ly/iceberg/DSI-Client-Project/blob/master/02.1_Data_Cleaning_Tweets.ipynb)
- [02.2 Weather/Power Outage Data Cleaning, Preprocessing, and EDA](https://git.generalassemb.ly/iceberg/DSI-Client-Project/blob/master/02.2_Data_Cleaning_Weather_and_Outage.ipynb)
- [03. Merging Dataframes](https://git.generalassemb.ly/iceberg/DSI-Client-Project/blob/master/03_Merge_Weather_and_Tweets.ipynb)
- [04. Modeling](https://git.generalassemb.ly/iceberg/DSI-Client-Project/blob/master/Modeling.ipynb)

### 1. Data Collection

#### Twitter Data
We collected tweets using [TweetScraper](https://github.com/jonbakerfish/TweetScraper), but it should be noted that we only arrived at this solution after spending about a week trying other techniques without successfully compiling a usable collection of historical tweets.

Here is a brief summary of the issues we encountered:
- *Twitter's free API only allowed access to the seven most recent days of historical tweets. Further access required a costly subscription.*
- *While using the [TwitterScraper](https://github.com/taspinar/twitterscraper) package, we quickly exceeded the maximum number of API requests allowed and were prevented from pulling additional tweets.*
- *Before out access was blocked, TwitterScraper appeared to return 400 tweets per request, but, once we set the tweets in a pandas dataframe, we discovered that each request pulled only 20 unique tweets (repeated 20 times).*

We ran TweetScraper using the query "power outage conedison" and retrieved 4,375 tweets ranging from 2007-07-20 to 2019-04-18. The inclusion of 'conedison' implicitly narrowed the results of our scrape to the New York City area, which proved important since the data did not specify geographical locations of the tweets.

#### Weather Data
Weather data was gathered from Kaggle's [Historic Hourly Weather](https://www.kaggle.com/selfishgene/historical-hourly-weather-data#weather_description.csv) dataset, which includes hourly weather data for New York City from 10/1/2012 to 10/27/2017.

#### Outage Data
Power outage data was gathered from NYC Open Data's [OEM Emergency Notification](https://data.cityofnewyork.us/Public-Safety/OEM-Emergency-Notifications/8vv7-7wx3/data) database, which includes data on official NYC Office of Emergency Management notifications dating back to 2009. Power outage notifications contained the term "Power Outage" in the *Notification Title* column.

### 2.1 Twitter Data Cleaning, Preprocessing, and EDA
Cleaning, preprocessing, and EDA of the Twitter data are described in detail in [this notebook](https://git.generalassemb.ly/iceberg/DSI-Client-Project/blob/master/02.1_Data_Cleaning_Tweets.ipynb). Natural language processing was used to explore the vocabulary in the tweets, in order to identify language that could improve model accuracy.

### 2.2 Weather and Power Outage Data Cleaning, Preprocessing, and EDA

Cleaning, preprocessing and EDA of weather and power outages data are described in detail in [this notebook](https://git.generalassemb.ly/iceberg/DSI-Client-Project/blob/master/02.2_Data_Cleaning_Weather_and_Outage.ipynb). Exploratory analysis was done on weather data to identify patterns that could be identified in Twitter posts/tweets; weather being a predominant cause of power outages. Power outage notifications from NYC Open Data's [OEM Emergency Notification](https://data.cityofnewyork.us/Public-Safety/OEM-Emergency-Notifications/8vv7-7wx3/data) provided labels for predictive modeling. 

### 3. Merging Dataframes
The two dataframes (Twitter and Weather/Outages) were merged before modeling. Merging and final dataframe shown in detail in [this notebook](https://git.generalassemb.ly/iceberg/DSI-Client-Project/blob/master/03_Merge_Weather_and_Tweets.ipynb).

### 4. Modeling
We employ a Logistic Regression (lr) and a Support Vector Classifier (SVC) to tackle the problem. Given the nature of the problem, we were confronted with an imbalanced class problem. This problem was addressed using class weights. 
We find that the results are sensitive to class weights. We also find that the trade-off between the recall and precision appears to be more pronounced for the SVC than the lr.  


## Conclusions and Next Steps

Weather was confirmed as being strongly correlated to power outages. Initial research predicted high outage occurences during summer and winter storms. Current work showed higher occurences of power outages during summer storms only. However, findings are limited to small size of confirmed outage data. 

Further work with Twitter data also required. While tweets containing reports of power outages in New York City were collected, quality and size of this data can be improved with more resources and/or less restrictions from Twitter's API.

Next steps to extend current work and conclude in final product that will improve response time to power outages:

1. Logistic Regression and Support Vector Classifier models were used to classify tweets as those reporting power outages and tweets that do not contain report of power outages. Other models could be employed to better handle large number of features and imbalanced classes.
2. The inclusion of weather data as predictors in classification model could result in better model performance. It is known from research and current procedures of utility companies, that there is a strong correlation between weather and power outages.
3. Mapping of power outages to locations, in a clear map application that will visually allow for identification of areas where there are power outages. The extent of power outages would be visible via map application.
4. Recall was the guiding metric for model performance. More focus can be given to other classification metrics to improve overall model performance.
5. Testing of model with streaming tweets, with no fiter of stream. All tweets would be captured and power outages would be identified from these. This would allow for evaluation of model on unseen data.
6. Application of model that classifies reports of power outages from social media (Twitter) in real time. 

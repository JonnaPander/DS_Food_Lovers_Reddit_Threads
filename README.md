### Contents:
- [Project Overview](#Overview)
- [Problem Statement](#Problem-Statement)
- [Executive Summary](#Executive-Summary)
- [Data Dictionary](#Data-Dictionary)
- [Data and Resources](#Sources)
- [Reddit API](#API)
- [Data Cleaning](#Data-Cleaning)
- [Model Building](#Model-Building)
- [Model Performance](#Model-Performance)
- [Visualization](#Visualization)
- [Conclusions/Recommendations](#Conclusions/Recommendations)

<a id=Overview></a>
# Reddit Thread Classifier: Project Overview
* Utilized Reddit API & Natural Language Processing (NLP) to create a supervised learning tool for classifying Reddit Threads
* Pulled over 17,000 submissions from whole30 and IndianFood Subreddits using python 
* Applied bs4 (BeautifulSoup) python package for parsing HTML 
* Used time module to delay requests per Reddit's limit of 60 requests per minute
* Applied Logistic Regression with Count Vectorizer and utilized GridsearchCV to find the best model
* Applied Multinomial Naive Bayes with TfidfVectorizer

<a id=Problem Statement></a>
## Problem Statement

I have a collection food recipes, mostly Indian food dishes, I’ve been saving from different Subreddit threads.  I plan on doing the Whole30 challenge, how can I take a bank of Subreddit submissions collected from multiple subreddit and dechiper which subreddit it came from? I want to make sure this amazing recipe I saved from subreddits is Whole30 approved.

<a id=Executive Summary></a>
## Executive Summary

I've developed a smart tool using data science that is geared toward Indian food lovers who want to know if the amazing recipe they just found complies with the stringent clean food standards of the now infamous Whole30 challenge.  The subreddits in question are IndianFood and whole30.  

This tool saves the Indian food lover the painstaking task of reading the books, the blog, and the dreaded food labels to finally get a grasp on which of their favorite dishes they can and can’t eat during the challenge.

This tool takes the text of a recipe description, i.e. reddit title, and runs algorithms to determine if the origin of the recipe is the whole30 reddit thus meaning it is likely to be Whole30 approved. Pushshift Reddit API and BeautifulSoup were used to pull and parse over 10,000 submission titles per thread for data processing.  Sklearn statistical modeling was used to generate predictions and evaluate Logistic Regression and Multinomial Naive Bayes models. 

Several complex modeling algorithms in conjunction with Natural Language Processing (NLP) techniques were assessed to find the perfect fit and the best model to make the prediction. The models included Logistic Regression with CountVectorizer and GridSearch and Multinomial Naive Bayes with TFIDFVectorizer. The winning model was the Naive Bayes with a Training Accuracy Score of 96.15% and a Testing Accuracy Score of 93.52%. This beats the Baseline Accuracy of 48.23%. Based on the confusion matrix the Sensitivity (True Positive Rate) is 91.97% and the Specificity (True Negative Rate) is 94.96%.

<a id=Data Dictionary></a>
## Data Dictionary
|Feature|Type|Dataset|Description|
|---|---|---|---|
|**Titles**|*object*|IndianFood|Title of Subreddit submission|
|**Subreddit**|*object*|IndianFood|Name of the Subreddit of origin|
|**Titles**|*object*|whole30|Title of Subreddit submission|
|**Subreddit**|*object*|whole30|Name of the Subreddit of origin|

<a id=Sources></a>
## Data and Resources
[IndianFood](data/IndianFood.csv) <br>
[whole30](data/whole30.csv) <br>
**Python Version:** 3.7.6   
**Packages:** pandas, requests, time, numpy, sklearn, matplotlib, seaborn, bs4 (BeautifulSoup), regex, nltk     

<a id=API></a>
## Pushshift Reddit API
Pulled 14,055 submissions from IndianFood Subreddit and 13,097 submissions from whole30 Subreddit. Each request pulled the following:
*	Submission Title
*	Subreddit Name

<a id=Data Cleaning></a>
## Data Cleaning
After pulling the data, I prepared it before running it through an ensemble of models. I made the following changes and created the following variables:

*	Exported data to Excel to prevent having to run the requests each time I worked with the data
*	Combined the data from each Subreddit into one dataframe for processing
*	Checked for nulls, viewed data types, and removed titles that did not contain textual data but where http links
*	Mapped the name of each Subreddit (IndianFood, whole30) to a binary numeric value (0, 1)
*	Created a function that applied regex to remove titles with characters that where not numbers or letters (numbers where important to keep because they are used frequently in whole30) and parse the titles into individual words while removing stopwords

Here is why keeping numbers was important to indentifying whole30 Subreddit:
![alt text](https://github.com/JonnaPander/DS_Food_Lovers_Reddit_Threads/blob/master/assets/ltr_num.PNG "Importance of Keeping Numbers")

<a id=Model Building></a>
## Model Building 
Once the data was cleaned and the target variables converted to binary values, I prepared the data for modeling. I then split the data into train and tests sets with a test size of 33%.   

I ran the following models to assess the accuracy scores:   
*	**Logistic Regression** – Utilized CountVectorizer and a GridSearchCV to find the best parameters
*	**Multinomial Naive Bayes** – Utilized TFIDFVectorizer 

<a id=Model Performance></a>
## Model performance
Both models performed similarly, beating the Baseline Accuracy of 0.48, with Naive Bayes performing slightly better on the testing set. 
*	**Logistic Regression** : Training Accuracy = 0.96, Testing Accuracy = 0.93
*	**Multinomial Naive Bayes**: Training Accuracy = 0.96, Testing Accuracy = 0.94

<a id=Visualization></a>
## Visualization
![alt text](https://github.com/JonnaPander/DS_Food_Lovers_Reddit_Threads/blob/master/assets/HistOutcomes.PNG "Histogram of Outcomes")
![alt text](https://github.com/JonnaPander/DS_Food_Lovers_Reddit_Threads/blob/master/assets/Matrix.PNG "Confusion Matrix")

<a id=Conclusions/Recommendations></a>
## Conclusions/Recommendations
The tool whittles down the bank of recipes to recipes that are likely to be Whole30 approved with an testing accuracy score of 94%. This is a great starting point for building an Indan food themed Whole30 compliant recipe collection. However, in the tools current state, if you are an Indian food lover who wants to take on the Whole30 challenge you should perform a secondary check of the recipes classifications generated by the tool to ensure they meet the stringent food restrictions of Whole30. Currently, the model is overfit to the training data. This tool could be marketable with further improvements such are regularization and further processing of data with natural language processing (NLP) techniques to make better predictions.  


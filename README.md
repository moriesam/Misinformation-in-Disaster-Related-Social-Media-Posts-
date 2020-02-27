# **Identifying Misinformation in Disaster-Related Social Media Posts**

___

## Introduction:  



The past two decades have seen a meteoric rise of social media, particularly in allowing individuals the ability to reach ever-increasing numbers of peers with little to no cost. This amplification has significantly increased the voice of many who had previously been disenfranchised; however, it has also led to an unprecedented amount of opinion being portrayed as fact, or downright falsehoods being touted as gospel.  

While reasonable discourse is natural when it comes to democratic societies, having falsehoods and lies propagate the zeitgeist regarding major disasters and pandemics can have devastating real-world consequences.  Twitter is a company that has reaped the benefits of this new age of connectivity, while also allowing for the spread of misinformation that has affected hundreds of millions of people. Their response to misinformation has and will continue to have wide-ranging consequences. 

___

## Problem Statement:

***How can Emergency Management Agencies identify incorrect information before, during and after disasters?***

___

## Contents:
- [Introduction](#Introduction)
- [Problem Statement](#Problem-Statement)
- [Data Cleaning/Feature Cleaning](#Data-CleaningFeature-Cleaning)
- [Modeling](#Modeling)
- [Application Development](#Application-Development)
- [Takeaways](#Takeaways)
- [Next Steps](#Next-Steps)
- [Recommendations](#Recommendations)
- [Sources](#Sources)

___

## Data Cleaning/Feature Cleaning:



Our team worked to produce both a training dataset and a testing dataset. The testing set contained just over 1600 tweets, which were grabbed using the tool "Twitter Scrape". The tweets were then transformed in a TFiDFVectorizer (tvec) from the sklearn framework. The tvec was tuned using a GridSearch method measuring on accuracy of a logistic regression. English stopwords were used with a 5,000 limit of features out of a (1, 2) ngram range. Once transformed, 8 classification techniques were tested to find the highest accuracy score. Our training data was obtained from the Kaggle competition 'Real or Not? NLP with Disaster Tweets'. We took out all unnecessary columns, and removed punctuation using RegEx. No missing values, special values or outliers were discovered during the cleaning process. We then tokenized and lemmatized our text. 

Next we identified the percentage of tweets that were labeled as either 'relevent' or 'not relevent', identifying the top twenty key words that had the highest correlation with either of the labels, and charted and graphed them.

We then used the tool "Textblob" to generate sentiment scores, specifically Subjectivity and Polarity. After this was performed we used Count Vectorizer, with parameters of an ngram_range = (1,2) min_df = 10, max_df = 1.0, and having our stop_words = 'english'.
Next we used the TFIDF Vectorizer on the same data, with parameters of ngram_range = (1,2), min_df = 15, max_df = 1.0, and stop_words = 'english'. Both were pickled and then used to create DataFrames that would be used going forward.  

___

## Modeling:  

All of our models final scores were computed in the following order: 
- **Best score without tuning**
- **Best score of existing data with hyperparameters tuned**
- **Best score of new data with hyperparamaters tuned**
- **ROC_AUC score**
- **Precision score**  


#### **Logistic Regression**
Our modeling began with Logistic Regression with Standard Scaling. Our solver was 'liblinear', our penalty was {'l1','l2'} and our CV = 5. This model produced scores of 74.1%, 89.5%, 76.1%, 82.3% and 72.0%.   

Our next model was Logistic Regression with TFIDF and Standard Scaling. With the same solver, penalty and cv hyperparameters as in our previous model, this model produced scores of 77.7%, 82.5%, 80.5%, 86.7% and 81.7%, respectively. 

#### **RandomForests**
Our next model was using Random Forest with Scaling. Our parameters were as follows: 'n_estimators': [100, 150, 200], 'max_depth': [None, 5, 10], and 'min_samples_split' : [2,3, 5]. With our CV = 5, the model produced scores of 77.%, 95.7%, 79.1%, 84.7% and 81.0%. 

Next, we used Random Forest with TFIDF and Scaling. With the same parameters as our previous Random Forest model, the model produced scores of 78.5%, 95.7%, 79.1%, 84.7% and 81.8%. 

#### **Ada Boosting**
Our next model was utilizing Ada Boost with TFIDF. We had the following parameters: 'n_estimators': [50,100], 'base_estimator__max_depth': [1,2], and 'learning_rate': [.9, 1.]. With CV = 5, the model produced scores of 75.6%, 84.8%, 77.8%, 82.1% and 77.3%, respectively.

#### **Naive Bayes**
The next model we used was Multinomial Naive Bayes. This model did not provide a best score without tuning. The model produced scores of 81.3% on existing data, 80.1% on new data, a ROC_AUC score of 81.3%, and a precision score of 82.2%.

The last model we ran the data through was Gaussian Naive Bayes. We achieved a best score of existing data of 74.4%, best score of new data of 73.4%, a ROC_AUC score of 72.8% and a precision score of 90.2%. 


We determined that the best performing model was the Multinomial Naive Bayes, with an score on new data of 80.1%. This model also had the least over-fitting of any model with similar results.
___


## Application Development:

For a practical application of our results, a web app would be created that implements our model within a web browser. Before a tweet or a given amount of text is designated as either 'relevant' or 'not relevant', the back-end  must call on the data processing, modeling, and necessary databases.  

The architecture of the back-end has three components; the database, the web app framework and the data processing code. The data processing code is the jupyter notebooks used in the data analysis, then ported into python files for easy API access. The database would most likely be built using the SQLite package. Once tweets are classified as relevant to the natural disaster, the tweet data is stored in the database to be accessed by the web app. This would enable future updates to 'learn' from previously designated tweets, leading to an increase in accuracy.

___

## Takeaways: 

First, our training data was collected using a self-created API scrape. We were able to narrow our search to the coronavirus, and then used that data to confirm that the model was able to identify new tweets as 'relevant' or 'not relevant'. Although our training data focused on one topic, our goal was to have our model be able to distinguish between true or false tweets regarding disasters as a whole. 

Second, the amount of data required to provide a useful tool would be substantial. To act in an ethical manner, users would have to be informed about how results were achieved and what data would be utilized from them going forward. Potential users would need to feel confident in using the tool and know that any and all personal data was safe from third parties without their explicit consent.

Lastly, the idea of providing a tool that attempts to be an arbiter of truth with regards to disaster information is a significant responsibility. Getting something incorrect can and would be life-altering. One such example could be drawn from the coronavirus. If the model labeled a tweet as 'relevant', stating an area was safe to travel through when that was not the case, people could catch the virus, leading to illness and possibly death. Would the model be 'responsible' for anything it got wrong? Would the team who built it? These are tough questions that need further clarification before this app could be 'live'.
___

## Next Steps: 

- Test our model utilizing text within articles from reputable news agencies, including BBC and Reuters

- Create rules for metrics validation 

- Attempt unsupervised modeling on the data labeled 'not relevant' to gain further insight on patterns and improve model effectiveness

- Train the model for other social media platforms, including Facebook and Instagram

- Extend global translation on all tweets
___

## Recommendations:

- Partner with disaster response organizations that have access to datasets previously unavailable for improved model performance

- Utilize additional NLP tools, including Neural Networks, to determine improve model accuracy
___

## Sources:


[TextBlob Documentation](https://textblob.readthedocs.io/en/dev/ "TextBlob Documentation")

[Tweetscrape](https://pypi.org/project/tweetscrape/ "Twitter Scraper")

[Scikit-learn](https://scikit-learn.org/stable/tutorial/index.html "Scikitlearn")

[Kaggle Dataset](https://www.kaggle.com/c/nlp-getting-started/data "Kaggle Dataset")

___

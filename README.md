# Predicting Blockbusters

* * *
### 1) Introduction and Motivation 


#### Overview:
Movies hold significant cultural importance in today's world, with the movie industry generating billions of dollars annually. Many people dedicate their free time to watching movies or planning outings to theaters. However, how often have you finished watching a movie only to realize that it was a disappointment despite high expectations?

So, what truly determines a movie's success? Some movies boast budgets in the hundreds of millions yet underperform at the box office. Is there a way to utilize numerical data and metrics associated with each movie to predict its success even before its release? While there has been considerable research on predicting movies based on user ratings on social media, there's a gap in research focusing on analyzing the intrinsic data associated with movies [1].

#### Goal:
Our aim is to develop the capability to forecast a movie's success based on factors such as its budget, runtime, and popularity*. This prediction could prove valuable to both movie producers and the audience. Filmmakers could gain insights from correlations to guide their creative process, while the audience could make informed decisions about whether a movie is worth their time and money to watch in theaters.

>*Popularity is determined by factors such as daily votes, views, favorites, and watchlist additions compared to previous scores, along with its release date. TMDB employs this metric to enhance search results and highlight popular content during users' visits to the website. Even before a movie's release, popularity can gauge growing interest as the release date approaches and gauge post-release reception.

* * * 

### 2) Dataset and Methods 
#### TMDB 5000 Movie Dataset from Kaggle
- https://www.kaggle.com/tmdb/tmdb-movie-metadata 

This Kaggle dataset draws from TMDB due to copyright constraints related to the IMDB website.

The dataset consists of two sources: `tmdb_5000_credits` and `tmdb_5000_movies`. 

*tmdb_5000_credits features:*
- movie_id, title, cast, crew

*tmdb_5000_movies features:* 
- budget, genres, homepage, id, keywords, original_language, original_title, overview, popularity, production_companies, production_countries, release_date, revenue, runtime, spoken_languages, status, tagline, title, vote_average, vote_count   

We opted to merge these data sources to leverage the full range of available features.


  ![DataSet](https://github.com/zach-hb/Predicting-Blockbusters/blob/main/dataHead.PNG) 

* * * 

### 3) Feature Selection and Data Manipulation 

Initially, we excluded data points where values, such as duration or vote_average, equaled zero as they were implausible.

Our definition of a "successful" movie relied on three metrics: 'Popularity Success,' 'Vote Success,' and 'Commercial Success.'
1. 'Popularity Success': A movie's popularity surpasses the mean of all popularity scores. 
2. 'Vote Success': A movie's voting average exceeds the mean of all vote averages. 
3. 'Commercial Success': A movie's gross profit outpaces its budget. 

A movie is considered *truly successful* if it satisfies all three criteria. 

#### Preprocessing and Method Selection: 

We adopted a binary classification approach, introducing threshold values for specific columns. For each movie, we assigned a 1 or 0 to these columns, enabling various algorithms to determine its potential success. For example, for budget, we introduced seven new classifier columns, each representing a budget range. Similarly, we transformed 'title_month' and 'duration' into classifiers. For genre, the dataset provided text information, which we mapped to individual columns with corresponding binary values.

We also examined the correlation between different features and success factors and decided to drop features with weak correlations, such as 'homepage' and 'vote_count.' Certain attributes like actor names, directors, and languages were excluded, leading to a final modified dataset with 4716 rows and 35 columns.

* * *  

### 4) Supervised Learning Models 

To train our models, we randomly divided our data, allocating 75% for training and 25% for predictions. 


#### Decision Tree - Cross Validation Score  

Our initial supervised learning model was the decision tree. We experimented with different depths, including 7. The decision tree with depth 7 yielded slightly improved results, potentially due to the increased splits accounting for more features. However, to avoid overfitting, we opted for a moderate number of splits.

Depth = 7 

Cross validation accuracy = 77%

Accuracy = 79%

![Decision Tree](https://github.com/zach-hb/Predicting-Blockbusters/blob/main/Images/DecisionTreeCF.png) 


#### Random Forest Tree Model

Next, we employed the random forest tree model, which consists of multiple decision trees based on random subsamples of the data. As expected, the results were similar to those of the decision tree.


  ![RandomForest](https://github.com/zach-hb/Predicting-Blockbusters/blob/main/Images/randomforest.png) 


Depth = 7 

Cross validation = 76%

Accuracy = 79%


#### #### K-nearest neighbors

We also explored the K-nearest neighbors model. It's like having a group of friends nearby who help you decide whether something is similar or different. Instead of following a specific set of rules like the previous decision tree and random forest, K-nearest neighbors just remembers the examples it has seen before and uses them to make new decisions. To make our predictions more reliable and able to handle unusual cases, we considered 100 nearby neighbors instead of the default 5. K-nearest neighbors turned out to be more accurate than both the decision tree and random forest.

With 100 neighbors:

- Cross validation accuracy: 79%
- Actual accuracy: 79%

  ![K-nearest neighbors](https://github.com/zach-hb/Predicting-Blockbusters/blob/main/Images/KNNCF.png)

#### Support Vector Machine (SVM)

Finally, we tested a model called the support vector machine. It's like drawing a line between two different groups of things to tell them apart. With a linear method, SVM performed similarly or even a bit better than K-nearest neighbors on average. Among all the models we tried, SVM stood out as the best choice for our classification problem.

- Cross validation accuracy: 76%
- Actual accuracy: 79%

  ![Support Vector Machine](https://github.com/zach-hb/Predicting-Blockbusters/blob/main/Images/SVMCF.png)

These models provided insights into predicting movie success, with K-nearest neighbors and SVM offering the most promising results.

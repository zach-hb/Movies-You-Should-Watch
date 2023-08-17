# Anticipating a Movie's Triumph Before Release

* * *
### 1) Introduction and Motivation 

#### Preface:
We made the decision to shift our focus from the dataset initially proposed to a different Kaggle dataset concerning movie ratings. This shift was prompted by the realization that our previous dataset was better suited for visualization projects and unsupervised learning.

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

<p align="left">
  <img src="images/dataHead.PNG"> 
</p>

<br>

#### Pairwise Plot 

Given the presence of both categorical and continuous features, we employed a pairplot to better visualize relationships and guide our approach to handling these diverse types.

<p align="center">
  <img src="images/pairwise.PNG" width="900"> 
</p>

* * * 

### 3) Feature Selection and Data Manipulation 

Initially, we excluded data points where values, such as duration or vote_average, equaled zero as they were implausible.

Our definition of a "successful" movie relied on three metrics: 'Popularity Success,' 'Vote Success,' and 'Commercial Success.'
1. 'Popularity Success': A movie's popularity surpasses the mean of all popularity scores. 
2. 'Vote Success': A movie's voting average exceeds the mean of all vote averages. 
3. 'Commercial Success': A movie's gross profit outpaces its budget. 

A movie is considered *truly successful* if it satisfies all three criteria. The pie charts below illustrate the percentage breakdown of movies in the dataset that fulfill each success factor.

Popularity Success          |  Vote Success             | Commercial Success
:-------------------------:|:-------------------------:|:------------------:
<img src="images/popularitySuccess.PNG" width="300" /> | <img src="images/voteSuccess.PNG" width="300" />  | <img src="images/commercialSuccess.PNG" width="350" />

<br> 
Thus, out of all movies in the dataset, only 21.3% are genuinely successful by our team's standards.

<p align="left">
  <img src="images/success.PNG" width="300"> 
</p>

<br> 

To further identify significant features, we created visualizations to discern relationships between features and the success of movies.

Budget x True Success      |Title_month x True Success | Duration x True Success
:-------------------------:|:-------------------------:|:------------------:
<img src="images/budgetxSuccess.PNG" width="300" /> | <img src="images/monthxSuccess.PNG" width="300" />  | <img src="images/durationxSuccess.PNG" width="300" />

#### Preprocessing and Method Selection: 

We adopted a binary classification approach, introducing threshold values for specific columns. For each movie, we assigned a 1 or 0 to these columns, enabling various algorithms to determine its potential success. For example, for budget, we introduced seven new classifier columns, each representing a budget range. Similarly, we transformed 'title_month' and 'duration' into classifiers. For genre, the dataset provided text information, which we mapped to individual columns with corresponding binary values.

We also examined the correlation between different features and success factors and decided to drop features with weak correlations, such as 'homepage' and 'vote_count.' Certain attributes like actor names, directors, and languages were excluded, leading to a final modified dataset with 4716 rows and 35 columns.

* * *  

### 4) Supervised Learning Models 

To train our models, we randomly divided our data, allocating 75% for training and 25% for predictions. 


#### Decision Tree - Cross Validation Score  

Our initial supervised learning model was the decision tree. We experimented with different depths, including 7 and 3. The decision tree with depth 7 yielded slightly improved results compared to depth 3, potentially due to the increased splits accounting for more features. However, to avoid overfitting, we opted for a moderate number of splits.

Depth = 7 

Cross validation accuracy = 73%

Accuracy = 80%

<img src="images/decisionTree7.png"/> 

<p align="left">
  <img src="images/decisionTree.PNG" width="300"> 
</p>

Depth = 3 

Cross validation accuracy = 73%

Accuracy = 79%

<img src="images/decisionTree3.png" height="300"/> 


<br>

#### Random Forest Tree Model

Next, we employed the random forest tree model, which consists of multiple decision trees based on random subsamples of the data. As expected, the results were similar to those of the decision tree.

<p align="left">
  <img src="images/randomForest.PNG" width="300"> 
</p>

<img src="images/randomForest7.png"/>

Depth = 7 

Cross validation = 74%

Accuracy = 82%

<img src="images/randomForest3.png"/>

Depth = 3 

Cross validation = 78%

Accuracy = 80%

<br>


####
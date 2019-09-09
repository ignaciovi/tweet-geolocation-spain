Tweet Location
===

# Introduction
We propose a framework for estimating Twitter user's location based solely in the text of the tweets.
We will replicate an existing paper: *You Are Where You Tweet: A Content-Based Approach to Geo-locating Twitter Users* (http://faculty.cs.tamu.edu/caverlee/pubs/cheng10cikm.pdf), a research project which focuses the prediction on US states area.
In our project we will aim to predict the Spanish Autonomous Community (AC: first-level political and administrative division in which Spain is divided) from which the tweet belongs.

We will define a set of research question that will be studied in the document and the limitations of this project will be analysed:

- RQ1. Are there clear location signals embedded in this mix of topics and interests that can be identified for locating a user? We will analyse local words
- RQ2. Does mentioning specific places of a AC makes the user more likely to be located in that AC?

Local words are normally used by people that have been born and raised in the culture of that Autonomous Community. For simplicity sake, we are assuming that a person that tweets from a place has been born there and therefore have slang from that area.

The goal is to predict the origin location of a user. Therefore, we will first make predictions of tweets and then we will average on the most frequent predicted AC of his/her tweets.

# Work methodology
The way I structured the development of this project has been through setting sprints, Agile style, and moving each change made to Github. This way, I can keep an organised code and come back to different stages of the project. In a small project like this, the methodology presented might not be necessary, but this is a good praxis to develop since it can be used to projects with more code like FrontEnd development where each component can be develop independently in different branches and you decide what you want to move to Production at each moment without involving everything at the same time and putting in risk the appearance of interdependence between different components.

# Data acquisition
The approach followed for data selection was to take tweets from random dates. This way, we make sure that tweets are not biased by the events of a particular day (e.g. football match).
Since Twitter API doesn't allow to fetch tweets from specific dates, we will take a number of users and take tweets from their timeline. Taking tweets from different users we reduce once again the bias. If we take 100 tweets per user for example, we assume that this user didn't write the 100 of them the same day, which seems like a good assumptions. This way we reduce bias and increase the chances of improving the prediction

Therefore, the data acquisition steps will be:

1. Get most recent tweets of a location
First of all, we will define an area in which we want to obtain tweets from. This can easily be defined using coordinates of approximately the center of a AC  and setting a radium on which we want to fetch the tweets. The API function (https://developer.twitter.com/en/docs/tweets/search/api-reference/get-search-tweets) will fetch a number X of the most recent tweets in that location

2. For each tweet, get the user id 
If we take for example 500 tweets of an area, we will have 500 user ids (considering they are not duplicated)

3. Fetch X recent tweets of each user obtained
We will take for each user a number X of the recent tweets of their timeline.

We will then obtain a dataset like follows:

[id, tweet, location]
[3317191107, @1980SFC sabes por dónde echan el ARG-BRA de esta madrugada? Gracias y un abrazo., Andalusia]

In the paper mentioned in the introduction, a base dataset of 1,074,375 user profiles and 29,479,600 tweets was collected.
If we divide this quantitiy by the number of states in the US, we will get an approximation (considering there is a data balance) of the number of tweets needed per each state: around 590k tweets per state used in their prediction.

In this project we will start with a lower quantity. We will take 500 users per CA and 100 tweets per user
This makes a total of 50k tweets per AC.

In this project we will simplify our dataset to 5 ACs:
Andalusia, Madrid, Catalonia, Basque Country and Canary Islands

# Text processing

In order to clean the tweets text we apply the following rules:
- Lowercase text
- Remove accents
- Remove user mentions (@name) and word "RT" that is very common in tweets
- Remove URLs in tweets
- Remove stopwords
- Stemming

This pipeline gives as output a clean dataset of tweets.

# Most frequent words for each AC
Taking all the tweets for each AC we take a look at the most common words:

![](https://i.imgur.com/2qoOCvZ.png)
![](https://i.imgur.com/PpWLvPh.png)
![](https://i.imgur.com/RIQw992.png)
![](https://i.imgur.com/6ENZoWn.png)
![](https://i.imgur.com/0Vr8k4y.png)

We see that most of the words are similar for each CA.
However, for Canary Islands the word "Tenerife" appears frequently, same for Madrid with the word "Madrid"
Therefore, we can establish a relation between mentioning a city of a AC and belonging to that AC (closing here the research question).

Let´s se the total count of mentions:

For Andalusia:
    Andalusia counts 89
    Madrid counts 41
    Catalonia counts 13
    Basque_Country counts 1
    Canary_Islands counts 0

For Madrid:
    Andalusia counts 12
    Madrid counts 142
    Catalonia counts 17
    Basque_Country counts 1
    Canary_Islands counts 0

For Catalonia:
    Andalusia counts 16
    Madrid counts 47
    Catalonia counts 133
    Basque_Country counts 2
    Canary_Islands counts 0

For Basque_Country:
    Andalusia counts 18
    Madrid counts 87
    Catalonia counts 15
    Basque_Country counts 44
    Canary_Islands counts 0

For Canary_Islands:
    Andalusia counts 1
    Madrid counts 52
    Catalonia counts 17
    Basque_Country counts 4
    Canary_Islands counts 0
    
It is not true for all of the AC's, but it can help in the prediction.

# Prediction and Evaluation
In order to help our model focus more on meaningful words, we can use a TF-IDF score (Term Frequency, Inverse Document Frequency) on top of our Bag of Words model. TF-IDF weighs words by how rare they are in our dataset, discounting words that are too frequent and just add to the noise.

We will train the model and make predictions for each tweet independently. Once the predictions are made, we will average the result of the prediction for all the tweets of a user. The most frequent among the tweets will be the location of the user.
Therefore, for training set we will randomise all the users that we have an take 90% of them. For each user, we will put all his/her tweets in the training bag.
For testing dataset, we will take the remaining 10% of users.

Following we find the results of the accuracy according to different models.
We will use accuracy since we care about the user's that were correctly predicted and the appearance of false positives is not a big issue in this case like it would be in for example the prediction of cancer yes/no.

Accuracy for Random Forest: 0.44
Accuracy for Linear SVC: 0.57
Accuracy for Logistic Reg: 0.50

As we see, we obtain a decent prediction but still low to be used in real life users. We observed that mentioning cities of a AC can be correlated with the user being from that AC, but this can't be considered as a strong feature to obtain good predictions.
We don´t have strong indicators to predict the location of a user based solely on its tweets.
Probably for local languages like Catalan and Basque we can increase the accuracy but for the rest of spanish communities we don´t have enough clues to give a valid and accurate prediction of the location.
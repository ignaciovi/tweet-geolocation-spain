Tweet Location
===

# Introduction
We propose a framework for estimating Twitter user's location based solely in the text of the tweets.
We will replicate an existing paper: You Are Where You Tweet: A Content-Based Approach to Geo-locating Twitter Users, a research project which focuses the prediction on US states.
In our project we will focus the prediction in Spanish Autonomous Communities (CA) instead.

Predicting using only the text of a tweet offers certain limitations. We will define a set of research question that will be studied in the document, studying those limitations:
RQ1. Are there clear location signals embedded in this mix of topics and interests that can be identified for locating a user? What about Words typical of each CA
RQ2. Does mentioning specific places of a CA makes the user more likely to be located in that CA?

Supposing that we have X CAs, we will make a prediction for a set of tweets of a specific user. Then, we will take the prediction that was more frequent between those tweets and that will be the location of the user.

# Getting data

## Approach
The best approach for data selection is to take tweets from random dates. This is because if we take just tweets from a specific date maybe they talk about something in particular that happen that day and it would be biased.
Since apparently is not possible to fetch tweets from specific dates, we will take a number of users and take tweets from their timeline

1. Get tweets of location
2. For each tweet, search location of the user
3. If location is within the bounding box (there is an algorithm to give true or false for this?), save user id
4. Take user id, search its timeline and take X tweets

In the paper they collected a base dataset of 1,074,375 user profiles and 29,479,600 status updates.
If we divide by the number of states in the US we will get an approximation (considering balance) of the number of tweets per each state: around 590k per state

We will check first for 100k per Comunidad Autonoma
If we take 100 tweets per user we will need 1k users per CA

# Tweet preprocessing

2. Dont get the RT username
3. Filter links
4. Transform accent into not accent
5. Transform ñ into ny


# First study of the differences in vocabulary

Take 10k tweets from 100 users and study the common words


# Prediction

We will predict location of a user, not of a tweet.
So for each tweet of the user find a probability of belonging
THen average among all users tweets and take the CA that has most probability
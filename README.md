# Create ML Models With BQML

This exercise is part of my Data Engineering and Machine Learning Fundamental training provided by Google. ‘Create ML Models with Big Query ML Challenge Lab’ is a non-guided lab under the quest of ‘Create ML Models with Big Query ML’ from Google Skills Boost. 

# Scenario

You have started a new role as a junior member of the Data Science department Jooli Inc. Your team is working on a number of machine learning initiatives related to urban mobility services. You are expected to help with the development and assessment of data sets and machine learning models to help provide insights based on real work data sets.

# Challenge

One of the projects you are working on needs to provide analysis based on real world data that will help in the selection of new bicycle models for public bike share systems. Your role in this project is to develop and evaluate machine learning models that can predict average trip durations for bike schemes using the public data from Austin's public bike share scheme to train and evaluate your models.

Two of the senior data scientists in your team have different theories on what factors are important in determining the duration of a bike share trip and you have been asked to prioritise these to start. The first data scientist maintains that the key factors are the start station, the location of the start station, the day of the week and the hour the trip started. While the second data scientist argues that this is an over complication and the key factors are simply start station, subscriber type, and the hour the trip started.

You have been asked to develop a machine learning model based on each of these input features. Given the fact that stay-at-home orders were in place for Austin during parts of 2021 as a result of COVID-19 you will be working on data from previous years. You have been instructed to train your models on data from 2017 and then evaluate them against data from 2020 on the basis of Mean Absolute Error and the square root of Mean Squared Error.

You can access the public data for the Austin bike share scheme in your project by opening this link to the Austin bike share dataset in the browser tab for your lab.

As a final step you must create and run a query that uses the model that includes subscriber type as a feature, to predict the average trip duration for all trips from the busiest bike sharing station in 2020 (based on the number of trips per station in 2020) where the subscriber type is 'Single Trip'.

Lab link: https://www.cloudskillsboost.google/focuses/14294?parent=catalog

Dataset: https://console.cloud.google.com/bigquery?p=bigquery-public-data&d=austin_bikeshare&page=dataset&project=sacred-particle-340305


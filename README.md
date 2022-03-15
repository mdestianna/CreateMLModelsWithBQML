# Create ML Models With BQML

This exercise is part of my Data Engineering and Machine Learning Fundamental training provided by Google. ‘Create ML Models with Big Query ML Challenge Lab’ is a non-guided lab under the quest of ‘Create ML Models with Big Query ML’ from Google Skills Boost. 

## Scenario

You have started a new role as a junior member of the Data Science department Jooli Inc. Your team is working on a number of machine learning initiatives related to urban mobility services. You are expected to help with the development and assessment of data sets and machine learning models to help provide insights based on real work data sets.

## Challenge

One of the projects you are working on needs to provide analysis based on real world data that will help in the selection of new bicycle models for public bike share systems. Your role in this project is to develop and evaluate machine learning models that can predict average trip durations for bike schemes using the public data from Austin's public bike share scheme to train and evaluate your models.

Two of the senior data scientists in your team have different theories on what factors are important in determining the duration of a bike share trip and you have been asked to prioritise these to start. The first data scientist maintains that the key factors are the start station, the location of the start station, the day of the week and the hour the trip started. While the second data scientist argues that this is an over complication and the key factors are simply start station, subscriber type, and the hour the trip started.

You have been asked to develop a machine learning model based on each of these input features. Given the fact that stay-at-home orders were in place for Austin during parts of 2021 as a result of COVID-19 you will be working on data from previous years. You have been instructed to train your models on data from 2017 and then evaluate them against data from 2020 on the basis of Mean Absolute Error and the square root of Mean Squared Error.

You can access the public data for the Austin bike share scheme in your project by opening the Austin bike share dataset on link below.

As a final step you must create and run a query that uses the model that includes subscriber type as a feature, to predict the average trip duration for all trips from the busiest bike sharing station in 2020 (based on the number of trips per station in 2020) where the subscriber type is 'Single Trip'.

Lab: https://www.cloudskillsboost.google/focuses/14294?parent=catalog

Dataset: https://console.cloud.google.com/bigquery?p=bigquery-public-data&d=austin_bikeshare&page=dataset&project=sacred-particle-340305

## Task 1 Create dataset

## Task 2 Create forecasting BQML model
Create the first machine learning model to predict the trip duration for bike trips. The features of this model must incorporate the starting station name, the hour the trip started, the weekday of the trip, and the address of the start station labeled as location. You must use 2017 data only to train this model.

```
CREATE or REPLACE MODEL biketrip.biketrip_model1
OPTIONS
  (model_type='linear_reg', labels=['duration_minutes']) AS
SELECT
    B1.start_station_name,
    EXTRACT(HOUR FROM B1.start_time) AS starthour,
    EXTRACT(DAYOFWEEK FROM B1.start_time) AS dayofweek,
    B2.address as location,
    duration_minutes
FROM
    `bigquery-public-data.austin_bikeshare.bikeshare_trips` as B1 
INNER JOIN 
    `bigquery-public-data.austin_bikeshare.bikeshare_stations` as B2
ON B1.start_station_id = B2.station_id
WHERE
    EXTRACT(YEAR FROM B1.start_time) = 2017
    AND duration_minutes > 0
```

## Task 3 Create the second machine learning model
Create the second machine learning model to predict the trip duration for bike trips. The features of this model must incorporate the starting station name, the bike share subscriber type and the start time for the trip. You must also use 2017 data only to train this model.

```
CREATE or REPLACE MODEL biketrip.biketrip_model2
OPTIONS
  (model_type='linear_reg', labels=['duration_minutes']) AS
SELECT
    start_station_name,
    subscriber_type,
    EXTRACT(HOUR FROM start_time) AS starthour,
    duration_minutes
FROM
    `bigquery-public-data.austin_bikeshare.bikeshare_trips` 
WHERE
    EXTRACT(YEAR FROM start_time) = 2017
    AND duration_minutes > 0
```

## Task 4 Evaluate the two machine learning models
Evaluate each of the machine learning models against 2020 data only using separate queries. Your queries must report both the Mean Absolute Error and the Root Mean Square Error.

### 1st Model
```
SELECT SQRT (mean_squared_error) AS rmse,
    Mean_absolute_error
FROM ML.EVALUATE(MODEL biketrip.biketrip_model1,
    (
    SELECT
    B1.start_station_name,
    EXTRACT(HOUR FROM B1.start_time) AS starthour,
    EXTRACT(DAYOFWEEK FROM B1.start_time) AS dayofweek,
    B2.address as location,
    duration_minutes
    FROM
    `bigquery-public-data.austin_bikeshare.bikeshare_trips` as B1 
INNER JOIN 
    `bigquery-public-data.austin_bikeshare.bikeshare_stations` as B2
ON B1.start_station_id = B2.station_id
WHERE
    EXTRACT(YEAR FROM B1.start_time) = 2020
    AND duration_minutes > 0)
    )
```

### 2nd Model
```
SELECT SQRT (mean_squared_error) AS rmse,
    Mean_absolute_error
FROM ML.EVALUATE(MODEL biketrip.biketrip_model2,
    (
    SELECT
    start_station_name,
    subscriber_type,
    EXTRACT(HOUR FROM start_time) AS starthour,
    duration_minutes
    FROM
    `bigquery-public-data.austin_bikeshare.bikeshare_trips` 
WHERE
    EXTRACT(YEAR FROM start_time) = 2020
    AND duration_minutes > 0)
    )
```

## Task 5 Use the subscriber type machine learning model to predict average trip durations
When both models have been created and evaulated, use the second model, that uses subscriber_type as a feature, to predict average trip length for trips from the busiest bike sharing station in 2020 where the subscriber type is Single Trip.

```
SELECT
   start_station_name,
   COUNT(*) AS trips
FROM `bigquery-public-data.austin_bikeshare.bikeshare_trips`
WHERE EXTRACT(YEAR FROM start_time) = 2020
GROUP BY start_station_name
ORDER BY trips DESC
```
```
SELECT AVG(predicted_duration_minutes) as average_duration_minutes
FROM ML.PREDICT(MODEL `biketrip.biketrip_model2`, 
(
SELECT
    start_station_name,
    subscriber_type,
    EXTRACT(HOUR FROM start_time) AS starthour,
    duration_minutes
FROM
    `bigquery-public-data.austin_bikeshare.bikeshare_trips`  
WHERE
    EXTRACT(YEAR FROM start_time) = 2020
    AND subscriber_type = 'Single Trip (Pay-as-you-ride)'
    AND start_station_name = 'Riverside/South Lamar'))
```






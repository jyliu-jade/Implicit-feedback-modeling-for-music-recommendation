# Implicit-feedback-modeling-for-music-recommendation
This is a project for Spring 2019 DSGA-1004 Big Data

# Overview

In the final project, we apply the tools we have learned in this class to build and evaluate a recommender system.  

## The data set

On Dumbo's HDFS, you will find the following files in `hdfs:/user/bm106/pub/project`:

  - `cf_train.parquet`
  - `cf_validation.parquet`
  - `cf_test.parquet`
  
  - `metadata.parquet`
  - `features.parquet`
  - `tags.parquet`
  - `lyrics.parquet`
  
  
The first three files contain training, validation, and testing data for the collaborative filter.  Specifically, each file contains a table of triples `(user_id, count, track_id)` which measure implicit feedback derived from listening behavior.  The first file `cf_train` contains full histories for approximately 1M users, and partial histories for 110,000 users, located at the end of the table.

`cf_validation` contains the remainder of histories for 10K users, and should be used as validation data to tune your model.

`cf_test` contains the remaining history for 100K users, which should be used for your final evaluation.

The four additional files consist of supplementary data for each track (item) in the dataset.  

## Basic recommender system 

Our recommendation model uses Spark's alternating least squares (ALS) method to learn latent factor representations for users and items.  This model has some hyper-parameters thatwe tune to optimize performance on the validation set, notably: 

  - the *rank* (dimension) of the latent factors,
  - the *regularization* parameter, and
  - *alpha*, the scaling parameter for handling implicit feedback (count) data.
  
Evaluations are based on predictions of the top 500 items for each user.

## Extensions 
  - *Alternative model formualtions*: the `AlternatingLeastSquares` model in Spark implements a particular form of implicit-feedback modeling, but you could change its behavior by modifying the count data.  Conduct a thorough evaluation of different modification strategies (e.g., log compression, or dropping low count values) and their impact on overall accuracy.
  - *Fast search*: use a spatial data structure (e.g., LSH or partition trees) to implement accelerated search at query time.  For this, it is best to use an existing library such as `annoy` or `nmslib`, and you will need to export the model parameters from Spark to work in your chosen environment.  For full credit, you should provide a thorough evaluation of the efficiency gains provided by your spatial data structure over a brute-force search method.
 

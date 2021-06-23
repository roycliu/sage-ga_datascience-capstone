# sage-ga_datascience-capstone
## [Deployed App](https://roy-liu-sage-ga.herokuapp.com/capstone) (host on Heroku)

## Selecting Dataset

- First I was training recommendation system using Amazon user review 2018 dataset. However the challenges are
  - dataset is way too large, in hundred MB size
  - average review per user is 1, 
    - not much variation in terms of distances
    - and ends up with a really super sparse sparse matrix
  - not easy to clean; e.g. a lot of NaN, 
  - no product details in review datasets, and has to use Amazon product API to load
    - there is a daily download quota, very hard to do large dataset training

## Cleaning Dataset

- Searching on other datasets in the meanwhile, found steam video game datasets on Kaggle
  - Two datasets are both under 50 MB size
  - 200k rating, 12k users, and 40k titles are sufficient
  - easy to clean; just some dropping some columns and filtering out rows with too  NaN
  - still sufficient data left after cleansing
- After cleansing, got errors while inner joining datasets by game title
  - explicitly converted title column into Unicode 
  - further dropped few columns that were not needed for training

## Building System

- Recommendation for Cold Start User
  - Top played (ranked by hour played)
  - Top sold (ranked by unit sold) 
-  Collaborative Filtering Recommendation 
  - Create pivot table and sparse matrix
    - straightforward using cleansed datasets
  - Creating pairwise distance matrix
  - Creating a Cosine distance matrix to double check the pairwise distance matrix

## Preparing for Deployment

- Saved needed DataFrames into csv files
- Prepared a genre DataFrame only for building the app
  - Realized that 40k game titles are too large to fit in a dropdown
  - Not all titles have user data 
  - Get unique genre off the inner joined dataset, only 16k title left

## Developing & Deploying the App

- To be able to recommend by user preference, collect user genre and favorite title first
  - return the top 10 titles that similar users have played
- most challenging part is creating data table using filtered result in Series format
  - tried two different format, DataTable vs IFrame

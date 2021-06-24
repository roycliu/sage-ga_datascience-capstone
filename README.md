# sage-ga_datascience-capstone
  
  
## [Deployed App](https://roy-liu-sage-ga.herokuapp.com/capstone) (platform: Heroku)
## [Jupyter Notebook](https://github.com/roycliu/sage-ga_datascience-capstone/blob/main/capstone/Game%20Recommendation%20System.ipynb)
## ©Dataset Source: Kaggle
- [Stem Video Games](https://www.kaggle.com/trolukovich/steam-games-complete-dataset)
- [Stem User Behavior](https://www.kaggle.com/tamber/steam-video-games)
  
  ![image](https://user-images.githubusercontent.com/37002271/123145167-680ac180-d411-11eb-8333-fc1a4599e1bf.png)

  
  
## 🤨Selecting Dataset

- ⛔First I was training recommendation system using __Amazon user review 2018__ dataset (UCSD). However the challenges are
  - ❌dataset is way too large, in half GB size
  - ❌average rating per user is 1 product, 
    - not much variation in terms of distances
    - and ends up with a really super scattered `sparse matrix`
  - ❌not easy to clean; e.g. a string encoding issue and excessive NaN columns & rows, 
  - ❓no product details in review datasets, and has to use Amazon product API to load
    - there is a daily download quota, very hard to do large dataset training

## 🧹Cleaning Dataset

- Searching on other datasets in the meanwhile, found Steam video game datasets at Kaggle
  - An online game hub with 6M ~ 20M concurrent online user
  - Has made real-time player data steam available to public
  - ✅Two Steam datasets found are both under 50 MB size
  - ✅good size for project purpose
    - 200k rating, 12k users, and 40k titles are sufficient
  - ✅easy to clean; just had to drop some columns and filter out rows with too NaN in required columns
  - ✅still sufficient data left after the clean up
  - note, found __Netflix__ dataset at Kaggle is also a good candidate
- ❗After cleansing, got errors while inner joining datasets on `game title` column. To resolve the issue had to
  - explicitly converted title column into Unicode , and 
  - further dropped few columns that were not needed for training (left them for app not model)

## 🏭Building System

- Recommendation for `Cold Start User`
  - 🥇Top played (ranked by hour played)
  - 🏆Top sold (ranked by unit sold) 
- Tried Key word, types & genre based recommendation: (not part of the app)
-  Collaborative Filtering Recommendation 
  - Create pivot table and sparse matrix
    - straightforward using cleansed datasets
  - Creating pairwise distance matrix
  - 🎯Creating a Cosine distance matrix to double check the pairwise distance matrix

## 🧰Preparing for Deployment

- 💾Saved needed DataFrames into csv files
- Prepared a genre DataFrame only for building the app
  - Realized that 40k game titles are too large to fit in a dropdown
  - Not all titles have user data 
  - Get unique genre off the inner joined dataset, only 16k title left

## 📊Developing & Deploying the App

- To be able to recommend by user preference, collect user genre and favorite title first
  - return the top 10 titles that similar users have played
- most challenging part is creating data table using filtered result in Series format
  - tried two different format, DataTable vs IFrame
  
  
![image](https://user-images.githubusercontent.com/37002271/123145786-0a2aa980-d412-11eb-8152-b38c358103b4.png)



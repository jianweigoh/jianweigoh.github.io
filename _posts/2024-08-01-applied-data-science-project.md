---
layout: post
author: jianweigoh
title: "Applied Data Science Project Documentation"
categories: ITD214
---
## Project Background
The use of Machine Learning to Track Audience Preferences and Guide the Development of a Good Movie/Show 

Investing in the production of movies and television shows demands substantial capital investment and extensive manpower coordination. However, the reality is that not all projects achieve commercial success or profitability. Our team hope to leverage machine learning techniques to discern audience preferences and trends. If successful, we can mitigate the risks of production failures. With the wealth of information now available, we aim to showcase how data-driven decisions that align closely with consumer preferences can be made, and subsequently maximise the potential for success.

Our business goal is to able to sustainably identify popular topics among audiences and predict the success of prospective films, accomplished by the following business objectives:
1. Train a text classification model to predict the sentiment of a movie review, to automate the labelling of the sentiment for new movie reviews on IMDB.
2. Perform topic modelling on the movie reviews to uncover insights on the topics prevalent in positive reviews and likewise for negative reviews
3. Train a binary classification model to predict prospective films’ IMDB score and hence its potential popularity level
4. Train a regression model to predict prospective films’ IMDB votes and hence its potential audience engagement level --> Train a binary classification model to predict the likely scale of prospective films’ IMDB votes, and hence its potential audience engagement level

![image](https://github.com/user-attachments/assets/5239c401-e02a-43ec-9fbd-01153ae98821)

Proposed success criterions are as follows:
- To automate the classification of new movie reviews with a 80% accuracy
- To identify at least 2 topics that are associated with positive sentiment
- Predict for a prospective film, at 60% accuracy rate or a normalised RMSE of 0.2 for its IMDB score and IMDB number of votes

This repository studies business objective #4

## Work Accomplished
Business objective #4 studies the feasibility of creating regression and classification model to predict a film's imdb vote count, based on the dataset's features. Dataset obtained from https://www.kaggle.com/datasets/maso0dahmed/netflix-movies-and-shows was subjected to exploratory data analysis (EDA), data cleaning, data transformation, before performing modelling and evaluation. Details in the following sections.

### Exploratory Data Analysis (EDA)
EDA for this project was conducted in 4 broad strokes. 

Firstly, basic pandas head and info methods were applied to visualise the dataset studied. 

![image](https://github.com/user-attachments/assets/eec06086-a966-42cf-a7fd-f5df7b0335c5)

Secondly, and EDA table function (adapted from https://www.kaggle.com/code/keishibata/netflix-movies-and-shows-eda-wordcloud) was applied to understand the number of missing, unique, and pandas describe method values across all the features in the dataset. 

![image](https://github.com/user-attachments/assets/805f04dd-e725-426e-8b03-6f0c789494ba)

Thirdly, the ydata_profiling library was used to generate quick histogram visualization of available features, and check for insights that might have been missed in earlier EDA steps.

![ydatacharts](https://github.com/user-attachments/assets/58bb23ab-c478-4937-ac51-f4f99d13914b)

Lastly, additional visualisations were performed to supplement the histograms generated with ydata_profiling and for more focused data exploration of input and target variables

![image](https://github.com/user-attachments/assets/30b6a220-2bd3-4a8f-ad2e-c96f0fddf46e)

### Data Preparation
Duplicate entries were removed based on title AND year of release. This is due to the nature of movies, whereby it is common to see remakes under the same movie title. Such movies should be treated as unique records during modelling. Records will NAN target variable values were also dropped.

Next, we remove entries that do not make logical sense
- Records without target variable
- Records with runtime that is = 0

The dataset on hand collects information of both movies and television shows. Compared to movies, shows have shorter runtime, but can be aired over multiple seasons / episodes. Hence, propose to consolidate runtime and seasons into a single feature ('runtime_combined'), to better reflect the total duration of the title.

One hot encoding was also performed on categorical features
- Type (Movie vs Show)
- Genres
- Production country
- Content rating
- Year of movie release

Additional feature engineering
- The first production country listed was mapped to its respective continent to reduce number of dimensions. All other production countries were discounted
- Content rating was binned to reduce the number of dimensions, and consolidate similar age band rating
- Year of movie release was also binned into 3 separate bins, each with equal size

Features that will not be used for modelling are dropped at this point

### Modelling
Lorem ipsum dolor sit amet, consectetur adipiscing elit. Fusce bibendum neque eget nunc mattis eu sollicitudin enim tincidunt. Vestibulum lacus tortor, ultricies id dignissim ac, bibendum in velit. Proin convallis mi ac felis pharetra aliquam. Curabitur dignissim accumsan rutrum. In arcu magna, aliquet vel pretium et, molestie et arcu. Mauris lobortis nulla et felis ullamcorper bibendum. Phasellus et hendrerit mauris. Proin eget nibh a massa vestibulum pretium. Suspendisse eu nisl a ante aliquet bibendum quis a nunc. Praesent varius interdum vehicula. Aenean risus libero, placerat at vestibulum eget, ultricies eu enim. Praesent nulla tortor, malesuada adipiscing adipiscing sollicitudin, adipiscing eget est.

### Evaluation
Lorem ipsum dolor sit amet, consectetur adipiscing elit. Fusce bibendum neque eget nunc mattis eu sollicitudin enim tincidunt. Vestibulum lacus tortor, ultricies id dignissim ac, bibendum in velit. Proin convallis mi ac felis pharetra aliquam. Curabitur dignissim accumsan rutrum. In arcu magna, aliquet vel pretium et, molestie et arcu. Mauris lobortis nulla et felis ullamcorper bibendum. Phasellus et hendrerit mauris. Proin eget nibh a massa vestibulum pretium. Suspendisse eu nisl a ante aliquet bibendum quis a nunc. Praesent varius interdum vehicula. Aenean risus libero, placerat at vestibulum eget, ultricies eu enim. Praesent nulla tortor, malesuada adipiscing adipiscing sollicitudin, adipiscing eget est.

## Recommendation and Analysis
Explain the analysis and recommendations

Lorem ipsum dolor sit amet, consectetur adipiscing elit. Fusce bibendum neque eget nunc mattis eu sollicitudin enim tincidunt. Vestibulum lacus tortor, ultricies id dignissim ac, bibendum in velit. Proin convallis mi ac felis pharetra aliquam. Curabitur dignissim accumsan rutrum. In arcu magna, aliquet vel pretium et, molestie et arcu. Mauris lobortis nulla et felis ullamcorper bibendum. Phasellus et hendrerit mauris. Proin eget nibh a massa vestibulum pretium. Suspendisse eu nisl a ante aliquet bibendum quis a nunc. Praesent varius interdum vehicula. Aenean risus libero, placerat at vestibulum eget, ultricies eu enim. Praesent nulla tortor, malesuada adipiscing adipiscing sollicitudin, adipiscing eget est.

## AI Ethics
Discuss the potential data science ethics issues (privacy, fairness, accuracy, accountability, transparency) in your project. 

Lorem ipsum dolor sit amet, consectetur adipiscing elit. Fusce bibendum neque eget nunc mattis eu sollicitudin enim tincidunt. Vestibulum lacus tortor, ultricies id dignissim ac, bibendum in velit. Proin convallis mi ac felis pharetra aliquam. Curabitur dignissim accumsan rutrum. In arcu magna, aliquet vel pretium et, molestie et arcu. Mauris lobortis nulla et felis ullamcorper bibendum. Phasellus et hendrerit mauris. Proin eget nibh a massa vestibulum pretium. Suspendisse eu nisl a ante aliquet bibendum quis a nunc. Praesent varius interdum vehicula. Aenean risus libero, placerat at vestibulum eget, ultricies eu enim. Praesent nulla tortor, malesuada adipiscing adipiscing sollicitudin, adipiscing eget est.

## Source Codes and Datasets
https://github.com/jianweigoh/ITD214_Assignment

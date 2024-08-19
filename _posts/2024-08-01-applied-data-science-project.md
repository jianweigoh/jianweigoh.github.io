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

Features that will not be used for modelling were dropped at this point

### Modelling - Regression
Prior to modelling, min-max normalisation of the target variable (imdb_votes) was performed due to the large range (5 - 2,268,288). Train test split using a 70:30 ratio was used.

For regression modelling, the following models were used, and their respective mean square error (MSE) and coefficient of determination (R^2) ws obtained as per tables below:

| Model           | MSE      | R2       |
| --------------- | -------- | -------- |
| Linear Regression | 0.0012   | 0.1432   |
| Random Forest   | 0.0012   | 0.1471   |
| Deep Learning   | 0.0039   | -1.7884  |

To reduce the effect of possible noise in the regression models, feature selection through the following methods were performed, along with their corresponding metrics:

| Feature Selection Method                   | MSE      | R2       |
| ------------------------------------------ | -------- | -------- |
| SelectKBest with f_regression              | 0.0012   | 0.1408   |
| Recursive Feature Elimination (RFE)        | 0.0013   | 0.0761   |
| Lasso Regression for Feature Selection     | 0.0013   | 0.0479   |
| Feature Importance Random Forest (RF)      | 0.0012   | 0.1446   |

### Evaluation - Regression
Results from the regression modelling is unfortunately poor, as observed from the dismal R^2 score. It is unlikely for hyper parameter tuning to change the performance significantly. At this point, binary classification modelling is considered, with outcomes being representative of 'having a good number of imdb votes' and 'having a poor number of imdb votes'

### Modelling - Classification
Preliminarily, the threshold was determined based on the median imdb_vote count, which arrived 2471. However, in light of encouraging evaluation metrics in the inital modelling runs, the threshold was set at 100000, which represents a more meaningful number of imdb_vote count to reflect a more well discussed movie/show.

| Model                        | Accuracy | Precision | Recall   |
| ---------------------------- | -------- | --------- | -------- |
| K-Neighbors Classifier (KNN)  | 0.9403   | 0.9209    | 0.9403   |
| Support Vector Classifier (SVM) | 0.9423   | 0.8891    | 0.9423   |
| Logistic Regression (LR)     | 0.9403   | 0.9259    | 0.9403   |
| Decision Tree (DT)           | 0.9259   | 0.9247    | 0.9259   |
| Gaussian Naive Bayes (GNB)   | 0.6328   | 0.9437    | 0.6328   |
| Random Forest (RF)           | 0.9469   | 0.9351    | 0.9469   |
| Gradient Boosting (GB)       | 0.9436   | 0.9324    | 0.9436   |

In light of the high performing metric, propose to check the features to see if there are any input variables that are heavily influencing the results. A feature importance chart was generated to study the aforementioned.

![image](https://github.com/user-attachments/assets/4a44a4e4-d410-401b-9935-0083bed29914)

Based on the results of the feature importance chart, propose to remove the following variables:
- runtime_combinted: to investigate if this input is heavily influencing the model
- Summarised_year_Old, Summarised_year_Modern, and Summarised_year_New: possibility that longer the movie has been released, the more time it has to accumulate votes

Following the feature selection, the same classification models were trained again, yielding the following results and feature importance chart

| Model                        | Accuracy | Precision | Recall   |
| ---------------------------- | -------- | --------- | -------- |
| K-Neighbors Classifier (KNN)  | 0.9351   | 0.9133    | 0.9351   |
| Support Vector Classifier (SVM) | 0.9430   | 0.8892    | 0.9430   |
| Logistic Regression (LR)     | 0.9397   | 0.9062    | 0.9397   |
| Decision Tree (DT)           | 0.9279   | 0.9150    | 0.9279   |
| Gaussian Naive Bayes (GNB)   | 0.5521   | 0.9429    | 0.5521   |
| Random Forest (RF)           | 0.9338   | 0.9145    | 0.9338   |
| Gradient Boosting (GB)       | 0.9397   | 0.9108    | 0.9397   |

![image](https://github.com/user-attachments/assets/7180019b-9a4f-4aeb-8b49-acbd76e395b0)


### Evaluation - Classification
Unlike regression modelling, the results from the classification modelling had been very encouraging. High accuracy, precision and recall (>90%) across the board (with the exception of GNB model) indicates that this model will be fairly reliable in the prediction of whether a film will engage a large number of audience, defined by having more than 100000 votes on imdb. These results far exceed the prescribed 60% accuracy rate for success of the business objective.

## Recommendation and Analysis
We propose that producers can consider using our classification model(s) to predict whether a propsective film in consideration will capture the attention of a large audience. That being said, the following considerations and areas of improvement needs to be considered.
- To monitor for model drift, as audience preferences and trends may change over time
- As the identity of the movie is clear, to consider obtaining other sources of information e.g. lead actors / production team / movie budget and integrating with existing dataset, to improve the robustness of the model
- Consider further feature selection to create a more parsimonious model

## AI Ethics
No personal and confidential information was included in the dataset. Failure of the models ability to accurately prediction of movie/show success is not expected to bring about harm to society, albeit at the cost of the production company. That being said, propose that for the use of this classifcation model, a human-in-the-loop arrangement should be considered, not in view of safety, but rather, there are other considerations when it comes to the creation of a film/show which cannot be sufficiently simplified with machine learning alone. Additional inputs needs to be collected from other sources to make a more informed and stratgic decision. This model should be intended for to support decision making, rather than to automate and draw conclusions on behalf of humans.

![image](https://github.com/user-attachments/assets/04a88708-c0cc-4eb2-a4fd-1acb1dd3f54d)

## Source Codes and Datasets
https://github.com/jianweigoh/ITD214_Assignment

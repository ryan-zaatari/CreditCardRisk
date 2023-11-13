---
layout: default
description: Explore driving factors that indicate credit card risk.
title: Credit Card Risk Model
---

## Introduction/Background

For years, banks have placed an immense focus on assessing loan risk. Substantial research has delved into the analysis of loan risks, aiming to uncover correlations and patterns among people who struggle to payback their loans on time. We look to further investigate these patterns and variations in hopes to gain a better understanding of those who face challenges in meeting their financial gains. 

Our research will be based off of the Kaggle dataset: [Credit Card Fraud Detection](https://www.kaggle.com/datasets/mishra5001/credit-card?select=application_data.csv).It contains over 300,000 people who have taken out loans and indicates whether they were late to pay installments. The dataset contains 122 columns, or features, those of which include gender, annual income, whether one owns assests such as real estate or a car, credit amount, education, and information on previous loan application/history. 

## Problem Statement

Each year, Banks loan out millions of credit cards to customers. A big issue they face is loaning out cards to customers who either will be late to monthly payemnts or not pay at all and charge off. While it might may seem obvious that banks should avoid lending to customers who are likely to not pay back on time, several factors compel them to continue doing so- diversification, competition, and overall reputation. Our objective is to develop a model that better assesses the risk of incoming customers and help identify trends in one's profile that may lead to risk.

## Data Preprocessing and PCA
Our dataset consists of dozens of features, those including gender, education, whether one owns a phone, car, real estate, etc. We also have a feature TARGET that tells us whether an applicant ended up making a late payment. If TARGET = 1, our applicant ended up making a late payment. Applicants where TARGET = 1 we will consider a risk. We hope to reduce the dimensionality of our data through the use of Principal Component Analysis. In order to do this, we first need to clean our data. Of course we begin by removing all null valued entries. We then removed columns where there standard deviation is exactly 0. If all entries share the same value for that particular feature, there is no variance. In our case, FLAG_MOBIL (whether an applicant owns a phone) has a standard deviation equal to 0.

We then transform all data into numerical values. For example, "Male" -> 1, "Female" -> 0 or for education, "Higher education" -> 2, "Incomplete higher" -> 1, "Secondary / secondary special"  -> 0, "Lower secondary" -> -1. 

However, if a feature had non-numerical values that were difficult to code numerically, the feature was deleted. For example, "WALLSMATERIAL_MODE" is a feature analyzing the material of the applicants wall. This vary for every person and as a result may lead to overfitting. These are types of features we chose to remove, reducing our dataset by 30 or so features.

After cleaning our data, we normalize our data so that is possesses a mean of 0 and standard deviation of 1. This makes it so that our data is scaled appropriately to run PCA.

![Alt text](<Elbow Plot.png>)

Notice there is no extremely weighted feature in our dataset. In order to retain 80% of the variance, we need to reduce our data set by 60%, keeping around only 30 features. This is not ideal and something we are aware of. Ideally, we want data where a majority of the variance retained with only 3-5 components. The reason this may not be the case for us is the there is still a lot of noise in our dataset, and that there are a dozen more features that we should intially remove from our dataset. As for now, the following are the 10 features of our dat that hold the most variance.

| Feature Name | Variance |
|----------|----------|----------|
| NAME_EDUCATION_TYPE | 6.87% |
| DAYS_ID_PUBLISH | 5.5% | 
| FLAG_DOCUMENT_8 | 4.33% | 
| REGION_POPULATION_RELATIVE | 3.7% | 
| FLAG_EMAIL | 3.13% |
| EXT_SOURCE_3 | 2.8% |
| NAME_CONTRACT_TYPE | 2.57% | 
| FLAG_OWN_REALTY | 2.52% |
| FLAG_DOCUMENT_6 | 2.48% | 
| FLAG_DOCUMENT_16  | 2.33% | 

Even though our Principal Component Analysis only retains ~17% of the variance with 3 components and ~12% with 2 components, we provide visualizations of PCA with 2 and 3 components on 500 data points. 

![Alt text](<PCA 2D.png>)
![Alt text](<PCA 3D.png>)

## Classification Models
The first model we will use to analyze loan applicants is a Neural Network. Our training data will be 90% of our initial dataset, and the dataset we will be working with will consist of enough features to retain 80% of the initial variance. Before analyze the results of this model, we have to address the fact that our training data is imbalanced. If not, our model can always predict a person is not a risk and be correct ~90% of the time. The following is our initial training data.

![Alt text](<Imbalanced Data.png>)

To compensate for this imbalancement, we create synthetic data to represent risk applicants. This balances our data to be roughly a 50/50 split between risk and non-risk applicants.

![Alt text](<Balanced Data.png>)

Let us now discuss the structure of our Neural Network. As of now, the architechture that has yielded the best results is a 256-128-64-32-16-8-4-2-1 network. The number represents the number of neurons for that hidden layer. Moreover, the activation type for all of our neurons is ReLu, expect for our output neuron which has a Sigmoid activation type. Below are plots of our training and testing data's loss/accuracy per epoch:
* * *

![Alt text](<Accuracy Plot.png>)
![Alt text](<Loss Plot.png>)

Notice that both the accuracy of our testing and training data are increasing. This on the surface is good, but when analyzing the trajcteroies of their loss functions per epoch, it seems like we may have an overfitting problem. As our the number of epochs increase, the loss of our training data approaches whereas the loss of our testing data seems to be increasing. This is not good. Our model may be trying to memorize our training data instead of actually learning. To better understand the following, we can look at the recall and precision of our data for nonrisk and risk applicants:

|  | Precision | Recall | F1 |
|----------|----------|----------|----------|
| Nonrisk | 0.93 | 0.77 | 0.85 |
| Risk | 0.13 | 0.37 | .19 | 

Our model's precision for risk clients is 0.13. This means out of every 100 times our model predicts an applicant is a risk to a bank, it is correct only 13 of those times. T his is not good and something we need to correct. On the other hand, the precision for those who do not possess fraud is quite high at 0.93.

## Review and Next Steps:
* **Reevaluate our data:**
We need to make sure all noise is removed. This will make our Principal Component Analysis more efficient and allow us to better predict risks in our Neural Network. 

* **Review Neural Network Architechture:**
There is a clear overfitting issue in our model. This is either due to our current data or the architechture of our model. If it is due to the architechture of our model, we need to reduce the number of hidden layers or neurons. 

* **Implement another model:**
We are going to implement another model for our data, that being either Naive Bayes or Logistic regression. After getting our neural network working, we hope to compare and contrast the trends picked up from both models to see if we can better understand the data of loan applicants who possess risk.

### Contribution Table

| Name        | Contributions      |
|:-------------|:------------------|
| Colby | GitHub Page, Problem Statement Data Collection, Background/Intro, Methods| 
| Greg | Problem Statement, Gantt Chart, Data Collection| 
| Robbie | Data Collection, Potential Results/Discussions | 
| Ryan | Data Collection, Potential Results/Discussions|
| Carter | Data Collection, Video Presentation |


#### Gantt Chart

[Gantt Chart - Proposal](https://gtvault.sharepoint.com/:x:/s/CS4641MLProjectTeam/EZsWnmIWIcJAk-Xb50eHuWsBEQd3F_iAyf3x9m4M9c_V7Q?e=lOfd4h)

##### References
* [Germanno Teles, Joel J. P. C. Rodrigues, Ricardo A. L. Rabê, Sergei A. Kozlov (2020). Artificial neural network and Bayesian network models for credit risk prediction. Journal of Artificial Intelligence and Systems, 2, 118–132.](https://iecscience.org/public/uploads/jpapers/202003/JUA6ia0ppS0FvH3QgOUl1C09ccBGRCx9SFgQJhtG.pdf)

* [P. Jeatrakul and K. W. Wong, "Comparing the performance of different neural networks for binary classification problems," 2009 Eighth International Symposium on Natural Language Processing, Bangkok, Thailand, 2009, pp. 111-115, doi: 10.1109/SNLP.2009.5340935.](https://ieeexplore.ieee.org/abstract/document/5340935)

* [Shlens, Jonathon. “A Tutorial on Principal Component Analysis.” ArXiv abs/1404.1100 (2014): n. pag.](https://arxiv.org/abs/1404.1100)

* [Mishra5001. (2019, July 15). Credit Card Fraud Detection. Kaggle. https://www.kaggle.com/datasets/mishra5001/credit-card?select=application_data.csv](credit-card?select=application_data.csv)


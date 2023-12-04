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

<div style="display:flex; justify-content:space-around;">
    <img src="PCA 2D.png" alt="PCA 2D.png" width="500"/>
    <img src="PCA 3D.png" alt="PCA 3D.png" width="500"/>
</div>

## Classification Models

### Neural Network
The first model we will use to analyze loan applicants is a Neural Network. We chose a neural network to be one of our models due to the many avaialable pre-existing archiechtures there are for binary classification. Our training data will be 80% of our initial dataset, and the dataset we will be working with will consist of enough features to retain 85% of the initial variance. Before analyze the results of this model, we have to address the fact that our training data is imbalanced. If not, our model can always predict a person is not a risk and be correct ~90% of the time. The following is our initial training data.

![Alt text](<Imbalanced Data.png>)

To compensate for this imbalancement, we create synthetic data to represent risk applicants. This balances our data to be roughly a 50/50 split between risk and non-risk applicants.

![Alt text](<Balanced Data.png>)

Let us now discuss the structure of our Neural Network. Let us first test the following architechture: 256-128-64-32-16-8-4-2-1 network. The number represents the number of neurons for that hidden layer. Moreover, the activation type for all of our neurons is ReLu, expect for our output neuron which has a Sigmoid activation type. Below are plots of our training and testing data's loss/accuracy per epoch:
* * *

<div style="display:flex; justify-content:space-around;">
    <img src="Accuracy Plot.png" alt="Accuracy Plot.png" width="400"/>
    <img src="Loss Plot.png" alt="Loss Plot.png" width="400"/>
</div>

Notice that both the accuracy of our testing and training data are increasing. This on the surface is good, but when analyzing the trajcteroies of their loss functions per epoch, it seems like we may have an overfitting problem. As our the number of epochs increase, the loss of our training data approaches whereas the loss of our testing data seems to be increasing. This is not good. Our model may be trying to memorize our training data instead of actually learning. To better understand the following, we can look at the recall and precision of our data for nonrisk and risk applicants:

|  | Precision | Recall | F1 |
|----------|----------|----------|----------|
| Nonrisk | 0.93 | 0.77 | 0.85 |
| Risk | 0.13 | 0.37 | .19 | 

Our model's precision for risk clients is 0.13. This means out of every 100 times our model predicts an applicant is a risk to a bank, it is correct only 13 of those times. We explored this issue further by testing other common neural network architechures that are used to conduct binary classification:

* **Architecture "64-32-16-8-4-2-1":**

<div style="display:flex; justify-content:space-around;">
    <img src="Accuracy Plot2.png" alt="Accuracy Plot2.png" width="400"/>
    <img src="Loss Plot2.png" alt="Loss Plot2.png" width="400"/>
</div>

|  | Precision | Recall | F1 |
|----------|----------|----------|----------|
| Nonrisk | 0.96 | 0.69 | 0.8 |
| Risk | 0.15 | 0.65 | .25 | 

* **Architecture "128-128-1":**
<div style="display:flex; justify-content:space-around;">
    <img src="Accuracy Plot3.png" alt="Accuracy Plot3.png" width="400"/>
    <img src="Loss Plot3.png" alt="Loss Plot3.png" width="400"/>
</div>

|  | Precision | Recall | F1 |
|----------|----------|----------|----------|
| Nonrisk | 0.94 | 0.77 | 0.85 |
| Risk | 0.15 | 0.47 | .23 | 

### Random Forest Classifier
The second model we chose to use is a Random Forest Classifier. The reason behind this choice is because random forest classifiers are generally harder to overfit. We thought this would be a great choice for a second, as our Neural Network tended to overfit our data. There are three parameters we dealt in our Random Forest Classifier.

* n_estimators: The number of trees in the forest
* min_samples_leaf: The minimum number of samples required to split an internal node
* max_depth: The maximum depth of the tree

 We will fix n_estimators = 100 while attempting to optimize min_samples_leaf and max_depth. The analysis we used is a grid search to exhaust combinations of different values for **min_samples_leaf** and **max_depth**. For every possible combination fo our paramaeters, we do a 5-fold cross-validation, which means the dataset is divided into 5 subsets (folds), and the model is trained and evaluated 5 times. Each time, a different fold is used as the test set, and the remaining folds are used for training. The chosen scoring metric for optimization is accuracy. Below are the results of our testings:

![Alt text](<Forest Optimization Chart.png>)

Notice that as either max_depth or min_samples_leaf decreases, our Mean Test Score decreases. So for us, it turns out that max_depth = 20 and min_samples_leaf = 1 is most optimal for us. The following are the training and testing data results with our "optimal parameters":

<div style="display:flex; justify-content:space-around;">
    <img src="Confusion Train Matrix.png" alt="Confusion Train Matrix" width="400"/>
    <img src="Confusion Test Matrix.png" alt="Confusion Test Matrix" width="400"/>
</div>

Our Training Data Accuracy was 98.3% whereas our testing accuracy came out to be 87.9%. Notice the lack in accuracy is not determing whether someone is not a fraud, but whether or not somebody is a risk to a bank. For this model in particular, we believe this may be because we optimized our parameters to maximize accuracy, not precision. If the metric used was precision, we believe our model would have optimized the following parameters differently. This is something we can look into in the future.

## Model Comparisons
In terms of accuracy, our Random Forest Classifier model was far more accurate than our Neural Network. Our Neural Network testing data accuracy was 76.5% after 25 epochs whereas our testing data accuracy for our RCF was 87.9%. As for the precision of our "Fraudulent Clients", our RCF (13%) yet again out performed our Neural Network (25%). In neither though is this a good or acceptable precision for a model.

What then are possible reasons for this lack of precision in "Fraudulent Clients"? Well as mentioned earlier, for our RCF model, we believe that if we optimized parameters to focus on precision, we would have better results. Now whether or not this would in turn affect overall accuracy, we do not know.

Another possible cause that has led to a lack of precision in our models happens to lie in the data itself. We believe the labelling of a "Fraudlent Client" is too broad. What do we mean by this? A client who pays mulitple installments late every month is labelled "Fraudulent" in our dataset. Now suppose we have a client who has paid all installments of past loans ontime but completely forgot to pay their last installment ontime, and as a result they pay it a day or two late. They are also labelled as "Fraudulent" in our dataset. This severity of late payments is not distinguished in our dataset, which may be making it hard for our model to have a high precision on fraudulent clients. This would also explain why PCA showed no features that held a significant amount of variance over the others.  

### Contribution Table

| Name        | Contributions      |
|:-------------|:------------------|
| Colby | Webpage, Evaluations| 
| Greg | Presentation / Video | 
| Robbie | Model Coding | 
| Ryan | Presentation / Video|
| Carter | Presentation / Video |


#### Gantt Chart

[Gantt Chart - Proposal](https://gtvault.sharepoint.com/:x:/s/CS4641MLProjectTeam/EZsWnmIWIcJAk-Xb50eHuWsBEQd3F_iAyf3x9m4M9c_V7Q?e=lOfd4h)

##### References
* [Germanno Teles, Joel J. P. C. Rodrigues, Ricardo A. L. Rabê, Sergei A. Kozlov (2020). Artificial neural network and Bayesian network models for credit risk prediction. Journal of Artificial Intelligence and Systems, 2, 118–132.](https://iecscience.org/public/uploads/jpapers/202003/JUA6ia0ppS0FvH3QgOUl1C09ccBGRCx9SFgQJhtG.pdf)

* [P. Jeatrakul and K. W. Wong, "Comparing the performance of different neural networks for binary classification problems," 2009 Eighth International Symposium on Natural Language Processing, Bangkok, Thailand, 2009, pp. 111-115, doi: 10.1109/SNLP.2009.5340935.](https://ieeexplore.ieee.org/abstract/document/5340935)

* [Shlens, Jonathon. “A Tutorial on Principal Component Analysis.” ArXiv abs/1404.1100 (2014): n. pag.](https://arxiv.org/abs/1404.1100)

* [Mishra5001. (2019, July 15). Credit Card Fraud Detection. Kaggle. https://www.kaggle.com/datasets/mishra5001/credit-card?select=application_data.csv](credit-card?select=application_data.csv)


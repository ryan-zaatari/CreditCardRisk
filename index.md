---
layout: default
description: Explore driving factors that indicate credit card risk.
title: Credit Card Risk Model
---

## Introduction/Background

For years, banks have placed an immense focus on assessing loan risk. Substantial research has delved into the analysis of loan risks, aiming to uncover correlations and patterns among people who struggle to payback their loans on time. We look to further investigate these patterns and variations in hopes to gain a better understanding of those who face challenges in meeting their financial gains. 

Our research will be based off of the Kaggle dataset: [Credit Card Fraud Detection](https://www.kaggle.com/datasets/mishra5001/credit-card?select=application_data.csv).It contains over 300,000 people who have taken out loans and indicates whether they were late to pay installments. The dataset contains 122 columns, or features, those of which include gender, annual income, whether one owns assests such as real estate or a car, credit amount, education, and information on previous loan application/history. 

## Problem Statement

Each year, Banks loan out millions of credit cards to customers. A big issue they face is loaning out cards to customers who either will be late to monthly payemnts or not pay at all and charge off. While it might may seem obvious that banks should avoid lending to customers who are likely to not pay back on time, several factors compel them to continue doing so- diversification, competition, and overall reputation. Our objective is to develop a model that better assesses the risk of incoming customers.

## Methods

To begin our analysis, we will apply PCA to our extensive dataset. This will allow us to identify which features carry the most influence when assessing credit card risk, given the dozens of attributes available. We hope to gain insight on attributes that carry the most weight. 

Furthermore, we will run supervised binary classification using neural networks, a technique to classify customers into two categories: those who made timely payments and those who did not. This approach will give us a better understanding of customer behavior and how it relates to their profiles.

## Potential Results / Discussion
PCA analysis will indicate that only a subset of these features will be required to accurately predict credit risk. We predict that customers who have had a previous history of timely payments will be much more likely to be approved than those who have not paid off previous loans. We also expect other features such as age, income, familial credit history, and assets to be good indicators of loan reliability.

Overall, our model will enable banks to better evaluate credit risk when reviewing applications. With enhanced predictive capabilities, banks can approve applicants likely to repay debts and avoid issuing credit to high-default risks. This data-driven approach will enable smart credit allocation while expanding access for creditworthy consumers.
* * *

### Checkpoints

**Midterm Report:**
* Expect to have already run PCA analysis in various ways on our dataset.
* Provide visualizations to contrasts the significance of certain features
* Begin our Binary Classification using Neural Networks

**Final Report:** 
* Have our binary classification complete
* If necessary, add an additional model to better analyze trends regarding risk and features (clustering, statistical model, etc.)


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

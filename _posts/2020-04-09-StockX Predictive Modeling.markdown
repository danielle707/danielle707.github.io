---
layout: post
title:  "StockX Price Premium Predictive Analysis"
date:   2020-04-08 08:42:17 -0400
categories: jekyll update
---

<h1 align="center">StockX Price Premium Predictive Analysis
</h1>

![banner](/assets/images/banner.png)

<h4 align="center">Use Machine Learning to learn undervalued sneakers</h4>                        

<p align = "center">
  <a href = "https://www.linkedin.com/in/zhidanwang/">
    <img class = "post image" src = "https://img.shields.io/badge/Linked-In-informational" align = "center"/>
  </a>
  <a href = "https://github.com/danielle707/StockX-Predictive-Modeling">
    <img class = "post image" src = "https://img.shields.io/badge/GitHub-🍺-brightgreen" align = "center"/>
  </a>
  <a href = "https://github.com/danielle707/StockX-Predictive-Modeling/graphs/contributors">
    <img class = "post image" src = "https://img.shields.io/github/contributors/danielle707/StockX-Predictive-Modeling" align = "center"/>
  </a>
</p>


## PROJECT OVERVIEW

- **ABSTRACT**: This project aims to investigate the features behind resale premiums on [StockX](https://stockx.com/sneakers) and their prediction power by conduct feature engineering and utilize external popularity index on different brands

- **ASSUMPTION**: Hot sneakers presents little seaonalities, and in this project we will not discuss time series analysis.
- **DATA**: [StockX Data Challenge 2019](https://s3.amazonaws.com/stockx-sneaker-analysis/wp-content/uploads/2019/02/StockX-Data-Contest-2019-3.xlsx); Demographic Data 2020
- **MODEL**: Tree-based and Linear Regression

|  Tree-based   |       Linear       |
| :-----------: | :----------------: |
| Random Forest |       Lasso        |
|    XGboost    | SVM(Linear Kernel) |

- **RESULT:** We have utilized our strategy and identified undervalued shoes through March 2020, and these shoes have increased their price within 80% of our prediction by the end of March 2020.

  *Out-of-Sample Results:*

|                      Sneaker Name                       |                            Image                             | Retail | Most Recent Sale | Current Premium | Predicted Premium |
| :-----------------------------------------------------: | :----------------------------------------------------------: | :----: | :--------------: | :-------------: | :---------------: |
| **Air Jordan** 1 Retro High Travis Scott(**Tan/Brown**) | <img src="https://stockx-360.imgix.net/Air-Jordan-1-Retro-High-Travis-Scott/Images/Air-Jordan-1-Retro-High-Travis-Scott/Lv2/img01.jpg?auto=format,compress&w=559&q=90&dpr=2&updated_at=1550180948" width="200"/> |  $175  |   $797 - $1486   |     497.4%      |    **802.0%**     |
|      **Blazer** Mid 77 Vintage Slam Jam(**White**)      | <img src="https://stockx-360.imgix.net/Nike-Blazer-Mid-77-Vintage-Slam-Jam/Images/Nike-Blazer-Mid-77-Vintage-Slam-Jam/Lv2/img01.jpg?auto=format,compress&w=559&q=90&dpr=2&updated_at=1554253378" width="200"/> |  $100  |   $470 - $486    |     381.0%      |    **455.5%**     |
|   **Yeezy** Boost 350 V2 Tail Light(**Grey, Orange**)   | <img src="https://stockx-360.imgix.net/adidas-Yeezy-Boost-350-V2-Tail-Light/Images/adidas-Yeezy-Boost-350-V2-Tail-Light/Lv2/img36.jpg?auto=format,compress&w=559&q=90&dpr=2&updated_at=1584757351" width="200"/> |  $220  |   $289 - $336    |      31.8%      |     **82.0%**     |

<h6 align="right">The price data above is based on March 26, 2020 </h6>



## Table of Contents

* [Motivation](#motivation)
* [Data Source](#data-source)
* [Exploratory Data Analysis](#exploratory-data-analysis)
  * [Time Feature](#time-feature)
  * [Color Feature](#color-feature)
  * [Region Feature](#region-feature)
* [Modeling](#modeling)
  - [Feature Engineering](#feature-engineering)
  - [Model Selection](#model-selection)
  - [Result](#result)
* [Prediction Result](#prediction-result)
* [Limitation](#limitation)

## Motivation

<img src="/assets/images/red_yeezy.jpg" width="200" align = "right" />


The right is the famous Red Nike Yeezy. Its retail price is $250, and the latest resale price is $6,200, marked up by nearly 2400%. The high resale premium of this pair is not a single event. The once niche market of sneaker resale has grown to become a $2 billion market, and it is projected to reach $6 billion by 2025. Within the sneaker resale market, StockX is one of the largest platforms. The website operates like a stock exchange, where users can place a bidding or asking prices, and a deal is made whenever there’s a match. What is so valuable for us is that the platform offers transparent and actionable data. Using such data, we want to build a predictive model to identify undervalued sneakers, which resellers can invest in now and sell at higher price later.

## Data Source

We utilized public data offered through the [StockX Data Contest](https://stockx.com/news/the-2019-data-contest/), which consists of 99,956 transactions from 2017 to 2019. The dataset consists of two brands -- Yeezy and Nike Off-White, and over 50 different styles. 

To expand upon the features, we also manually obtained the colorway and number of sales from the StockX website. We then converted style and color into dummy variables, and sneaker size into frequency encoding as common shoe sizes are sold at higher premium. For modeling purposes, our target variable is price premium, which equals price markup over retail price, and our input variables are days since release, style, colorway, size, and number of sales.

Table: Initial Data Preprosessing

![data_source](/assets/images/data_source.png)

## Exploratory Data Analysis

Looking at price premium, we could easily observe that it is heavily skewed to the right. Most resale transactions were marked up between 0% to 500%, yet there are some extreme transactions whose markups were over 2000%. As such, we would first conduct anomaly detection to examine those points on the very high end and find common features among them.

<img src = "/assets/images/y_plot.png" width="300"/> <img src = "/assets/images/volinplot.png" width = "300"/>


### Time Feature

Statistically, the best time to resell is 3 to 5 weeks before the release date 0. The worst time to resell is the first 9 weeks after release, when the market is saturated. After that, as the availability in market declines, buyers are willing to pay higher premiums. 

<img src="/assets/images/alltime.png" width="240"/> <img src="/assets/images/allregion.png" width="240"/> <img src="/assets/images/allstyle.png" width="240"/> 

- **Time effect on Top 3 Nike Brands**

  The most sale Nike brands are Air Jordan, Presto and Zoom. Among these sneakers, Air Jordan has the highest premium even after long time. The common golden time period to have high price premium. are after day 200. White color has relative competitive premium against others.

<img src="/assets/images/timeaj.png" width="240"/> <img src="/assets/images/timepresto.png" width="240"/> <img src="/assets/images/timezoom.png" width="240"/> 

- **Time effect on Yeezy**

  It is worth notice that Different colors present different time pattern for Yeezy. Grey and Tan color have laggest 200 days after white and black color.

  ![timeyeezy](/assets/images/timeyeezy.png)

  The best time to sell in the secondary market is 3 to 5 weeks before the release date. The worst time to sell is the first 9 weeks after release, when the market is saturated. After that, as the availability in market declines, buyers are generally willing to pay higher premiums. 

### Color Feature

Our dataset consists of two major brands – yeezy and nike off-white. In terms of yeezy, we could see that basic colors including black, white, and grey have constant growth. Bolder colors like orange would start high but decline as time passes. In terms of off-white, red is the most popular color.

### Region Feature

Looking at the number of sales, California ranks the first and Oregon ranks the third. However, when we look at the percentage of population that purchases from StockX, Oregon comes to the top with nearly 2 transactions per 1000 people. Hence, StockX might want to make more promotions in Oregon, but keep in mind that as a reseller, we cannot control the sales region.

![edaregion](/assets/images/edaregion.png)

## Modeling 

### Feature Engineering

To recapture the features in our datasets, the target variable is the price premium, calculated as the percentage change of sale price over retail price, and input features are days since release, brand, region, colorway and number of sales, adding up to 31 variables.

### Model Selection

In our project, we tried two types of machine learning models, linear and tree-based regressions. We carefully accessed the performance of each model using 5-fold cross validation:

1. we used the same train-test split and the same seed for cross validation
2. we used the GridSearchCV package to implement all models
3. we used R2 as our evaluation metric

- **Linear Model - Lasso**

  According to our Exploratory Data Analysis, certain styles and colors (i.e. white) have over emphasizing power. In consideration of outliers and overfitting issues, we utilized the Lasso model to ensure regularization.
  <p align = "center"><img src = "/assets/images/lasso.svg" width = "280" align = "center"/></p>

  ```python
  lasso = Lasso()
  parameters = {'alpha': [1e-5,1e-4,1e-3,1e-2,1e-1,1e0,1e1,1e2,1e3,1e4,1e5]}
  r2 = make_scorer(r2_score, greater_is_better=True)
  clf = GridSearchCV(lasso, 
                    parameters, 
                    cv=5,
                    scoring=r2)
  ```

  To choose the optimal shrinkage parameter, we used cross validation, and the optimal shrinkage coefficient is ![Alt text](/assets/images/1e-6.svg?sanitize=true)

- **Linear Model - SVM**

  ```python
  x.train = preprocessing.scale(x_train)
  linearsvr = LinearSVR(tol=0.01)
  parameters = {'C': [0.01, 0.1, 1, 10, 100]} 
  r2 = make_scorer(r2_score, greater_is_better=True)
  clf = GridSearchCV(linearsvr, 
                     parameters, 
                     cv=5, 
                     scoring=r2)
  ```

- **Tree-based - XGBoost**

  XGBoost model comes in handy when dealing with nan value in datasets, in this project, we tuned parameters in XG Regression Tree to get prediction on our dataset. 
  
  ```python
  params = {'colsample_bytree': [i/10. for i in range(8,11)],
            'subsample': [i/10. for i in range(8,11)],
            'eta': [.3, .4, .5],
            'max_depth': list(range(3,6)),
            'min_child_weight': list(range(4,7)),
            'eval_metric': ['rmse'],
            'objective': ['reg:squarederror']}
  xg_reg = xgb.XGBRegressor()
  r2 = make_scorer(r2_score, greater_is_better=True)
  clf = GridSearchCV(xg_reg, 
                     params, 
                     cv=5, 
                   scoring=r2)
  ```
  
  In hyper-parameter selection, `max_depth` and `min_child_weight` is limited by the number of attributes, in our case, we give a range that have combination smaller than the variable count. The way of feature selection in XGRegression Tree is achieved by grid search `colsample_bytree` and `subsample`.
  
  <img src = "/assets/images/xgboost_tree.png" width = "580"/>

  Though we have limited `max_depth` and `min_child_weight` , the optimal parameters are 4 and 5, gettting up combination more than the number of variables. Therefore the model is prone for overfitting in this case.

#### Model Summary

|                   Model                    |                 r2 score                 | MSE                                      | Pros                                                         | Cons             |
| :----------------------------------------: | :--------------------------------------: | ---------------------------------------- | ------------------------------------------------------------ | ---------------- |
| <span style="color:blue">**Lasso** </span> | <span style="color:blue">**0.78**</span> | <span style="color:blue">**0.51**</span> | Easy to interpret, handle overfitting, handle multicollinearity | Weak in outliers |
|                    SVM                     |                   0.75                   | 0.39                                     | Memory efficient, fast, accurate in high dimensions          | Overfitting      |
|                  XGBoost                   |                   0.96                   | 0.04                                     | High `r_square`, clear tree structure                        | Overfitting      |

According to our modeling results, XGBoost has the highest R2. However, the XGBoost model is considered overfitted given that we have 31 features but less than 100,000 data points. The other two models, Lasso and SVM, are comparable in terms of predictive power. Further, the lasso model is more intrepretable as it allows us to easily examine constant effect of a predictor variables on price premiums. As such, taking both predictive power and model interpretability into account, we decided to proceed with the lasso model. Below is a summary of our fitted lasso model.

## Prediction Result

The Lasso coefficients are as above. Orange blocks represents negative coefficients, and blues represents positive coefficients. In terms of styles, AirJordan is the most lucrative, with an additional price premium of 324%. In terms of colors, red and tan will boost price premium, while blue seems to hurt resale prices. Further, as the number of sales increase, the price premium will go down. ![featureSig](/assets/images/featureSig.png)

According to our prediction model, sneakers of certain styles (i.e. AirJordan 1 and blazer) and of certain colors (i.e.red and tan) are the most investable. Based on this knowledge, we tried to find some undervalued sneakers, which we can invest in now  and sell higher later. We listed some proposed candidates below. 

The first pair listed is Air Jordan 1 by Travis Scott, released in May, 2019. If you are a sneakerhead, you probably already know that this is a popular pair. The question is, given the current resale price, could the price go up even higher. The answer is YES! This pair is selling at 500% markup as of early April, while our model predicts that the markup should have been 800%. As such, there is an arbitrage opportunity here, which would turn into a profit of over $500 on a single pair. The other candidates would follow similarly.

|                           Sneaker                            | Feature                                                      |                            Image                             |
| :----------------------------------------------------------: | :----------------------------------------------------------- | :----------------------------------------------------------: |
| [Air Jordan 1 Retro High Travis Scott](https://stockx.com/air-jordan-1-retro-high-travis-scott) | <font size="2"><b> Features: </b>air-jordan(3.24), Tan/Brown(1.83) <br/><b>Current Price Premium: 497.4% -> $875</b><br/><b>Predicted Price Premium: 802.0% -> $1400</b></font> | <img src="https://stockx-360.imgix.net/Air-Jordan-1-Retro-High-Travis-Scott/Images/Air-Jordan-1-Retro-High-Travis-Scott/Lv2/img01.jpg?auto=format,compress&w=559&q=90&dpr=2&updated_at=1550180948" width="150"/> |
| [Blazer Mid 77 Vintage Slam Jam](https://stockx.com/nike-blazer-mid-77-vintage-white-black) | <font size = "2"><b>Features: </b> blazer(0.58), White(0.71)<br><b>Current Price Premium: 381% -> $381</b> <br><b>Predicted Price Premium: 455.5% -> $455</b></font> | <img src="https://stockx-360.imgix.net/Nike-Blazer-Mid-77-Vintage-Slam-Jam/Images/Nike-Blazer-Mid-77-Vintage-Slam-Jam/Lv2/img01.jpg?auto=format,compress&w=559&q=90&dpr=2&updated_at=1554253378" width="150"/> |
| [Yeezy Boost 350 V2 Tail Light](https://stockx.com/adidas-yeezy-boost-350-v2-tail-light) | <font size = "2"><b>Features:</b> blazer(0.58), White(0.71)<br><b>Current Price Premium: 31.8% -> $70</b> <br><b>Predicted Price Premium: 455.5% -> $180</b></font> | <img src="https://stockx-360.imgix.net/adidas-Yeezy-Boost-350-V2-Tail-Light/Images/adidas-Yeezy-Boost-350-V2-Tail-Light/Lv2/img36.jpg?auto=format,compress&w=559&q=90&dpr=2&updated_at=1584757351" width="150"/> |

<h6 align="right"><font size="2">The price data above is based on March 26, 2020
</font></h6>

## Limitation

**1. Time Limitation** The data we utilized only cover sales from 2017 to 2019. Due to the changing tendency of the sneaker resale market, the model’s predictive power might be less stable in far futures.

**2. Brand Limitation** Since our data only covers two major brands -- Yeezy and Nike Off-White, it might not be appropriate to generalize our predictive model to other brands of sneakers.


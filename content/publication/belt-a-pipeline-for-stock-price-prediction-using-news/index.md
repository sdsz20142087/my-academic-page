---
title: "BELT: A Pipeline for Stock Price Prediction Using News"
publication_types:
  - "1"
authors:
  - Yingzhe Dong
  - Da Yan
  - Abdullateef Ibrahim Almudaifer
  - Sibo Yan
  - Zhe Jiang
  - Yang Zhou
doi: 10.1109/BigData50022.2020.9378345
publication_short: 2020 IEEE International Conference on Big Data (Big Data)
abstract: >-
  Abstract—Stock investment is a vehicle for many people to grow their wealth.
  However, market downturns can cause huge losses and need to be predicted for a
  timely sell. In fact, with effective prediction, stocks are a good investment
  even during periods of market volatility as many stocks are “on sale”.

  News is an important source of signal for stock price move- ment. However, stock analysts usually adjust their analysis according to the news in a subject manner, and wrong judgments can cause investors huge losses.

  Twitter is a great source for breaking news, and provides a timely stream of signals on stock trends. News on Twitter also tends to have a great impact on the market due to the large number of Twitter users. This paper proposes a data-driven pipeline to timely incorporate Twitter news about a company into a time series prediction model on the company’s stock price. Our approach, called BERT-LSTM (BELT), extracts informative features on stock price direction from Twitter news using the state-of-the-art natural language processing (NLP) model BERT, which are then used as covariates to a many-to-many stacked LSTM model that also utilizes historical stock prices to predict the direction of future stock price. Utilizing a carefully curated stock news dataset, we fine-tune BERT to effectively identify those news tweets that are relevant, and to extract NLP features that are indicative of price rises and falls. All model parameters are trained end-to-end to provide a data-driven and objective pipeline to incorporate news signals so as to avoid subjective analysis. Extensive experiments on real stock prices and Twitter news show that BELT is able to predict stock prices more accurately utilizing news information than if historical price data are used alone for prediction, and beats StockNet which is the current state of the art for news-based stock movement prediction.

  Index Terms—stock, news, prediction, BERT, LSTM
draft: false
featured: true
slides: belt-a-pipeline-for-stock-price-prediction-using-news
image:
  filename: featured.jpg
  focal_point: Smart
  preview_only: false
date: 2021-04-28T02:04:34.675Z
---

---
date: 2023-03-03T10:58:08-04:00
description: "The case of Australia, Chile and Peru's Central Banks"
featured_image: "/images/ds/central_banks/sentiment.png"
tags: ["Random Forest", "Monetary Policy"]
title: "Sentiment Analysis for Monetary Policy Predictions"
author:
  - Vicente Lisboa
  - Margherita Phillip
  - Renato Vassallo
slug: sentimentanalysis
showtoc: true
draft: false
---

This project highlights the scope for using text data from central bank statements to improve predictions about policy rates. The problem is constructed as a classification into three potential outcomes: the rate in the next period goes up, goes down, or stays the same. We test our approach with data from the central banks of **Australia**, **Chile** and **Peru**.

# Main findings

+ We find that a **Random Forest Classifier** trained only on economic data (e.g. inflation, growth) performs less well than when we also train it on sentiment scores for the bank statements.
+ Incorporating **sentiment indicators** significantly improves the prediction of ‘hold’ and ‘lower’ events in the next period compared to a baseline model.
+ However, in terms of **feature importance**, macroeconomic variables continue to be the most relevant.
+ These results shed light on the suitability of incorporating sentiment indicators to enrich and improve existing
analytical models.

# Data processing
We worked with two sets of data for each of the three countries: economic indicator data and text data from the statements issued by the central banks. The text data was scraped from the banks' websites (using a combination of the `Selenium` and `BeautifulSoup` libraries) and subjected to three different approaches of processing:

1. **General (positive vs negative) dictionary, LM_tone:** using Loughran and McDonald’s (2021) Sentiment Word List to measure the net sentiment of each statement.
2. **Domain-specific (hawkish vs dovish) vocabulary, GT_tone:** by aggregating sentences scores after classifying into hawkish and dovish considering positive and negative modifiers.
3. **Tf-idf - cosine similarity:** computing the _term frequency-inverse document frequency_ and calculating the cosine similarity between two consecutive statements.

{{< figure src="/images/ds/central_banks/sentiment.png" title="Peru: sentiment indicator using general dictionary" >}}



# Machine Learning Model

To predict the next monetary policy decision (_target: next_decision_), we fit two models. The **baseline model** only has economic features as its input (previous decision, current inflation, inflation expectations, unemployment, GDP growth), while the **augmented model** received those same features alongside the sentiment indicators.

As an estimation strategy, we opted for a methodology that is flexible and captures possible non-linear relationships between the variables. A test run of different machine learning models, showed that the **Random Forest** was at least as good as other options; and it was essential for the project’s objective to determine the importance of different features.

To optimize performance, we defined a grid of hyper-parameter ranges, performing a _StratifiedKFold_ for the cross validation. We ensured that the maximum depth was capped in order to prevent over-fitting. After fitting the model with the optimal set of parameters, we computed the predictions in the corresponding testing set (randomly generated).

# Results

In order to visualize the performance of the models, we generated a confusion matrix where each row represents the actual category of our target (raise, hold or lower), while each column represents the instances in a predicted class.

## Baseline model

The matrix located at the top corresponds to the training set while the lower matrix corresponds to the out-of-sample predictions. From to the results of the **baseline model** we conclude that 0.681 of the decisions were predicted correctly. This corresponds to the sum of the diagonal. The results also suggests that the feature with the most influence in the prediction on the next policy decision is the bank’s decision (to lower, hold or raise) in the current period. The GDP difference to last year is the second most important feature.

{{< figure src="/images/ds/central_banks/baseline.PNG" title="Results for the baseline model">}}

## Augmented model

Next, we went on to to evaluate the gains (in terms of predictive power) of adding latent textual features derived from monetary policy statements to the baseline model that only considers traditional macroeconomic variables.

By summing the diagonal of the confusion matrix for the **augmented model**, we can see that 0.823 of the decisions were predicted correctly. If we compare these results with those obtained in the baseline model, we can see an improvement of 0.142 more of the target values correctly classified. Also, by looking at the confusion matrix, the model is more accurate in predicting ‘hold’ and ‘lower’ events. Therefore, we have some evidence that adding sentiment indicators improves the predictive power of the model.

{{< figure src="/images/ds/central_banks/augmented.PNG" title="Results for the augmented model" >}}

None of the sentiment indicators are able to beat the economic indicators in terms of importance for the model, but there are clear gains in predictive capacity when including textual features. Among the sentiment indicators, **tone_GT** is the one with the highest relevance, followed by tone_LM and the cosine similarity vectors.

# Full text article and codes

Full Text: [Read article](/pdfs/sentiment_analysis_central_banks.pdf)

GitHub Repository: [Codes](https://github.com/RenatoVassallo/sentiment_analysis_central_banks.git)

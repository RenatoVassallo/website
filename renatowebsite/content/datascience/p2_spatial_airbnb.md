---
date: 2023-02-15T10:58:08-04:00
description: "Application using Barcelona data for 2022"
featured_image: "/images/ds/spatial_airbnb/map2.png"
tags: ["Airbnb", "Spatial Analysis"]
title: "Spatial interpolation of apartment prices using Airbnb web scraped dataset"
author: Renato Vassallo
slug: spatialanalysis
showtoc: true
draft: false
---

The purpose of this article is to to compare the results of several **interpolation methods for spatial data** of Airbnb apartment prices in Barcelona. Applying text mining techniques, an Airbnb web scraped dataset from 2022 is developed, and different interpolation methods are applied, including Voronoi approach, Nearest Neighbors (NN), Inverse Distance Weighting (IDW), and ensemble methods.

# Main findings

+ Based on the RMSE value, the **NN interpolation method** with n = 5 yields much more accurate prediction values than the Voronoi or IDW methods.
+ Areas with high Airbnb prices are located around L’Eixample, El Poblenou and Les Corts, while areas with low prices are in the vicinity of Porta and Sant Andreu neighborhoods.
+ These results could be associated with the high student and business demand for accommodation near the city center.

# Data processing

We work with a database obtained from [Inside Airbnb](https://www.insideairbnb.com), a website on which web scraped datasets of “snapshots” of cities are published. We decided to work with the files of Barcelona of the situation on 2022.
The initial data has 15778 observations for 75 features, among which there are numerical and text data. The pre-processing of this initial data includes: data cleaning, remove outliers, inputing reasonable values for missing data and extraction of textual features using NLP techniques.

The following Figure shows that the price distribution is slightly skewed to the right.To have a point of comparison, we’ve created an _is_top_100_ dummy that will assign a value of 1 if the listing is in the top 100 reviewed listings on Airbnb. The right panel shows that, on average, those top 100 ads have a higher price than the others (approximately 90 vs. 95 euros on average).

{{< figure src="/images/ds/spatial_airbnb/stat1.PNG" title="Descriptive statistics" width="100%">}}

# Geospatial data

From **Inside Airbnb** web page, we obtain a GEOJSON file that contains full list of Barcelona neighborhoods with geospatial data that we will use to visualise information on the map. We use **Leaflet** R package and display listings from both groups using lat/long information coming from listing details dataset. The next Figure give us an idea of geographical distribution with Red points being in Top 100 most popular listings.

{{< figure src="/images/ds/spatial_airbnb/map3.png" title="Neighborhoods geospatial data" >}}

# Results
Our goal is to make predictions of price per square meter of Airbnb listings continuously in space within the boundary of Barcelona. We decide to predict at locations forming a fine grid within Barcelona.

For the interpolation of the values of the variable of interest, we consider 4 classical methods: (i) Voronoi polygon; (ii) Nearest Neighbors; (iii) Inverse Distance Weighting (IDW); and, (iv) Ensemble method.

{{< figure src="/images/ds/spatial_airbnb/voronoi.PNG" title="Voronoi method" >}}

Cross validation is used to see how well the model works by sampling it a defined number of times (folds) to pull out sets of training and testing samples, and then comparing the model’s predictions with the (unused) testing data.

Based on the RMSE value, the Nearest Neighbors Interpolation Method with n = 5 yields much more accurate prediction values than the Voronoi or IDW Interpolation Methods.

{{< figure src="/images/ds/spatial_airbnb/map_nn.png" title="KNN method" >}}


# Full text article and codes

Full Text: [Read article](/pdfs/spatial_airbnb.pdf)

GitHub Repository: [Codes](https://github.com/RenatoVassallo/spatial_airbnb.git)

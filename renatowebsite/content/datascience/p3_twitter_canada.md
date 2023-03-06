---
date: 2023-02-12T10:58:08-04:00
description: "An Analysis for Canada"
featured_image: "/images/ds/twitter_canada/cover.jpg"
tags: ["Twitter", "COVID-19"]
title: "Tracking Twitter Conversations on the COVID-19 Pandemic"
author:
  - Pearl Herrero
  - Nicole Poynarova
  - Luis Quiñones
  - Renato Vassallo
slug: twitter_canada
showtoc: true
draft: false
---

The objective of this Technical Note is to understand how the **conversation on Twitter** about COVID-19 evolved during the beginning of the pandemic. In particular, we are interested in tracking only Canadian medical doctor users. To do this, the first step was to collect a large-scale dataset of COVID-19-specific geotagged tweets using the `Twarc` library, then filter and clean the tweets to obtain some descriptive statistics that could serve as input for further research.

# Gathering tweets using twarc2

We proceed with the following steps:

1. **Streaming:** the first step was to obtain a list of user names and user ids for Medical Doctors. We implement this with the stream endpoint, making use of the bio description of the users.

2. **Getting followers:** from the smaller side. Given we have 117 user’s we will get the follower’s or following from, the time estimate is around 1 hour and 57 minutes. Based on the rate limit of 15 requests per 15 minutes.

3. **Filtering:** we filter, among the followers, those that are Medical Doctors using the same text matching criteria that we used at the beginning.

4. **Users activity:** the next step is to obtain all the users’ activity around the period spanning 2019-10-01 to 2020-10-01 (included). For this, we use the command `twarc2 timelines`.

# Descriptive statistics

In particular we are interested on analyzing the _top 10 hashtags_, _mentions_ and _emojis_. Then we are going to plot the timeline evolution for the total number of retweets and the evolution of tweets that contain any word related to the pandemic.

From users activity data around the period spanning 2019-10-01 to 2020-10-01, we extract relevant English-language tweets and filter for information on incidence of mentions, hashtags, and emojis. The next Figure presents the ranking of the 10 most used hashtags, mentions and emojis.

{{< figure src="/images/ds/twitter_canada/top10.PNG" title="Top 10 statistics" >}}

# Evolution of COVID-19 indicator

We construct a pandemic indicator using the following list of keywords: pandemic, covid, asymptomatic, coronavirus, incubation, pathogen, distancing, quarantine, isolation, antibody, outbreak, epidemic, mask, immunity, and respirator. The indicator, shown in Figure 3, is consistent with the dynamics of COVID-19: a violent increase is observed at the beginning of March 2020 and then gradually falls, showing a certain rebound towards the end of the year (as a consequence of the second wave of infections). A certain erratic behavior of the series is observed, which would be associated with a seasonal
weekend component.

{{< figure src="/images/ds/twitter_canada/covid.png" title="Pandemic indicator using words related to COVID-19" >}}

# Full text article and codes

Full Text: [Read article](/pdfs/twitter_canada_fv.pdf)

GitHub Repository: [Codes](https://github.com/RenatoVassallo/twitter_analysis_canada.git)

---
layout: post
date: 2017-04-17
title: Design an A/B Test
slug: ab-test
description: Designed an A/B test, including which metrics to measure and how long the test should be run. I also analyzed the results of an A/B test that was run by Udacity, recommended a decision, and proposed a follow-up experiment.
categories: ab-test
tags: ab-test confidence-intervals binomial-distribution hypothesis-testing variability-of-different-metrics sign-test
---

## Experiment Design

This project was made as part of Udacity's Data Analyst Nanodegree.

The project instructions can be found here: https://docs.google.com/document/u/1/d/1aCquhIqsUApgsxQ8-SQBAigFDcfWVVohLEXcV6jWbdI/pub?embedded=True

### Metric Choice
#### Invariant Metrics: 
- Number of Cookies; 
- Number of Clicks;
- Click-through Probability

Visiting the course overview page or clicking on the “start free trial” button happen before the free trial screener is triggered, so they must behave equally on control and experiment groups.
 
#### Evaluation Metrics:
- Gross Conversion: enrollments / clicks should be a good evaluation metric. It measures if the proposed change is really discouraging users who inform less than 5 hours of study per week from enrolling. This metric is expected to decrease significantly in order to launch the experiment.
- Net Conversion: payments / clicks should also be a good evaluation metric. It measures if the free trial screener is changing the proportion of students who remain enrolled past the 14-day boundary after starting a free trial. This metric is expected not to decrease significantly in order to launch the experiment, since the students who complete payments usually dedicate 5 or more hours per week to studying.
 
#### Unused Metrics:
- Number of user-ids: the number of enrollments could potentially be used as an evaluation metric, but since we have the gross conversion, it would be redundant, and also, comparing raw numbers of user-ids assumes control and experiment groups are equally sized, which is not always true.
- Retention: payments / enrollments would be a perfect metric for this experiment, except for the experiment size needed to make a powerful test. At least 17 weeks would be needed in order to complete the experiment, which is too much. This metric would be expected to show a significant increase in order to launch the experiment.
 
### Measuring Standard Deviation
- Gross Conversion: 0.0202
- Net Conversion: 0.0156

In both cases, the empirical and analytical variabilities are expected to be comparable, because the unit of diversion (cookies) and the unit of analysis (cookies) are the same.
 
### Sizing
#### Number of Samples vs. Power
I’m not using Bonferroni correction, since the metrics are correlated, and also because I need a specific combination of results of all metrics in order to recommend a change, so it would be too conservative. 
Number of Pageviews: 679300.
 
#### Duration vs. Exposure
I would divert 100% of the traffic, which would lead to 17 days.
The experiment introduces a popup on the site, which is one more step in the way of enrolling in a free trial. This change does not present physical, psychological, emotional, social or economic risks above minimal risk.
If a student enrolls in the free trial, his data becomes personally identifiable, so there has to be an agreement on privacy policies towards the data, even though the collected information is not sensitive and does not involve political attitudes, financial or health data, for example.
Based on the factors cited above, I chose to divert all traffic.

## Experiment Analysis
### Sanity Checks
Number of Cookies: CI = (0.4988, 0.5012), Observed = 0.5006, pass

Number of Clicks on “Start free trial”: CI = (0.4959, 0.5041), Observed = 0.5005, pass

Click-through-probability on “Start free trial”: CI = (-0.0013, 0.0013), Observed = 0.0001, pass
 
### Result Analysis
#### Effect Size Tests
Gross Conversion: CI = (-0.0291, -0.0120), dmin = 0.0100, statistically and practically significant.

Net Conversion: CI = (-0.0116, 0.0019), dmin = 0.0075, not statistically nor practically significant.
 
#### Sign Tests
Gross Conversion: p-value = 0.0026, statistically significant.

Net Conversion: p-value = 0.6776, not statistically significant.
 
#### Summary
The Bonferroni correction is designed to reduce risks of false positives when, in a set of tests, the launch of an experiment is conditioned to any of them matching my expectations. In my case, I need all of my tests to match my expectations in order to launch the experiment, so I’m not using the Bonferroni correction.
There were no discrepancies between the effect size and the sign tests.
 
### Recommendation
The encountered results for the evaluation metrics were:

- Gross Conversion: in order to launch the experiment, this metric should present a statistical and practical decrease, which were the results found on the tests.
- Net Conversion: the confidence interval found for the effect size on net conversion includes the negative practical significance threshold. It means that there is a chance, for an alpha of 0.05, that this metric presented a practically significant decrease. It would be possible to repeat the experiment with more power, but it is unlikely that this trend would change.
Since I need both metrics to match my expectations and I cannot conclude that the net conversion has not decreased, my recommendation is not to launch the experiment.
 
## Follow-Up Experiment
 
One follow-up experiment that could reduce early cancellations could be the following: when a student clicked on the “start free trial” button, a message would appear informing him that the course usually requires 5 hours of dedication per week or more, and he would be requested to mark the hours he would commit to the course on his agenda or calendar in order to proceed. There is going to be a checkbox saying “I have reserved the hours I will commit to the course”, and a “next” button that would be disabled until the checkbox was checked. The next button would then proceed to the usual checkout process.
This may seem similar to the attempted experiment, but it has an important difference: it does not suggest students to try the free course materials instead of engaging on the free trial. Maybe this fact could make a significant difference on the observed effects.
The hypothesis is that this new change might cause some students, who would otherwise not do so, to organize themselves and reserve some hours per week to study. This would, thus, reduce the number of students who abandon the free trial without significantly reducing the number of students who eventually complete the course.
The metrics would be gross and net conversions. They measure respectively the number of enrollments and the number of payments per click on the “start free trial” button. Combined, they can express if the hypothesis holds true. Also, as calculated on the attempted experiment, they are feasible in terms of experiment size.
The unit of diversion would be cookie, and the invariant metrics could be the number of course pageviews, and the number of clicks on “start free trial”.

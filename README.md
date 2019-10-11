# DSdiary Chapter 1 - common interview questions




## 4-step framework for metric change (diagnostic questions, before A/B test):

* External factors: big events? Seasonality? Competitor launch new feature? Industry trend?
* Internal factors: data error? System issue? New feature launch? Marketing events?
* Metric &  product segments: split the metric/product into segments, how do these segments change? 
  -	Metric segments, e.g. metric = A + b + C + …
  -	Product segments, e.g. # likes = # likes in posts + # likes in pages + … 
*	User segments: platform, device, version, browser, location, country, language


## 5-step framework for metric not significant (or weird result) after A/B test

* Sanity check of invariant metrics or sample size, make sure the experiment set-up is correct
* sample size not large enough
  - check how test p-values and also target metric change over time. 
    - If the variance is very large, that means the sample size is not enough. If the variance is small, it means we have enough sample size.
* test running time not long enough
  - if there is a increasing/decreasing pattern for metric, and we are only running less than 1 week or 2 weeks, we could wait longer to see a plateau or more steady stage
* selection bias: test/control people are not random assigned
  - we could build a RF model, using user behaviour variables and demographic variables to predict whether this user is in test group or in control group. If there is some variables that show up split ability, that means randomization is not correclty set up for these variables. e.g. 90% of the US users are in test group, 10% of the US users are in control group, if US users have some weird performance, this will affect total test result.
  - break results into different segments to find outliers
    - Simpson’s paradox: which could result from either incorrect setup, or user segments perform differently
*	Use non-parametric methods to test


## 2-step framework for metric not significant/weird after rolling out to all users:
* Learning effect: change aversion or novelty effect, need more time get plateau state
*	Seasonality: could add a hold-out group not launching new feature to compare to control



## Deal with multiple metrics in AB test:
* Do multiple comparison
  - FWER, like Bonferroni correction
    - family-wise error rate (FWER) is the probability of making one or more false discoveries, or type I errors when performing multiple hypotheses tests
    - Bonferroni alpha threshold: alpha/n, where n=#tests
  - FDR correction like BH process
    - False Discovery Rate controls the expected proportion of "discoveries" (rejected null hypotheses) that are false (incorrect rejections)
    - adjusted p-value = p-value * n/r, where n=#tests, r is the rank of this p-value in ascending ordered p-values
  - Bootstrap 
  
*	Multiple metrics not moving to same direction
  - Learning effect or Short term effect, what would be long term result?
  - ROI or value of the new feature?
*	Use OEC: overall evaluation criterion





## Product health diagnosis/measurement

* Growth
* Retention
* Stickiness
* Engagement


###	Growth: KPI, directly related to revenue and monetization
* DAU/WAU/MAU: choose which depends on expected user action
*	D/D, W/W, M/M, Y/Y changes: e.g. 28-day rolling window difference
*	Quick ratio: (#new user + #resurrected user) / (# churned user), between time points T1 and T2. The numbers are on T2, comparing to T1. 
-	Growth = new users + resurrected users + retained users – churned users
-	Because delta(growth) = new users + resurrected users – churned users. 

###	Retention: best indicator of product value and product-market fit, most important lever for growth. Get people to come back.
*	Dn/Wn/Mn retention rate: a short-term proxy for long term retention rate

###	Stickiness: get people to come back on their own volition, so to reduce dependency on tactics such as push notifications
*	DAU/MAU: the percentage of the users who are active on a monthly basis who also are also active on a daily basis

*	Lness:
-	L5+/7: percentage of the people who at least visit 5 times per week
-	L21+/28: ……21 times….per month

*	Sticky retention: DoD/WoW retention
-	E.g. WoW retention is week-over-week retention, the ratio of the users who are active last week that are also active this week

###	Engagement: good sign for product-market fit, the most important driver of retention
*	Time spent / DAU
*	#sessions
*	Time spent/session
*	Content consumption, e.g. #views
*	Content production, e.g. #new posts
*	Content feedback, e.g. #likes/#comments




## How to improve product?

The question is not asking you to be visionary. But to check if you can find things from datasets as a data scientist. Always try to incentivize “good” and dis-incentivize “bad”.

1. Firstly, define the target. Say engagement (in order to move long-term retention and revenue)
2. Then choose metric used to evaluate engagement: i.e. total_time_spent / DAU
3. Pick variables that would move the metric: use both user demographic characteristics and user behavior variables
4. Use model (RF is good here) to check the relationship between each variable and engagement. Come up with several scenarios to explain and make suggestions based on the results (improve which segment)

As you can see, conclusions usually end up being about:
1. tell marketing to get more of the good performing user segments
2. tell product to fix the experience for the bad performing ones


## How to use ML in product improvement

1. Build regression/classification model using models like RF
  - regression: outcome could be continuous variables like #sessions/day
  - classification: outcomes like conversion (yes or no), click(yes or no)
2. two-fold usage of ML model
  * Get insights from model output and use the insights to improve product or find fraud patterns
    - First, use variable importance to get important variables
    - Second, use PDP (partial dependence plot) to understand how these variable affect target outcome
      - e.g. we find young users (age < 30) have positive correlation with outcome 
      - e.g. US performance is good. UK performance is not good. 
    - Work with product team to figure out why some segments don't perform well, and fix the problem.
    - Work with market team to boost the performance of segments that do perform well.
  * Use the model directly for new data prediction
    - Fraud detection
      - e.g. design a fraud/spam prediction pipeline. If the predicted probability is > 0.9, we will assume it's fraud and put this session on hold and send for manual check. If 0.3 < p < 0.9, we ask for additional verification steps like phone number verification via SMS. If p < 0.3, we assume it's normal user. 
    - User value prediction (for prioritizing new users)



## Fraud detection

People who commit fraud would like to repeat it if not being caught. 

The goal of fraud detection:
1. Find the fraud patterns, and identify existing fraud
2. Build prediction model, deal with potential fraud

The general idea is to build a ML model using existing labeled dataset, and use model output insights for goal 1, and use model itself for goal 2.

Before jumping into building a model, think about whether you can create new powerful variables. This is
called feature engineering and it is the most important step in machine learning. However, feature
engineering is quite time consuming. In a take-home you should just give an idea of how you would do it
and emphasize that with more time you would go deeper into it.
A few obvious variables that can be created here could be:
* Time difference between sign-up time and purchase time
* If the device id is unique or certain users are sharing the same device (many different user ids using
the same device could be an indicator of fake accounts)
* Same for the ip address. Many different users having the same ip address could be an indicator of
fake accounts
* Usual week of the year and day of the week from time variables


# r/CheckUp: Forecasting user engagement and community health on Reddit

User engagement is one of the most important metrics for determining the overall health of a website. Online forums like Reddit.com are particularly interesting because the content is generated entirely by its users. There are multiple types of Reddit users; ranging from people who post/comment as frequently as everyday to the users who lurk and never comment/post. In January 2013, 46% of the posts and 25% of the comments were produced by engaged users.

<b> Engaged users </b> are the users who either post or comment on five or more days in a given month.

The health of a subreddit (specific forum on Reddit) is dependent on the amount of content being produced. By predicting growth or decline before it happens, Reddit can intervene and implement changes to alter the trajectory of a subreddit. Additionally, subbreddits can be classified by predicted growth to investigate the characteristics of each type of growth.

# Project

The goal of this project is to build a predictive model to forecast how the number of engaged users who are active in a specific subreddit will change.
Motivating the Definition of Engaged Users

The graph below shows the number of users as a function of the minimum number of days that they either post or comment in January 2013. For a given X, the number of users includes all users that produce content on X or more days. This is represented in the plot by the shaded orange area under the curve. The horizontal grey line represents 10% of the number of users who post or comment on one day.

![](https://github.com/tharrington923/rCheckUp/blob/master/website/rcheckup/static/images/engaged_users.png)

# Reddit Data

The Reddit data consists of entries for every single comment and post. Each post/comment contains a timestamp, the title, body, author, subreddit, score, and many other fields. The data is available on Google BigQuery from present back to 2006. However, I focus on the 2013-2014 data in this project.

Using SQL and BigQuery, monthly time series were constructed for each subreddit for the number of engaged users, the total number of characters in comments/posts, the total score (aggregate of up and down votes on comments/posts), and the number of moderators.

![](https://github.com/tharrington923/rCheckUp/blob/master/website/rcheckup/static/images/data.png)

# ARMA Model

To start, an autoregressive (AR) moving average (MA) model was fit to the number of engaged users time series for a specific subreddit. The fit was performed on the February 2013-August 2014 data. Using the fit, the number of engaged users was forecasted for September, October, and November.

![](https://github.com/tharrington923/rCheckUp/blob/master/website/rcheckup/static/images/arma.png)

# ARMAX Model

To improve upon the ARMA model, three exogenous subreddit features were encorporated into an ARMAX model. The exogenous features consisted of time series for the subreddit for the total number of characters in comments/posts, the total score (aggregate of up and down votes on comments/posts), and the number of moderators. The same fit period was used.

![](https://github.com/tharrington923/rCheckUp/blob/master/website/rcheckup/static/images/armax.png)

# Quantifying Growth

To classify the subreddit as growing, shrinking, or constant, the average value of the values for July and August was computed to form the baseline. Then, the standard deviation for the time series (2/2013-8/2014) was computed. If the value of the predicted number of engaged users in November fluctuated more than one standard deviation from the baseline, the prediction was classified as growing or shrinking depending on the direction. If not, then the subreddit was classified as not changing.

![](https://github.com/tharrington923/rCheckUp/blob/master/website/rcheckup/static/images/quantify_growth.png)

# Results

The plot below shows the time series and ARMAX fit for the hockey subreddit. The shaded blue region represents the region being predicted. The ARMAX models performed significantly better than the ARMA models. The ARMA models obtained around 40% prediction accuracy across the 161 subreddits investigated, while the ARMAX obtained over 60% accuracy. Inclusion of more exogenous features is likely to yield even more accurate models, but could not be explored due to the time constraints of this project.

![](https://github.com/tharrington923/rCheckUp/blob/master/website/rcheckup/static/images/results.png)

The confusion matrix below shows how each of the 161 subreddit ARMAX models performed in comparison to the true trend of the subreddits. While many of the subreddits were classified as not changing, it is important to minimize the number of subreddits incorrectly predicted in the top right and bottom left. The large number captured as not changing is the direct result of how growth was classified. It would be bad for a subreddit to be predicted to grow and then shrink.

![](https://github.com/tharrington923/rCheckUp/blob/master/website/rcheckup/static/images/cmatrix.png)

---
title: "Home Run Modeling"
date: 2024-02-28
draft: false
tags: ["sports", "baseball", "cs"]
---

![Header image](/images/baseball_modeling/header_image.png#center)

## Why home runs?

Some of the best moments in baseball games are home runs.
Something about hitting the ball out of the park is satisfying.
Since baseball season just started, I wanted to model a part of the game.
I decided to model home runs since they are pretty rare events but should still be able to be accurately predicted.
When I say accurately predicted, I mean that we can accurately predict the probability of a player hitting a home run.
At the end of the day, we can only compare to the true data...but we have many years of baseball data (baseball is nice since there are very detailed statistics and metrics for each player for 100+ years).

We are going to model the probability that a player hits a home run in a game.
There are only two outcomes: a player hits a home run in the game or they don't.
We will use the past couple years of MLB data to build the model, and if we need more data in the future it is easy to add.

There are many challenges that we'll have to overcome in order to build a good model.
First, even though modeling the probability of a player hitting a HR in a game seems relatively simple, there are lots of variables that can affect this probability:

- Batter skill
- Which pitcher(s) the batter goes against
- Batter skill against different pitchers
- How many times a batter bats per game
- Stadium (different stadiums have different geometry. And location matters: at Coors Field in Denver, balls travel further due to the less dense air.)
- Weather
- Probably many other factors

I think the most important variables to consider are batter skill, pitcher skill, stadium, and weather.
We will start by building a simple model and then add more feature variables to improve the model.
We can also iterate on the model itself.

## Data data data

In order to build a good model, we need good data.
Luckily, [baseball-reference.com](https://www.baseball-reference.com/) has detailed data from decades of baseball history.
For example, we can see that for a given [random game](https://www.baseball-reference.com/boxes/CHN/CHN202306130.shtml), the website lists which players got home runs.

In order to think of the data we need, it is helpful to me to think of the interface that we want for our data in order to do the analysis and make it extendable in the future.

### Game

```
class Game:
    id -> int
    time -> timestamp
    location -> str
    home_team -> str
    away_team -> str
    home_team_lineup -> list of Players
    away_team_lineup -> list of Players
```

### Player

```
class Player:
    _id -> int
    name -> str
    pitcher_or_hitter -> bool
    team_name -> str
    stats -> dict
        Batting average -> int
        On base % -> int
        ...

    functions:
        get_stats_before_game( game_id ) -> dict
        did_player_hit_homerun( game_id ) -> bool
```

### Training data

To build the training data, we can do:
```
X, y = [], []
for each game in training data:
    for each player in game:
        X.append( player.get_stats_before_game( game._id ) )
        y.append( player.did_player_hit_homerun( game._id ) )
```

Then, we can train any binary classification model.


### Graphs

After downloading the data for 2022-2023, we will explore it with some graphs and preliminary data analysis.
Let's explore some of the data we have.
We will look at data from the 2022 and 2023 regular season.
One thing we have to consider is that stats become more consistent as more games are played.
For example, if a player has a bad first game they could have a batting average of zero, which will then level out to a more consistent level as more games are played.
We can look at the data to see when stats level out.
Then, when we train and test our model we can disregard these games.
Here are a few stats for Matt Olson, the 2013 [batting average leader](https://www.espn.com/mlb/history/season/_/year/2013).

![Matt Olson stats](/images/baseball_modeling/matt_olson_stats.png#center)
_Matt Olson season stats_

We can see from this graph that these stats tend to even out after 10-20 games.
Let's also look at batting average for the entire league after a certain number of games:

![League batting average](/images/baseball_modeling/all_players_batting_average.png#center)
_League batting average over a number of games after first game played_

We can see that from the top graph it seems that after 20 games, there is a lot less variation in batting average.
From the standard deviation in the bottom graph, it also seems that standard deviation levels off after around 20 games.
Therefore, we will require at least 20 previous games for a player to train and predict whether they got a home run.


### Data analysis

We can analyze our 2022/2023 data and see how whether a player hit home runs correlates to our stats.
For each game in the 2022 and 2023 regular seasons, we aggregate the stats of each player _before_ the game and whether they hit a home run in that game.

| Stat | Mean when player did not hit home run | Mean when player hit home run | Percent difference |
| -------- | :-------: | :-------: | :-------: |
| Batting average | 0.246 | 0.249 | +1.27% |
| On base % | 0.317 | 0.322 | +1.88% |
| Slugging % | 0.401 | 0.429 | +6.87% |
| RBIs per AB | 0.125 | 0.138 | +10.21% |
| Home runs per AB | 0.033 | 0.041 | +24.60% |
| At Bats Per Game | 3.216 | 3.369.041 | +4.74% |

The stats that change the most are RBIs per AB and home runs per AB.
Home runs per AB is the stat that has the most difference between players that hit home runs in a game and those that did not.
This makes sense that home runs per AB is a probably a good predictor of if a player will hit a home run in a game.

We can see that batting average has only a 1% increase in players that hit a home run in a game compared to players that did not.
In contrast, slugging percent increased by 7% between players that hit home runs in a game and those that did not.
This makes sense because slugging more directly indicates home run power than batting average.
Also players who hit lots of home runs often strike out a lot.


## Building the model

We will build a binary classification model with logistic regression to predict home run probability per game.

### Features

We will first build a base model that we can later improve on.
For our base model, we only use data from the batter (i.e. batting average, slugging, etc.).
Later, we will add data about the starting pitcher and other data.
We gather the following stats for each player before each game and use these as our features for the model:
- Batting average
- Slugging %
- On base %
- Home runs per AB
- RBIs per AB
- Average ABs per game

The last feature, average ABs per game, is important because we are modeling home runs per _game_ not _AB_.
Therefore, a pinch hitter is less likely to hit a home run in a game than a starter since they have less ABs total.


### Train/test/split

We divide our data in train/val/test with proportions of 70% / 10% / 20%.
Overall, we have 60k training examples, 8.5k validation examples, and 17k test examples.

### Logistic regression

We will use a logistic regression model to do binary classification on whether a player will hit a home run.
Logistic regression is good for a base model to do binary classification.
To formalize our problem, we assume that we have the \\(i\text{th}\\) data pair \\(\left(x_i\in\mathbb{R}^m, y_i\in\\{0,1\\}\right)\\).
\\(x_i\\) is the feature vector and \\(y_i\\) is whether a player hit a home run (1=true and 0=false).
For logistic regression, we have our hypothesis function
$$h_\theta(x_i)=\frac{1}{1-e^{-\theta^{\ T}x_i}},$$
This is the logistic function.
We are modeling probability \\(p_i\\),
$$p_i=P(y_i=1|x_i;\theta)=h_\theta(x_i)$$
$$1-p_i=P(y_i=0|x_i;\theta)=1-h_\theta(x_i).$$
We see that the likelihood function \\(L\\) is
$$L(\theta)=\prod_i^{n} \ p_i^{y_i} \ \left(1-p_i\right)^{1-y_i}.$$

Now, we maximize the log likelihood function with stochastic gradient descent [1].
We use the sklearn `LogisticRegression` class to train and test the model.


## Evaluating the model

For evaluation, we see that our logistic regression model outputs probabilities, but our ground truth data is only 1 or 0.
Therefore, there are two ways we can evaluate our model.
The first is to see if the probabilities our model outputs correspond to the estimated true probabilities.
The second is to pick a threshold value to convert the probabilities into 1 or 0 and use normal binary classification metrics.

### Probability bins

Hitting a home run is inherently random.
Even the best player in the best conditions against the best pitcher will not always hit a home run.
This is because hitting a home run in baseball is really hard!
For evaluation, however, we do not have the true probabilities...we only have whether the player hit a home run in the game or not.
Therefore, for evaluation we should simulate a bunch of games with our probabilities and see if it lines up with the real-world results.
This process is building probability bins.

We have that our logistic regression model \\(f(x_i)=h_{\theta^\*}(x_i)=p_{i, \text{pred}}\\) outputs a probability of the positive class (home run).
We want to look at bins of our logistic model outputs and compare them to the true results.

First, we can divide our \\(N\\) datapoints into groups of 2000 elements.
Since our training data had 60k examples, we have 30 total bins.

For each bin \\(b\\), there is a set of predicted probs from our model \\(\alpha=\\{p_{\text{pred},i\in b}\\}\\) and a set of true outcomes \\(\beta=\\{y_{\text{pred},i\in b}\\}\\).
We then calculate the mean of \\(\alpha\\) and the mean of \\(\beta\\) per bin.
If our model is accurate, these means should be close.



![Probability bins](/images/baseball_modeling/probability_buckets.png#center)
_Probability bins_

We can see that the logistic regression does a good job of approximating the true probabilities.
It seems at the lower end of the range between 0.05 and 0.10, the model underpredicts pretty consistently.
Between 0.10 and 0.15, the model seems to have more variation than at the lower end of the range however.

It is also interesting to see how the model predictions are distributed:
![Predicted probability histogram](/images/baseball_modeling/predicted_probability_distribution.png#center)
_Predicted probability histogram_

We can see that our model typically outputs probabilities between 0.05 and 0.16.


### Binary classification metrics

Our logistic regression model \\(h_{\theta^\*}(x_i)=p_{i, \text{pred}}\\) outputs a probability of the positive class (home run).
We then predict \\(y_i\\) by
$$
y_{i, \text{pred}} =
\begin{cases}
0,  & \text{if $p_{i,\text{pred}}\geq t$} \\\\
1, & \text{if $p_{i,\text{pred}}< t$}
\end{cases}\ ,
$$
where \\(0\lt t\lt 1\\) is the threshold value.
We decide to use \\(t\\) as the mean of our predicted probabilities.
Here are some binary classification metrics for our model:

||
| -------- | ------- |
| Accuracy | 0.575 |
| Precision | 0.145 |
| Recall | 0.605 |
| F-1 | 0.234 |
| ROC AUC | 0.617 |

### Receiver operating characteristic (ROC)

Changing the threshold value affects the true positive rate
$$TPR=TP/(TP+FN)=E_i\left[P(p_{i,\text{pred}} \geq t\ |\ y_i=1)\right]$$
and false positive rate
$$FPR=FP/(FP+TN)=E_i\left[P(p_{i,\text{pred}} \geq t\ |\ y_i=0)\right],$$
where we have # of true positives \\(TP\\), # false positives \\(FP\\), # true negatives \\(TN\\), and # false negatives \\(FN\\).
Graphically, we can look at the distributions of our predicted probabilities for the true positive and negative classes:

![Predicted probability histogram for positive and negative classes](/images/baseball_modeling/predicted_probability_distribution_pos_neg.png#center)
_Predicted probability histogram for positive and negative classes_

By moving the threshold \\(t\\) on the x axis, we can see that the \\(TPR\\) and \\(FPR\\) will be correlated but will change at different rates.
If the two distributions were on top of each other, then we would have a random model with no predictive power.
On the other hand, if there was no overlap between the distributions, then our model would be able to make predictions perfectly.
The receiver operating characteristic (ROC) curve is a graph of the false positive rate compared to the true positive rate.

![ROC curve](/images/baseball_modeling/roc_curve.png#center)
_ROC curve_

Random guessing produces a straight line on on the ROC graph.
The better our model, the higher the area under the curve (ROC) will be.

## Feature importance

| Stat | Model weight | Accuracy of model trained <br>without feature |
| -------- | :-------: | :--: |
| Batting average | **-0.372** | 0.551 |
| On base % | 0.144 | **0.489** |
| Slugging % | 0.287 | 0.548 |
| Home runs per AB | 0.091 |0.547|
| RBIs per AB | -0.009 | 0.551 |
| At bats per game | -0.289 | 0.594 |



## Future improvements
It seems that this model performs pretty well overall.
However, we are using only batter data right now, so in the future we will add more data about the opposing pitching and other data such as location.
We can also try new models to see if it improves our metrics.
Lastly, we can train the model on more data and see if it improves.
The code used for the data and graphs is here: [link](https://github.com/lucaspauker/home_run_modeling).


## Citations
1. https://see.stanford.edu/materials/aimlcs229/cs229-notes1.pdf




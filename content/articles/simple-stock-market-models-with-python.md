---
title: "Simple Stock Market Models with Python"
date: 2020-12-20
draft: false
tags: ["finance", "cs"]
---

## Introduction

In this blog post, I will implement a few simple time series models of a stock price over time.
I will also see how they do if we trade using them.
We will look at moving averages (MA) and exponential moving averages (EMA).

## Data

First, we need to download the price data.
For this article, we will use SPY historical open price data.
We can download this from [Yahoo Finance](https://finance.yahoo.com/quote/SPY/history?period1=728265600&period2=1607904000&interval=1d&filter=history&frequency=1d&includeAdjustedClose=true).
Now, let's process the data into a dataframe and split the data into test and train datasets.

```
import pandas as pd
data = pd.read_csv('./SPY.csv')
data['Datetime'] = [datetime.strptime(d, '%Y-%m-%d') for d in data['Date']]

percent_train = 0.7
train_data = data.head(round(len(data) * percent_train))
test_data = data.tail(round(len(data) * (1 - percent_train)))
print(len(train_data), len(test_data))
```
Output: _4912 2105_

## Moving Average

Moving averages have a single parameter: the number of datapoints we look back.

```
n_param = 30  # Number of days to look back with
```

Now let's build the moving average data.

```
import numpy as np
ma = []
ma += ([None] * n_param)
for i in range(len(dataset) - n_param):
    ma.append(np.mean(dataset['Open'].iloc[i: i + n_param]))
dataset['MA'] = np.array(ma)
dataset['MA'] = dataset['MA'].dropna()
```

Let's plot the data.

```
import matplotlib.pyplot as plt
fig = plt.gcf()
fig.set_size_inches(10, 5)

r = range(0, 400)
plt.plot(dataset['Datetime'].iloc[r], dataset['MA'].iloc[r], label='MA')
plt.plot(dataset['Datetime'].iloc[r], dataset['Open'].iloc[r], label='Open Price')
plt.title('SPY Open Prices')
plt.xlabel('Time')
plt.ylabel('Price')
plt.legend()
plt.grid()
plt.show()
```

![SPY moving average](/images/spy_ma.png#center)
_Moving average (n=30) for subset of SPY data_

Now let's try a simple mean reversion trading strategy.
If the MA is below the open price by a _THRESHOLD_, then we will sell SPY.
If it is above the open price by a _THRESHOLD_, then we will buy.
In principle, if the moving average is a better indicator of true value than the price today, this strategy will do well.

```
MAX_POSITION = 50
THRESHOLD = 1

position = 0
cash = 5000
positions = [position]
capital = [cash]

for i in range(1, len(dataset)):
    capital.append(cash + position * dataset['Open'].iloc[i])
    positions.append(position)

    diff = dataset['Open'].iloc[i] - dataset['MA'].iloc[i]
    if abs(diff) <= THRESHOLD: continue
    if np.sign(diff) == -1:  # Buy
        if abs(position - 1) > MAX_POSITION: continue
        cash += dataset['Open'].iloc[i]
        position -= 1
    elif np.sign(diff) == 1:  # Sell
        if abs(position + 1) > MAX_POSITION: continue
        cash -= dataset['Open'].iloc[i]
        position += 1

sign = np.sign(position)
for _ in range(abs(position)):  # Get rid of remaining position
    cash += sign * dataset['Open'].iloc[-1]
    position -= sign * 1
assert(position == 0)
print(cash)
```
Output: _10163.34_

Being long SPY would have left us with $15,468, so this strategy is pretty bad.
However, we arbitrarily chose `n_param=30`.
Let's see what parameter does best on the training dataset.
Here is the result of the trading strategy above for `n_param` in the range [0, 500), sampled every 10 numbers.

![MA Parameter Estimation](/images/n_param_ma.png#center)
_Cash after trading versus different MA parameters_

We can see that around `n_param=370` is the best parameter for our training dataset.
Let's use this parameter on the test dataset and see how it does.

![MA Returns](/images/ma_returns.png#center)
_MA vs. long SPY returns_

As we can see, the MA strategy does worse than being long SPY.
It also looks like there is more variance in returns.
This probably has to do with the fact that our test data is from 2013-2020, which was a bull market.
Thus, our MA strategy is mostly long and doesn't differentiate itself from the baseline strategy very much.

##Exponential Moving Average
The idea behind EMA is that more recent data has a higher weight than old data.
EMA takes a smoothing parameter $\alpha$
The formula for the EMA of a time series with prices $(p_1,p_2,...)$ is:

$$
EMA_t = \alpha * p_t + (1 - \alpha) * EMA_{t-1}
$$
for $t > 1$. Also,
$$
EMA_1 = p_1.
$$
Expanding this equation, we get
$$
\begin{eqnarray}
  EMA_t
  &=&\alpha *[p_t + (1 - \alpha) *p_{t-1} + (1 - \alpha)^2 *p_{t-2} +...]\\\\
  &=&\alpha *\sum_i (1 - \alpha)^i *p_{t-i}.
\end{eqnarray}
$$
Therefore, we can see that points further back in time are weighted less. Here is the code to generate the EMA from our data:

```
ema = [dataset['Open'].iloc[0]]
for i in range(1, len(dataset)):
    mult = 2 / (n_param + 1)
    prev_ema = ema[i - 1]
    ema.append(mult * (dataset['Open'].iloc[i] - prev_ema) + prev_ema)
dataset['EMA'] = np.array(ema)
dataset['EMA'] = dataset['EMA'].dropna()
```

Let's compare the EMA to the simple MA.

```
fig = plt.gcf()
fig.set_size_inches(10, 5)

r = range(0, 400)
plt.plot(dataset['Datetime'].iloc[r], dataset['MA'].iloc[r], label='MA')
plt.plot(dataset['Datetime'].iloc[r], dataset['EMA'].iloc[r], label='EMA')
plt.plot(dataset['Datetime'].iloc[r], dataset['Open'].iloc[r], label='Open Price')
plt.title('SPY Open Prices')
plt.xlabel('Time')
plt.ylabel('Price')
plt.legend()
plt.grid()
plt.show()
```

![SPY exponential moving average](/images/spy_ema.png#center)
_EMA and MA (n=30) for subset of SPY data_

Conducting the same analysis as the simple MA, we see that we maximize return on the training dataset when `n_param=370`. Similar to the MA, however, when we run the mean reversion strategy on the test dataset, it performs slightly worse than simply being long SPY.

## Conclusion
We can see that trading using this methods does not yield any edge.
This is expected however, as these models are very simple and don't take into account any information besides the price of SPY.
I hope that the analysis is interesting and that this post clearly shows how to build and test simple models.

## Sources
- [https://en.wikipedia.org/wiki/moving_average#exponential_moving_average](https://en.wikipedia.org/wiki/moving_average#exponential_moving_average)


---
title: "Your Prod Code Should Have Bugs"
date: 2025-11-11
draft: false
tags: ["programming", "cs"]
---

<div class="center-box">
<img src="/images/this_is_fine.jpg" alt="This is fine meme" class="medium-image">
</div>

_Debugging prod on a Friday night_

## What is your job as a software engineer?

When you ship software, you are balancing the following two things:
1. Get it done fast -> doing it faster means the feature appears earlier for customers and ultimately the business makes more money
2. Don't introduce bugs -> bugs can cause [outages](https://www.geekwire.com/2025/how-the-aws-outage-happened-amazon-blames-rare-software-bug-and-faulty-automation-for-massive-glitch/) or other errors that lead to losses

The problem is that anyone that has worked with software knows that no matter what, you cannot guarantee that your shipped software will not have bugs.
In fact, your software will always have bugs.
No matter how many "lgtms" you get on your pull request, bugs always find a way of creeping in.

I have lately been thinking a lot about risk management as a software developer.
Working in the trading industry, bugs often directly lead to monetary losses, so you want to avoid them at all costs!
In fact, entire firms have gone out of business due to software bugs.
The most famous example of this is [Knight Capital](https://en.wikipedia.org/wiki/Knight_Capital_Group#2012_stock_trading_disruption).
They had a software bug that caused their automated trading software to use millions of dollars to buy stocks causing them to lose 440 million dollars in less than an hour and go out of business.
So as a software developer at a trading firm, a key component of your job is to avoid catastrophic errors at all cost.

This idea that a large portion of your job is to avoid catastrophic errors still applies outside of trading, however.
An outage for a large company can cost the company millions of dollars every hour, and there are other ways software can cause monetary losses.
Therefore, every software developer should be conscious of risk when they are pushing new code.
As a software developer, you are a risk manager.


## Let's squash all the bugs

There is only one way to truly never introduce bugs into your software: never push new code.
But your whole job is to push code, so we can't do that...
Instead, we can think of a framework to dictate how much time you should dedicate to a task in order to manage the risk of bugs.
This is meant as a heuristic way to think about software development, not an exact science.

First, let's assume we have a task that has some value to the business \\(v\\).
We can think of \\(v\\) as money generated over time, since the task contributes to the business’s overall value each day.
Second, we have the cost of the bug \\(c\\).
Lastly, we have some function \\(p(t)\\) that is the chance of the bug happening.
This function decreases as time increases.

Then we can think of minimizing the loss function
$$L(t) = t * v + c * p(t).$$
We can see that as time increases, the first part of the equation \\(t * v\\) increases, but the second part of the equation \\(c * p(t)\\) decreases.
So the minimum of this equation will be when the marginal value of adding the feature to prod exceeds the risk of a bug.


## Different stakes for different jobs

Cancer machine


## Mitigating risk

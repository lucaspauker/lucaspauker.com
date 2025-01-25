---
title: "Why Gambling is (more) Rational (than you think)"
date: 2025-01-16
draft: false
---

I remember watching a [video](https://youtu.be/3kGlk1E_Cnw?si=iXkaoAK-_p6_ta9K&t=654) a while ago where a pro card counter answers questions about gambling.
In one of his answers, he said that what makes someone a true gambler is that the joy of winning a bet is more than the sadness from losing a bet.
This has stuck with me since I watched the video over a year ago.
Since then, I sometimes think about why people gamble and what makes some people more prone to gambling.
I play poker with my friends weekly, and in my group, there are many different kinds of gamblers.
Some people are there for fun, some care more about making money, and some just like the thrill of gambling.

## Why would you ever gamble?

Most gambling has negative expected value (EV), which is the average amount you can expect to win or lose per bet if you were to play an infinite number of times.
When you play any game at the casino (besides poker, perhaps), you have negative EV.
This means that even if you play the game perfectly, you will lose money in expectation over time.
Of course, you can still make money by getting lucky, but over time with enough bets you will lose money.
Don't believe me? Look at how big and expensive the casinos are in Vegas...they didn't get that big by losing money in their own games.

I made a simulation of the amount of money a player will have in a game where the house has a 1% edge.
In the simulation, the player always bets 5% of their money and they play 100 games each trial.
The game has a 50% chance of winning: if you lose you lose your bet and if you win you get your bet * 1.98 back.
So the EV of this game is 0.5 * 1.98 = 0.99.

![Gambling simulation](/images/gambling/gambling_simulation.png#center)

As we can see, the game is negative EV--you are expected to lose about 35$ after playing 100 times...and most casino games have a greater than 1% edge.
So then why would you ever gamble?
Are gamblers irrational or dumb?
I don't think this is the case--I think there are actually a few different types of gamblers.
Most of these gamblers are making rational decisions, but some of their decision making criteria may be flawed.
I will describe them more in depth but here are the three types:
1. Recreational gambler
2. Misinformed gambler
3. Rational gambler

## General framework

In economics, there is a concept of utility.
Utility is how much value someone gets from certain goods, services, or outcomes.
Utility varies from person to person.
For example, I spend my money on musical instruments since I get utility from playing music, while someone else might instead spend their money on unicycles or juggling clubs since they get utility from that.
So utility is a general framework for understanding why people make certain decisions.
If someone makes a decision to do something they expect to get more utility from doing that than the alternative.
So, even if gambling is negative EV in terms of money, it can be positive EV in terms of utility.
However, each type of gambler derives their expected utility from a different source.

Formally, each person has a utility function \\(U\\) which takes as inputs the assets and experiences the person has.
Therefore, the \\(\Delta U(\text{action})\\) function is how much the utility will change after a certain action.
Since we are dealing with gambling, which is uncertain, we will usually think of \\(U\\) and \\(\Delta U\\) as the expected utility and expected change in utility, respectively.
Some of the equations in general will not be exact, but I am just trying to communicate the main ideas.

In order for it to make sense to gamble, we need to have
$$\Delta U(\text{making bet}) > \Delta U(\text{not making bet}) = 0.$$
We assume that the expected change in utility of not making a bet is zero for simplicity.

For a risk-averse person who does not gamble, the expected change in utility for making a bet might look like this:
$$ \Delta U(\text{making bet}) = p_\text{win} * \Delta U(\text{win payout}) + (1-p_\text{win}) * \Delta U(\text{losing bet}) < 0, $$
where \\(p_\text{win}\\) is the chance of winning the bet.
Because a risk averse person would rather not lose money than take the chance to win money, the utility of making the bet is negative and they will therefore not gamble.

## Recreational gambler

The first type of gambler is the recreational gambler.
This is the kind of person that likes to play poker for fun or go to the casino once in a while with friends for a good time.
The recreational gambler understands that they are going to lose money on average, but they derive utility from the act of gambling, since the thrill is fun.

So for the recreational gambler, we have
$$\Delta U(\text{gambling}) = \Delta U(\text{enjoyment of gambling}) + \Delta U(\text{making bet}) > 0$$
and therefore
$$\Delta U(\text{enjoyment of gambling}) > -\Delta U(\text{making bet}).$$
So for the recreational gambler, the utility gained from the enjoyment of gambling outweighs the negative utility of placing the bet.

## Misinformed gambler

The second type of gambler is the misinformed gambler.
This is someone who THINKS they have positive EV but actually has negative EV.
The place where I see this happen the most is sports betting.
Because people watch football, they think that they can beat the books and predict the outcomes of games better than the books.
While it is certainly possible to beat the books, most people do not succeed in doing so.
It is also possible to be a misinformed gambler in a game like poker, where a player may overestimate their skill.

For the misinformed gambler, their expected utility function does not match the true utility of making the bet.
$$
\begin{align}
\Delta U(\text{making bet}) =& \\  p_\text{win perceived} * \Delta U(\text{win payout}) \\\\
                            &+ (1-p_\text{win perceived}) * \Delta U(\text{losing bet}) \\\\
                            &> 0,
\end{align}
$$
where \\(p_\text{win perceived}\\) is the perceived chance of winning the bet.

## Rational gambler

The last category of gambler is the rational gambler.
A rational gambler is someone who gets more joy from winning a bet than sadness from losing the bet.
In order to understand the rational gambler, we need to look at the marginal utility function for money.
The marginal utility is the amount of utility someone gets from their Nth dollar.
So basically we think of how much utility do I get from the 1,000th dollar in my bank account compared to the 10,000th dollar.
For a non-gambler, money has less utility as they get richer.
This is because there is really a fixed amount of things to spend money on and after the first superyacht...will you really be much happier with a second?

![Normal marginal utility](/images/gambling/normal_marginal_utility.png#center)

However, for a rational gambler, the marginal utility function is flipped: it has a positive slope.
This means that the gambler gets more utility from being rich than from being poor.

![Gambler marginal utility](/images/gambling/gambler_marginal_utility.png#center)

The question is why would someone have a utility function like this?
In general, the reason is that the utility of money is based on what you can spend with the money.

The first explanation of why this curve makes sense is for someone who is a compulsive spender.
Imagine that you are someone who spends all the money you have on a meal at the end of the day and you earn $10 every day.
So that means you can get a Subway sandwich every day.
Or instead, you could gamble that $10 and risk eating beans and rice ($1) for the chance at getting a steak dinner ($80).
Let's say that nine times out of 10 you end up eating beans and rice, but one time out of ten you get the steak dinner.
So would you rather eat Subway every day or take the gamble and mostly eat beans and rice but sometimes get a very nice dinner?
To me, this choice is not very clear and it makes sense why one would choose the gamble.

A second explanation has to do with someone who thinks they are unable to make enough money to get out of their current socioeconomic class or acheive a financial goal.
For example, if you cannot save much and see that it will take a hundred years to buy a house, then how can you deal with this?
One option is to save for a hundred years and never get your house, or instead you can buy a lottery ticket every week and have a chance at acheiving a house in the short term.
Sometimes, this seems like the only way.

In reality, marginal utility is not a simple straight line or decreasing function for just about anyone.
For example, money to spend on food and rent has much more utility than money spent on recreation.
Furthermore, even for a gambler, there is a certain point where money has decreasing value.
So the marginal utility curves need to be thought of as a guide to understanding decision making, not a fact of how everybody thinks.

## Conclusion

In [articles](https://www.wsj.com/business/hospitality/gambling-addiction-sports-betting-apps-4463cde0) about gambling, I think that there is not enough emphasis into understanding why people decide to gamble in the first place.
Often, gamblers are treated as addicts with a problem or irrational people.
I hope that increasing understanding into the motives behind gambling in the first place can lead to better policy regarding gambling.



---
title: "US Treasury Futures Delivery Options"
date: 2020-11-30
draft: false
tags: ["finance"]
---

## Introduction

This blog post will discuss the delivery options for someone who is short US Treasury (UST) futures contracts.
During the last month of trading, the short "delivers" the USTs specified in the futures contract to the long.
The short has various options when they make delivery of the treasuries.
Understanding these options is interesting and important for anyone who trades UST futures and the UST basis.

UST futures are some of the most liquid financial contracts in the world.
UST futures began being traded in 1977 and the Chicago Board of Trade has consistently introduced more Treasury futures products due to their popularity.
UST futures are unique from other futures contracts because the short can deliver any UST from a basket of bonds/notes.
For example, for the 10-year T-note futures (TN), one can deliver,
>"U.S. Treasury notes with a remaining term to maturity of at least six and a half years, but not more than 10 years, from the first day of the delivery month" ([CME&nbsp;Website](https://www.cmegroup.com/trading/interest-rates/us-treasury/10-year-us-treasury-note_contract_specifications.html)).

Since there are many notes with maturities between six and a half years and 10 years, the short who is delivering bonds has a wide range of notes to choose from.
This optionality to choose from a variety of securities, as well as the fact that the short can choose any day in the delivery month to make delivery, creates a variety of valuable options for the short.

Trading of a UST futures contract terminates a week before the end of the delivery month.
The options the short has to change the contract they are delivering during this week are known as the “end-of-month” option.
The options before trading expires to change the contract are known as the “switch” option.
Lastly, the short has a timing option since they can choose when to deliver throughout the month.
I will go through these options separately, starting with the switch option.

For this article, I will assume some understanding of US treasuries, yields, conversion factors, and cheapest to deliver contracts.
I will include links to supplemental articles.

## The Switch Option

The switch option, which is the short's option to switch the delivery contract before the last week of the month, has value due to possible changes in the [cheapest to deliver contract](https://www.investopedia.com/terms/c/cheapesttodeliver.asp).
The switch option depends on a number of factors, namely changes in yields, changes in the slope of the yield curve, and new UST issues.

### Changes in Yields

As yields change, as mentioned, the cheapest to deliver contract may change.
There are two rules of thumb that give intuition for how yields affect the cheapest issue to deliver.

1. Duration rule:
For bonds with the same yield below 6%, the bond with the lowest duration will be cheapest to deliver.
For bonds with the same yield above 6%, the bond with the highest duration will be cheapest to deliver.
2. Yield rule:
For bonds with the same duration, the bond with the highest yield will be cheapest to deliver.

The [duration of a bond](http://www.rnfc.org/courses/finance/modules/bond-yields-modelling/Understanding_Duration_Volatility.pdf) is the weighted average time of cash flow payments.
As a result, prices of higher duration bonds are more sensitive to yield changes.
6% is an important number since the conversion factor of a bond is the price at which it would trade if its yield to maturity was 6%.
Therefore, if all bonds in the delivery basket yielded 6% during the delivery month, all bonds would have a converted price of 100 and there would be no cheapest to deliver bond.
So for two bonds with the same yield but different durations, if yields fall from 6%, the price of the bond with the higher duration will rise relatively more, so the lower duration bond will be cheaper to deliver.
If yields instead rise from 6%, the higher duration bond's price will fall more than the bond with the lower duration and thus the higher duration bond will be cheaper to deliver.
Thus, we have the first rule.

If we instead have two bonds with the same duration but different yields, the [converted price](https://www.cmegroup.com/trading/interest-rates/files/Calculating_U.S.Treasury_Futures_Conversion_Factors.pdf) for the higher yield bond will be lower than the lower yield bond so the higher yield bond will be cheaper to deliver. This is the second rule.

These two rules help us understand how changes in yield will change the cheapest contract to deliver.
The ability to change the delivery contract is valuable to the short since they can choose to deliver the cheapest contract.

### Changes in the Yield Curve

There is a tendency for the yield curve to flatten as rates increase and steepen as they decrease ([here is my tool](/yield-curve) to see today's yield curve).
Bond yields with different maturities do not move in parallel.
Failing to take this fact into account would result in a mispricing of the short's switch option value.
The yield curve tends to flatten as rates increase and steepen as rates decrease.
Because of this, the value of the switch option is reduced.

To see this, imagine we have two bonds A and B such that A has a longer maturity than B.
If yields fall, then the yield curve tends to steepen so the shorter maturity bond (B) will have a larger change in yield than A.
This means that B will be relatively more expensive compared to A.
Falling yields however tend to cause lower duration bonds (B) to become cheaper, so this effect means that a larger change in yields is required for a change in the cheapest to deliver contract.
If yields rise, then the yield curve tends to flatten so the shorter maturity bond (B) will have a larger upward change in yield than A.
This means that A will be relatively more expensive compared to B.
Rising yields however tend to cause higher duration bonds (A) to become cheaper, so this effect also means that a larger change in yields is required for a change in the cheapest to deliver contract.
The result of the relative price changes between A and B is that the switch option will be less valuable since a change in the cheapest bond to deliver requires a larger change in yields.

Nonsystematic events may also result in yield spread changes.
When a specific single issue becomes expensive to deliver relative to other issues, it causes nonsystematic changes in yield spreads.
The examples of nonsystematic changes that [Belton et al.](#basis-book) give for this is changes in issuance, temporary squeezes, and Fed buyback programs.
The effect of this type of event is that the yield curve may flatten as yields fall or that the yield curve may steepen as yields rise.
This results in the opposite effect of systematic changes.
Nonsystematic yield curve changes add value to the delivery option if nonsystematic changes are likely enough and sufficiently large to cause a change in the cheapest to deliver.

### New Issues

Bonds and notes issued before the first delivery day are eligible for delivery.
If yields are high, higher durations are favored, according to the first rule of thumb above.
Since the new issue will have a low duration due to its high coupon, it will not likely be the cheapest to deliver.
If yields are low, lower durations are favored.
Since the new issue will have a high duration due to its low coupon it will also not likely be the cheapest to deliver.
Furthermore, new issues ("on the run" issues) tend to trade at a premium for liquidity so they are typically expensive.
Thus, we can see that under normal circumstances, the new issue is rarely the cheapest to deliver.
This means that the new issue option is not very valuable.

An exception to this rule is when yields are high but falling.
High yields mean that high duration bonds are cheaper to deliver.
But since yields are falling, the new issue has the lowest yield and longest maturity in the delivery basket, meaning it has a high duration, which would make it cheapest to deliver.

## The End-of-Month Option

The last day of trading for a futures contract is the [seventh business day before the last business day of the delivery month](https://www.cmegroup.com/trading/interest-rates/us-treasury/10-year-us-treasury-note_contract_specifications.html).
After the last trading day, the futures price for each bond is fixed, however the cash treasuries still trade.
Therefore, a sufficiently large change in yields might change the cash price of assets enough to change which contract is cheapest to deliver.
Changes in the cheapest to deliver contract are driven by different reasons during the end-of-month period compared to regular trading.
Since the futures invoice price is fixed during the end-of-month period, the net cost of delivering a bond into the futures contract only depends on the absolute cash price.
Therefore, high yield, low duration bonds tend to be cheaper as yields rise during the end-of-month period.
This contrasts with the first rule of thumb above that higher duration bonds are cheaper to deliver as yields rise before the end-of-month period.

## The Timing Option

The short has not only the option to choose which contract to deliver, but they can also choose when to deliver it within the delivery month.
This is the timing option.
There are three factors that can affect the timing option: carry, aftermarket price moves, and limit moves.

### Carry

If the cost of financing a bond is less than the coupon income on the bond, carry is positive.
If carry is positive, a long basis position (long cash, short futures) has positive cash flow since the bond position earns the positive carry.
Therefore, it is advantageous for one who is short futures to wait to make delivery.
If conversely, carry is negative, then it may pay for the short to make delivery at the beginning of the month to avoid negative carry costs.
However, delivering early means that the short gives up the value of the other delivery options, so it may not be advantageous.
There is a tradeoff for delivering early.

### Aftermarket Price Moves

This option is known as the wild card option and has to do with how the futures trading is structured.
During the delivery month, at 2:00PM (Central Time), futures trading closes and the CME determines the final settlement price for all futures contracts that expire in the delivery month.
All delivery invoices declared that day are determined by the final settlement price.
However, the cash treasuries market doesn't close until 4:30PM.
This means that there is a two and a half hour period where the futures settlement price is fixed but cash can still trade.
Since the short has until 8:00PM to declare intent to deliver, they can take advantage of price moves in the cash market during this time period.
If the cash price of a deliverable bond changes substantially, it may pay for the short to declare delivery that day and buy the cash treasuries.
Note that, assuming that the short is hedged, the cash purchase only consists of the "tail" on their position, or the remaining amount of cash treasuries the short must buy to make delivery.

### Limit Moves

During the delivery month, there are no price limits for the treasury futures contracts.
However, the short can give notice of delivery during the last two days of the previous month, where there are price limits.
If there is a limit move in those two days, the futures settlement price essentially is determined early and the wild card play is more likely to happen.
In practice, this option has little value because it only lasts two days.

## Conclusion

We can see that the delivery options for the short are complex and numerous.
I hope that this blog post is an interesting overview of the short's delivery options and helps the reader understand some of the complexity.

## Sources
- [http://www.rnfc.org/courses/finance/modules/bond-yields-modelling/Understanding_Duration_Volatility.pdf](http://www.rnfc.org/courses/finance/modules/bond-yields-modelling/Understanding_Duration_Volatility.pdf)
- [https://www.cmegroup.com/trading/interest-rates/files/us-treasury-futures-delivery-process.pdf](https://www.cmegroup.com/trading/interest-rates/files/us-treasury-futures-delivery-process.pdf)
- [https://www.cmegroup.com/education/files/treasury-futures-basis-spreads.pdf](https://www.cmegroup.com/education/files/treasury-futures-basis-spreads.pdf)
- <a id="basis-book">[Belton, Terrence M., et al. The Treasury Bond Basis: an in-Depth Analysis for Hedgers, Speculators, and Arbitrageurs. McGraw-Hill, 2005. ](https://www.goodreads.com/en/book/show/1795036.Treasury_Bond_Basis)</a>

---
title: "Newber"
date: 2025-05-03
draft: false
tags: ["ideas", "finance", "markets"]
---

Recently, when I was at the airport looking to get a ride share back home I checked the price of both Uber and Lyft. I noticed that an Uber was 25$ and a Lyft was 39$…that is a price difference of 14$, or more than half the price of the Uber! The Lyft ride was 56% more expensive…

How can two multibillion publicly traded companies have such different pricing? Both companies have entire teams devoted to determining pricing, yet they are still so off.

![AI car photo](/images/newber/ai_cars.png#center)
_Rideshare cars photo from ChatGPT_

At the end of the day, ride sharing is a market. The app connects riders (buyers of a service) to drivers (sellers of a service). However, this market is different than most financial markets. Namely, each rider has a different start and end point. This means that rides are not fungible. Accurate pricing must include the start location, the end location, time of day, traffic, etc.

We propose a solution to have ride sharing pricing be discovered using a free market. By doing so, we can create the most accurate and fair pricing of any ride sharing app. Furthermore, since pricing is discovered using a market, we do not have to spend as many resources on price determination as other players in the space.

First, we will present a simplified version of the idea. Then, we will dive into a more complex, real-world version of the idea.

## Simplified model

We will use Chicago as an example. The first problem we must solve is how to create a market that all customers can use. This means that customers that are on different routes should be able to place bids in the same market. The simple way to do this is to use a metric that is common across all rides. Instead of buying and selling rides, we will buy and sell dollars per minute. Since we are only using Chicago, we can assume that all the dollars per minute across the city are comparable. In the marketplace, drivers place offers on dollars per minute. We can intuitively think of this as the rate that the drivers are willing to take to get paid. For other ride share apps, drivers cannot set this in the same way.
On the other side of the marketplace, riders place bids on dollars per minute. A rider that needs to go somewhere quickly will just lift the best offer. A rider that wants to wait and save can place a competitive offer. Whenever there is a fill in the market, a driver will give a real ride to the rider for the agreed upon price.

One issue with this market is that the price discovery will be difficult, especially with a small number of drivers and riders at the beginning. Furthermore, drivers and riders may not want to place bids and offers from a UX perspective. To solve this, we will have a separate derivatives market where price discovery happens. This market can be used by speculators and market makers and will settle in cash.
The market will be set up as a futures market with the settle price occurring during a specific window. The market will settle to the weighted average of the dollar per minute of all rides taken during the window. Sharp participants in the market will take into account events such as a Chicago Bears game happening during the window that would affect supply and demand.

Then, when a driver or rider logs onto the app, they can see what the market-determined dollar per minute is and use this.

![Simplified model graphic](/images/newber/simple_graphic.png#center)
_How the simplified model works_


## Generalized model

The price per minute model fails since there are so many variables that affect the price of a ride such as start location, end location, time of day, traffic, weather, etc.
Because there are so many variables, we cannot have a market for every combination of variables.
Even if we decide to make markets for buckets of these variables (i.e. rainy, snowy, sunny weather), there are still too many combinations and this is a subjective way to do it.
We want to have a more general formulation for getting fair prices.

For each ride \\(i\\) that gets requested, we can list off the \\(n\\) features:
$$F_i\in\mathbb{R}^n.$$
The features can be arbitrary based on what we think is relevant for pricing.
Next, in our marketplace, there will be agents that submit their predicted the price of a ride for a given set of features.
Each agent \\(k\\) has an algorithm that spits out a price:
$$a_k(F_i)\in\mathbb{R}.$$
Once a ride is completed, we observe the true transaction price \\(p_i\\).
We use this true price as the ground truth to evaluate the accuracy of each agent’s prediction.
To incentivize good pricing behavior, each agent is rewarded based on how close their predicted price \\(a_k(F_i)\\) is to the true price \\(p_i\\). This can be done using a scoring rule such as:
$$R_{k, i} = - \|a_k(F_i) - p_i\|,$$
or a smoother version like a Gaussian or log scoring rule that sharply penalizes large deviations and rewards accurate forecasts.
Over time, agents who consistently predict well will earn more, while poorly performing agents may be de-weighted or removed.
This structure turns price discovery into an open, competitive marketplace for algorithms. Rather than relying on a centralized pricing engine, the platform outsources the pricing function to a distributed set of market participants. Each participant contributes pricing intelligence based on different perspectives, models, or data sources, and is financially incentivized to be as accurate as possible.
Importantly, this also allows the system to adapt dynamically. If traffic patterns shift due to a new road closure or there's an unexpected surge in demand near a stadium, agents that can quickly pick up these signals and adjust their pricing logic accordingly will profit. This creates a robust, decentralized forecasting mechanism that improves over time without centralized tuning.

Now that we have a system for generating decentralized, competitive price predictions, we can layer a UX on top that is intuitive and responsive to real-time market conditions.
When a rider opens the app and enters their desired destination, the app computes the ride's feature vector \\(F_i\\) and queries the pricing market. It displays the current predicted fair price based on the aggregate of agent submissions—this could be a median or weighted average of \\(a_k(F_i)\\) values.
This could also be rated by the average reward of the algorithm to give more weight to the accurate algorithms.
The rider can then choose to:
- Accept the market price and be matched with the best available driver.
- Place a lower bid if they are willing to wait for a driver to accept a cheaper rate.
- Request a premium ride with priority matching if they are in a hurry.

On the driver side, the app continuously shows nearby ride requests, each with the proposed price and estimated trip details. Drivers can:
- Accept a posted price (either rider-submitted or market-based).
- Post their own ask if they are only willing to drive at a higher rate.

Once a ride is matched and completed, the system logs the final transaction price \\(p_i\\) and uses it to score the agents who predicted that ride’s price. This feedback loop keeps the pricing market aligned with real-world behavior and allows the app to function as a real-time market clearing platform without needing static pricing rules or fixed fare tables.
This UX model strikes a balance between market flexibility and ease of use. Riders still see a simple “price to get home,” but that price is rooted in a deeper, incentive-aligned marketplace. At the same time, power users—drivers or riders—can express preferences and benefit from price discovery.

To complement the decentralized ride-level pricing system, we can maintain a city-wide futures market that trades contracts on the average ride cost (e.g., dollars per minute) over specific future time windows. This market captures macro-level expectations around supply and demand—such as rush hour, weather disruptions, or events—and provides a benchmark signal that pricing agents can reference or hedge against. It adds liquidity, enables forward-looking speculation, and strengthens overall price discovery by anchoring local predictions to broader market sentiment.

![Generalized model graphic](/images/newber/generalized_graphic.png#center)
_How the generalized model works_

## Comparing the models

| Feature                             | Simple Model (Dollar/Minute)                         | Generalized Model (Decentralized Pricing Agents)              |
|-------------------------------------|-------------------------------------------------------|----------------------------------------------------------------|
| Pricing Metric                      | Flat \$ per minute across city                        | Price predicted from ride-specific feature vector \\(F_i\\)     |
| Fungibility Assumption              | Rides are interchangeable by duration                | Rides are non-fungible, context-sensitive                     |
| Price Discovery                     | Single market for \$ per minute                       | Competing agents submit price predictions per ride            |
| Market Participants                 | Riders and drivers directly bid/ask                  | Agents price, riders and drivers accept/submit bids           |
| Flexibility                         | Low – can't adapt to ride-specific factors            | High – adapts to origin, destination, traffic, weather, etc.  |
| Futures Market Integration          | Core mechanism for price discovery                    | Provides macro-level benchmark for local predictions          |
| Scalability                         | Limited – breaks down in edge cases                   | Scales with algorithm diversity and feature dimensionality    |
| Long-Term Adaptability              | Manual adjustments required                           | Improves over time via competition and feedback               |



## Challenges and limitations
While a decentralized, market-driven pricing system offers flexibility, transparency, and adaptability, there are several key challenges that must be acknowledged:

1. **Liquidity**:
If there are too few drivers, riders, or pricing agents participating at any given moment, price discovery may become unstable or noisy. In particular, thin markets could lead to large pricing errors or poor matching, especially during off-peak hours or in low-density areas.

2. **User experience complexity**:
Introducing features like bidding, negotiation, or multiple price predictions may confuse users accustomed to the simplicity of “tap and go” ride apps. Balancing the benefits of market-driven flexibility with the need for an intuitive UX is non-trivial.

3. **Latency**:
Real-time matching and pricing require extremely low-latency systems. If pricing agents must compute and submit predictions in milliseconds, or if auctions are conducted for each ride, the infrastructure must be robust and fast enough to avoid slowing down ride dispatch.

4. **Gaming the system**:
Both pricing agents and users (riders/drivers) could try to manipulate the system—for example, by submitting misleading bids or gaming the prediction scoring mechanism. Designing incentive-compatible mechanisms is critical.

5. **Cold start**:
In the early phase of the platform, pricing agents may lack enough data to produce accurate models, and user participation may be too low to support price discovery. This makes it difficult to reach equilibrium or to calibrate the reward mechanism for agents.

6. **Regulatory and legal constraints**:
A fully decentralized pricing model could conflict with local regulations around transportation services, fare transparency, or algorithmic accountability. These systems would need to be carefully evaluated for legal compliance.

![AI car photo with Newber](/images/newber/ai_cars_flames.png#center)
_AI cars with Newber_


---
title: "Charity Calculator"
date: 2024-12-23
draft: false
author: "Lucas Pauker"
tags: ["charity", "modeling"]
---

<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>

<style>
    body { font-family: Arial, sans-serif; margin: 40px; }
    .widget-title { text-align: center; }
    .slider-container { margin-bottom: 20px; display: flex; }
    label { display: block; margin-bottom: 5px; margin-left: 16px; font-weight: bold; }
    input[type="range"] { width: 80%; display: inline-block; }
    span { display: inline-block; min-width: 60px; text-align: center; }
    .output { margin-top: 20px; text-align: center; color: black; }
    .chart-container { overflow-x: auto; width: 100%; }
    canvas { margin-top: 20px; border: 1px solid #ddd; width: 100%; }
    .widget-container-outer { width: 100%; border: 1px solid lightgrey; border-radius: 16px; margin: 64px 0px; box-shadow: rgba(99, 99, 99, 0.2) 0px 2px 8px 0px; }
    .widget-container-inner { margin: 32px; }
</style>

## Introduction
With holiday season in full swing, people may be more enticed to give to charity. However, there is a question of how to optimize your charitable donations. Specifically, I would like to optimize charitable donations for maximum impact. It seems to me that there are two parts to this equation:

1. Which charity(ies)/cause(s) should I give to
2. How much should I give?

The answer to the first question is deeply personal, so I want to focus on the second question. A naive answer to this question might be to donate as much as you can. However, this actually might not be the right choice if you believe you can compound your money and donate more in the future. The other extreme is to compound your money until you die and donate it all then. However, there are people that need help now so this seems like a bad solution as well.

I want to create a framework for deciding how much to give each year (based on some intuitive parameters).


## Existing frameworks for charitable giving
Before diving into my framework, it is interesting to review some existing approaches to charitable giving, many of which are grounded in religious traditions.

In Jewish tradition, giving to charity, or *tzedakah*, is a core principle. The Torah emphasizes the importance of helping those in need, and many interpretations suggest donating approximately 10% of one’s income. This practice is often referred to as *ma'aser kesafim* (giving a tenth).

In Islam, *Zakat* is one of the Five Pillars, making charitable giving a fundamental religious duty. Muslims are generally required to donate 2.5% of their accumulated wealth (beyond basic needs) each year.

While these frameworks provide clear guidelines, they tend to use a fixed percentage of wealth or income. This approach, while simple and effective, may overlook important considerations such as individual financial circumstances, the diminishing marginal utility of wealth, and the impact of time on charitable effectiveness.

Here is a simulation of donating a fixed percentage of a portfolio that grows at a fixed rate plus income.

<div class="widget-container-outer">
<div class="widget-container-inner">
<h3 class="widget-title">Constant percentage calculator</h3>
<label>Initial Portfolio Value (V₀): $<span id="av0-value">1000000</span></label>
<div class="slider-container">
    <span>1K</span>
    <input type="range" id="av0" min="1000" max="1000000" step="1000" value="1000000" oninput="updateDonationsA()">
    <span>1M</span>
</div>
<label>Annual Portfolio Growth Rate (p%): <span id="ap-value">8</span>%</label>
<div class="slider-container">
    <span>0%</span>
    <input type="range" id="ap" min="0" max="30" step="0.1" value="8" oninput="updateDonationsA()">
    <span>30%</span>
</div>
<label>Annual Income Minus Expenses (iₜ): $<span id="ait-value">0</span></label>
<div class="slider-container">
    <span>0</span>
    <input type="range" id="ait" min="0" max="1000000" step="1000" value="0" oninput="updateDonationsA()">
    <span>1M</span>
</div>
<label>% Donated: <span id="adonated-value">5</span></label>
<div class="slider-container">
    <span>0</span>
    <input type="range" id="adonated" min="0" max="100" step="1" value="5" oninput="updateDonationsA()">
    <span>100</span>
</div>
<div class="achart-container">
    <canvas id="achart"></canvas>
</div>
</div>
</div>

<script>
let chartInstanceA = null;

function updateDonationsA() {
    const V0 = parseFloat(document.getElementById('av0').value);
    const p = parseFloat(document.getElementById('ap').value) / 100;
    const d = parseFloat(document.getElementById('adonated').value) / 100;
    const it = parseFloat(document.getElementById('ait').value);

    document.getElementById('av0-value').innerText = V0.toLocaleString();
    document.getElementById('ap-value').innerText = (p * 100).toFixed(1);
    document.getElementById('adonated-value').innerText = (d * 100).toFixed(0);
    document.getElementById('ait-value').innerText = it.toLocaleString();

    let portfolio = V0;
    let donations = [];
    let totalDonation = 0;

    donations.push({ year: 0, donation: 0, portfolio: portfolio });
    for (let t = 1; t <= 10; t++) {
        const donation = portfolio * d;
        portfolio = portfolio * (1 + p) + it - donation;
        totalDonation += donation;
        donations.push({ year: t, donation: totalDonation, portfolio: portfolio });
    }

    updateChartA(donations);
}

function updateChartA(data) {
    const years = data.map(d => d.year);
    const portfolioValues = data.map(d => d.portfolio);
    const donations = data.map(d => d.donation);

    if (chartInstanceA) {
        chartInstanceA.data.labels = years;
        chartInstanceA.data.datasets[0].data = portfolioValues;
        chartInstanceA.data.datasets[1].data = donations;
        chartInstanceA.update();
    } else {
        const ctx = document.getElementById('achart').getContext('2d');
        chartInstanceA = new Chart(ctx, {
            type: 'line',
            data: {
                labels: years,
                datasets: [{
                    label: 'Portfolio Value',
                    data: portfolioValues,
                    borderColor: '#007BFF',
                    borderWidth: 2,
                    fill: false,
                    tension: 0.4
                }, {
                    label: 'Total Donated',
                    data: donations,
                    borderColor: '#28A745',
                    borderWidth: 2,
                    fill: false,
                    tension: 0.4
                }]
            },
            options: {
                responsive: true,
                scales: {
                    x: {
                        title: {
                            display: true,
                            text: 'Year'
                        }
                    },
                    y: {
                        title: {
                            display: true,
                            text: 'Amount ($)'
                        },
                        beginAtZero: false
                    }
                },
                plugins: {
                    legend: {
                        position: 'top',
                    },
                    tooltip: {
                        mode: 'index',
                        intersect: false
                    }
                }
            }
        });
    }
}

document.addEventListener('DOMContentLoaded', updateDonationsA);
</script>

## Framework for optimizing donations
Now we will build up a framework for understanding first how a portfolio evolves with donations and then how varying impact of individual donations can change how much to donate.
The core idea of the framework is to balance immediate charitable impact with the future growth of wealth.

<a href="#optimal-percentage-sim">Jump to simulation 1</a>


<a href="#risk-aversion-sim">Jump to simulation 2</a>

### Input variables
- **Current/initial portfolio value**: \\( V_0 \\) (your starting wealth)
- **Life expectancy**: \\( T \\) years (remaining years you expect to live)
- **Annual portfolio growth rate**: \\( p \\) (as a percentage)
- **Desired wealth at death**: \\( Y \\) (how much you want to leave behind, e.g., for family or legacy purposes)
- **Yearly net income**: \\( i_t \\) (your income after expenses in year \\( t \\))
- **Impact discount rate**: \\( d \\) (percentage decrease in the effectiveness of charitable donations over time due to compounding issues like inflation, diminishing needs, etc.)
- **Impact Function**: \\( I(t) \\) (a function of time that determines the relative impact of donations in year \\( t \\); by definition, \\( \sum_{t=1}^T I(t) = 1 \\)

### Output variable
The yearly amount to donate: \\( d_t \\).

### Building the framework

The model starts with the evolution of your wealth year over year. Here’s the basic equation:

$$V_t = p \ V_{t-1} + i_t - d_t$$

Where:
- \\( V_t \\) is your portfolio value at the end of year \\( t \\).
- \\( p \ V_{t-1} \\) represents the growth of your wealth from investments.
- \\( i_t \\) is your net income for year \\( t \\).
- \\( d_t \\) is the amount you donate in year \\( t \\).

We impose the terminal condition that you want to end with exactly your desired wealth \\( Y \\) at the end of your life (year \\( T \\)):

$$ V_T = Y $$

We can work recursively to express \\( d_t \\) as a function of the input variables. Expanding the wealth equation step by step:

1. Starting with year 1:
$$ V_1 = p \ V_0 + i_1 - d_1 $$

2. For year 2:
$$ V_2 = p \ V_1 + i_2 - d_2 $$
Substitute \\( V_1 \\):
$$ V_2 = p \ (p \ V_0 + i_1 - d_1) + i_2 - d_2 $$

Generalizing this, and using the terminal condition \\( V_T = Y \\), the portfolio value at year \\( T \\) becomes:

$$ V_T = Y = p^T \ V_0 + \sum_{t=1}^T p^{T-t} \ i_t - \sum_{t=1}^T p^{T-t} \ d_t $$

Rearranging for the summation of donations \\( d_t \\):
$$\sum_{t=1}^T p^{T-t} \ d_t = p^T \ V_0 + \sum_{t=1}^T p^{T-t} \ i_t - Y = k $$
where \\(k\\) is a constant.

Now, we have a constraint formula for donations as a function of the portfolio return rate, initial portfolio value, life expectancy, income, and desired end of life portfolio value.
Now, we want to see which \\(d_t\\) are available.
There are many \\(d_t\\) that satisfy the constraint above.
One such function is
$$
d_t =
\\begin{cases}
    0 & \\text{if } t < T \\\\
    p \ V_{T-1} + i_T - Y & \\text{if } t = T
\\end{cases}
$$
This is just donating everything at the end of your life up to \\(Y\\).
This is the way to maximize the total amount donated over your lifetime, assuming income and portfolio returns are positive.
However, we do not necessarily want to do this because we want to balance the total amount donated with immediate impact.


### Constant percentage donations

An intuitive way to determine donations is to use a constant percentage of our portfolio for donations.
So we have \\(d_t=u \ V_t\\) where \\(u\\) is the percent of our current portfolio to donate.
We can determine \\(u\\) using our constraint equation, so we get
$$\sum_{t=1}^T p^{T-t} \ d_t = \sum_{t=1}^T p^{T-t}\ u \ V_t = u\ \sum_{t=1}^T p^{T-t} \ V_t = k $$
so therefore
$$u = \frac{k}{ \sum_{t=1}^T p^{T-t} \ V_t} $$
and also
$$V_t = \frac{p \ V_{t-1} + i_t}{1+u}$$

We can numerically solve for \\(u\\) but finding a closed form is hard due to \\(V_t\\) dependence on \\(u\\).
Here is a simulation for finding the optimal fixed percentage based on the input variables.


<div class="widget-container-outer" id="optimal-percentage-sim">
<div class="widget-container-inner">
<h3 class="widget-title">Optimal percentage calculator</h3>
<label>Initial portfolio value (V₀): $<span id="v0-value">1000000</span></label>
<div class="slider-container">
    <span>1K</span>
    <input type="range" id="v0" min="1000" max="1000000" step="1000" value="1000000" oninput="updateDonations()">
    <span>1M</span>
</div>
<label>Time horizon (T years): <span id="t-value">10</span></label>
<div class="slider-container">
    <span>1</span>
    <input type="range" id="t" min="1" max="50" step="1" value="10" oninput="updateDonations()">
    <span>50</span>
</div>
<label>Annual portfolio growth rate (p%): <span id="p-value">8</span>%</label>
<div class="slider-container">
    <span>0%</span>
    <input type="range" id="p" min="0" max="30" step="0.1" value="8" oninput="updateDonations()">
    <span>30%</span>
</div>
<label>Annual income minus expenses (iₜ): $<span id="it-value">0</span></label>
<div class="slider-container">
    <span>0</span>
    <input type="range" id="it" min="0" max="500000" step="1000" value="0" oninput="updateDonations()">
    <span>500K</span>
</div>
<label>Desired wealth at horizon (Y): $<span id="y-value">1000000</span></label>
<div class="slider-container">
    <span>0</span>
    <input type="range" id="y" min="0" max="3000000" step="1000" value="1000000" oninput="updateDonations()">
    <span>3M</span>
</div>
<div class="chart-container">
    <canvas id="chart"></canvas>
</div>
<h3 class="output">
    Optimal annual donation: <span id="donation-output">0</span>
</h3>
</div>
</div>

<script>
let chartInstance = null;

function updateDonations() {
    const V0 = parseFloat(document.getElementById('v0').value);
    const T = parseInt(document.getElementById('t').value);
    const p = parseFloat(document.getElementById('p').value) / 100;
    const Y = parseFloat(document.getElementById('y').value);
    const it = parseFloat(document.getElementById('it').value);

    document.getElementById('v0-value').innerText = V0.toLocaleString();
    document.getElementById('t-value').innerText = T;
    document.getElementById('p-value').innerText = (p * 100).toFixed(1);
    document.getElementById('y-value').innerText = Y.toLocaleString();
    document.getElementById('it-value').innerText = it.toLocaleString();

    function solveForU(V0, p, it, T, Y) {
        const tolerance = 1e-6;
        const maxIterations = 100;
        let uLow = 0;
        let uHigh = 1;
        let u = (uLow + uHigh) / 2;

        for (let iteration = 0; iteration < maxIterations; iteration++) {
            let portfolio = V0;

            for (let t = 1; t <= T; t++) {
                const donation = u * portfolio;
                portfolio = portfolio * (1 + p) + it - donation;
            }

            const difference = portfolio - Y;

            if (Math.abs(difference) < tolerance) {
                return u;
            }

            if (difference > 0) {
                uLow = u;
            } else {
                uHigh = u;
            }

            u = (uLow + uHigh) / 2;
        }

        return u;
    }

    const u = solveForU(V0, p, it, T, Y);

    let portfolio = V0;
    let donations = [];
    let totalDonation = 0;

    donations.push({ year: 0, donation: 0, portfolio: portfolio });
    for (let t = 1; t <= T; t++) {
        const donation = u * portfolio;
        portfolio = portfolio * (1 + p) + it - donation;
        totalDonation += donation;
        donations.push({ year: t, donation: totalDonation, portfolio: portfolio });
    }

    document.getElementById('donation-output').innerText = (u * 100).toFixed(2) + '%';

    updateChart(donations);
}

function updateChart(data) {
    const years = data.map(d => d.year);
    const portfolioValues = data.map(d => d.portfolio);
    const donations = data.map(d => d.donation);

    if (chartInstance) {
        chartInstance.data.labels = years;
        chartInstance.data.datasets[0].data = portfolioValues;
        chartInstance.data.datasets[1].data = donations;
        chartInstance.update();
    } else {
        const ctx = document.getElementById('chart').getContext('2d');
        chartInstance = new Chart(ctx, {
            type: 'line',
            data: {
                labels: years,
                datasets: [{
                    label: 'Portfolio Value',
                    data: portfolioValues,
                    borderColor: '#007BFF',
                    borderWidth: 2,
                    fill: false,
                    tension: 0.4
                }, {
                    label: 'Total Donated',
                    data: donations,
                    borderColor: '#28A745',
                    borderWidth: 2,
                    fill: false,
                    tension: 0.4
                }]
            },
            options: {
                responsive: true,
                scales: {
                    x: {
                        title: {
                            display: true,
                            text: 'Year'
                        }
                    },
                    y: {
                        title: {
                            display: true,
                            text: 'Amount ($)'
                        },
                        beginAtZero: false
                    }
                },
                plugins: {
                    legend: {
                        position: 'top',
                    },
                    tooltip: {
                        mode: 'index',
                        intersect: false
                    }
                }
            }
        });
    }
}

document.addEventListener('DOMContentLoaded', updateDonations);

</script>


### Including impact

<a href="#risk-aversion-sim">Jump to simulation 2</a>

In reality, however, the personal impact of donations changes from year to year.
This is because you would like to donate to causes that are important to you which can change.
We will therefore try to maximize the net impact
$$
U = \sum_{t=1}^T {I(t)\ d_t}
$$

If \\(I(t)\\) is stochastic, then we are trying to maximize the expectation of \\(U\\).
Let's assume that \\(I(t) \sim \mathcal{N}(\mu, \sigma^2)\\).

If we simply maximize \\(U\\), then we will not donate money every year.
So instead, we maximize
$$U_{\text{risk-averse}} = \sum_{t=1}^T I(t)\ d_t - \gamma \cdot \sigma^2 \sum_{t=1}^T d_t^2$$
where \\(\gamma\\) is a constant.
This measure takes into account the variance of the utility.

Recalling we have the constraint
$$\sum_{t=1}^T p^{T-t} \ d_t = k,$$
in order to maximize \\(U_{\text{risk-averse}}\\), we write the Lagrangian
$$\mathcal{L} =\sum_{t=1}^T  I(t)\ d_t - \gamma \cdot \sigma^2 \sum_{t=1}^T d_t^2 + \lambda \left( k - \sum_{t=1}^T p^{T-t} \ d_t \right)$$

Then we calculate the partial derivatives and set them to zero:
$$\frac{\partial \mathcal{L}}{\partial d_t} = I(t) - 2 \gamma \ \sigma^2 \ d_t - \lambda \ p^{T-t} = 0$$

Solving for \\(d_t\\), we get
$$d_t = \frac{I(t) - \lambda \ p^{T-t}}{2 \gamma \ \sigma^2}$$

We then solve for \\(\lambda\\) by substituting the expression for \\(d_t\\) into the constraint
$$\sum_{t=1}^T p^{T-t} \ \frac{I(t) - \lambda \ p^{T-t}}{2 \gamma \ \sigma^2} = k,$$
which simplifies to
$$\lambda = \frac{\sum_{t=1}^T p^{T-t} \ I(t) - 2 \gamma \ \sigma^2 \ k}{\sum_{t=1}^T p^{2(T-t)}}$$

Now we have an expression for \\(d_t\\) in terms of the input variables!
We can simulate some impact functions and test what the results of this method are.

<div class="widget-container-outer" id="risk-aversion-sim">
<div class="widget-container-inner">
<h3 class="widget-title">Risk averse calculator</h3>
<label>Initial portfolio value (V₀): $<span id="v0-value-2">1000000</span></label>
<div class="slider-container">
    <span>1K</span>
    <input type="range" id="v0-2" min="1000" max="1000000" step="1000" value="1000000" oninput="updateDonations2()">
    <span>1M</span>
</div>
<label>Time horizon (T years): <span id="t-value-2">10</span></label>
<div class="slider-container">
    <span>1</span>
    <input type="range" id="t-2" min="1" max="50" step="1" value="10" oninput="updateDonations2()">
    <span>50</span>
</div>
<label>Annual portfolio growth rate (p%): <span id="p-value-2">8</span>%</label>
<div class="slider-container">
    <span>0%</span>
    <input type="range" id="p-2" min="0" max="30" step="0.1" value="8" oninput="updateDonations2()">
    <span>30%</span>
</div>
<label>Annual income minus expenses (iₜ): $<span id="it-value-2">0</span></label>
<div class="slider-container">
    <span>0</span>
    <input type="range" id="it-2" min="0" max="500000" step="1000" value="0" oninput="updateDonations2()">
    <span>500K</span>
</div>
<label>Desired wealth at horizon (Y): $<span id="y-value-2">1000000</span></label>
<div class="slider-container">
    <span>0</span>
    <input type="range" id="y-2" min="0" max="3000000" step="10000" value="1000000" oninput="updateDonations2()">
    <span>3M</span>
</div>
<label>Risk aversion parameter (γ): <span id="gamma-value-2">0.5</span></label>
<div class="slider-container">
    <span>0</span>
    <input type="range" id="gamma-2" min="0" max="1" step="0.01" value="0.5" oninput="updateDonations2()">
    <span>1</span>
</div>
<label>Impact standard deviation (σ): <span id="sigma-value-2">0.5</span></label>
<div class="slider-container">
    <span>0</span>
    <input type="range" id="sigma-2" min="0" max="1" step="0.01" value="0.5" oninput="updateDonations2()">
    <span>1</span>
</div>
<label>Impact in the first period (I(1)): <span id="i1-value-2">0.65</span></label>
<div class="slider-container">
    <span>0</span>
    <input type="range" id="i1-2" min="0" max="1" step="0.01" value="0.65" oninput="updateDonations2()">
    <span>1</span>
</div>
<label># Simulations: <span id="n-sims-value-2">5</span></label>
<div class="slider-container">
    <span>1</span>
    <input type="range" id="n-sims-2" min="1" max="100" step="1" value="5" oninput="updateDonations2()">
    <span>100</span>
</div>
<div class="chart-container">
    <canvas id="chart-2"></canvas>
</div>
<h3 class="output">
    First year donation: <span id="donation-output-2">0</span>
</h3>
</div>
</div>



<script>
let chartInstance2 = null;

function calculateDonations2(V0, p, it, T, Y, gamma, sigma, I1, mu, numSimulations) {
    gamma = gamma * 0.00005;
    const epsilon = 1e-6;

    // Helper function to generate a random normal variable
    function randomNormal(mean, stdDev) {
        let u = 1 - Math.random(); // Uniform(0,1) random number
        let v = 1 - Math.random();
        let z = Math.sqrt(-2.0 * Math.log(u)) * Math.cos(2.0 * Math.PI * v); // Box-Muller transform
        return z * stdDev + mean;
    }

    // Precompute terms that don't depend on t
    const portfolioTerm = Math.pow(p, T) * V0;
    const summationTerm = (Math.abs(p - 1) < epsilon) ? T : (1 - Math.pow(p, T)) / (1 - p);
    const partialSummationTerm = summationTerm - Math.pow(p, T - 1);
    const squaredSummationTerm = (Math.abs(p * p - 1) < epsilon)
        ? T
        : (1 - Math.pow(p * p, T)) / (1 - p * p);
    const incomeTerm = it * summationTerm;
    const k = portfolioTerm + incomeTerm - Y;
    const alpha = I1 * Math.pow(p, T - 1) + partialSummationTerm * mu;
    const lambda = (alpha - (2 * gamma * (sigma ** 2) * k)) / squaredSummationTerm;

    // Initialize arrays to store cumulative results
    let totalDonations = Array(T).fill(0);
    let totalPortfolioValues = Array(T).fill(0);
    let totalDonationAmounts = Array(T).fill(0);

    // Run simulations
    for (let sim = 0; sim < numSimulations; sim++) {
        let portfolio = V0;
        let portfolioValues = [];
        let donations = [];
        let totalDonationAmount = 0;

        for (let t = 1; t <= T; t++) {
            let I_t = t === 1 ? I1 : randomNormal(mu, sigma); // Adjusted for variable sigma
            let donation = Math.max(
                Math.min((I_t - lambda * Math.pow(p, T - t)) / (2 * gamma * (sigma ** 2)), portfolio),
                0
            );

            donations.push(donation);
            portfolio = portfolio * p + it - donation;

            totalDonationAmount += donation;
            portfolioValues.push(portfolio);

            // Accumulate results for averaging
            totalDonations[t - 1] += donation;
            totalPortfolioValues[t - 1] += portfolio;
            totalDonationAmounts[t - 1] += totalDonationAmount;
        }
    }

    // Compute averages
    const avgDonations = totalDonations.map(d => d / numSimulations);
    const avgPortfolioValues = totalPortfolioValues.map(v => v / numSimulations);
    const avgDonationAmounts = totalDonationAmounts.map(d => d / numSimulations);

    return [ avgDonations[0], avgDonations, avgPortfolioValues, avgDonationAmounts ];
}

function updateDonations2() {
    const V0 = parseFloat(document.getElementById('v0-2').value);
    const T = parseInt(document.getElementById('t-2').value);
    const p = parseFloat(document.getElementById('p-2').value) / 100 + 1;
    const Y = parseFloat(document.getElementById('y-2').value);
    const it = parseFloat(document.getElementById('it-2').value);
    const gamma = parseFloat(document.getElementById('gamma-2').value);
    const sigma = parseFloat(document.getElementById('sigma-2').value);
    const I1 = parseFloat(document.getElementById('i1-2').value);
    const numSimulations = parseFloat(document.getElementById('n-sims-2').value);
    const mu = 0.5;

    document.getElementById('v0-value-2').innerText = V0.toLocaleString();
    document.getElementById('t-value-2').innerText = T;
    document.getElementById('p-value-2').innerText = (p * 100 - 100).toFixed(1);
    document.getElementById('y-value-2').innerText = Y.toLocaleString();
    document.getElementById('it-value-2').innerText = it.toLocaleString();
    document.getElementById('gamma-value-2').innerText = gamma.toLocaleString();
    document.getElementById('sigma-value-2').innerText = sigma.toLocaleString();
    document.getElementById('i1-value-2').innerText = I1.toLocaleString();
    document.getElementById('n-sims-value-2').innerText = numSimulations;

    const [
        d1,
        donations,
        portfolioValues,
        totalDonated
    ] = calculateDonations2(V0, p, it, T, Y, gamma, sigma, I1, mu, numSimulations);

    document.getElementById('donation-output-2').innerText = '$' + donations[0].toLocaleString('en-US', { minimumFractionDigits: 2, maximumFractionDigits: 2 });

    updateChart2(portfolioValues, totalDonated, T);
}

function updateChart2(portfolioValues, totalDonated, T) {
    const years = Array.from({ length: T }, (_, t) => t + 1);

    if (chartInstance2) {
        chartInstance2.data.labels = years;
        chartInstance2.data.datasets[0].data = portfolioValues;
        chartInstance2.data.datasets[1].data = totalDonated;
        chartInstance2.update();
    } else {
        const ctx = document.getElementById('chart-2').getContext('2d');
        chartInstance2 = new Chart(ctx, {
            type: 'line',
            data: {
                labels: years,
                datasets: [{
                    label: 'Portfolio Value',
                    data: portfolioValues,
                    borderColor: '#007BFF',
                    borderWidth: 2,
                    fill: false,
                    tension: 0.4
                }, {
                    label: 'Total Donated',
                    data: totalDonated,
                    borderColor: '#28A745',
                    borderWidth: 2,
                    fill: false,
                    tension: 0.4
                }]
            },
            options: {
                responsive: true,
                scales: {
                    x: {
                        title: {
                            display: true,
                            text: 'Year'
                        }
                    },
                    y: {
                        title: {
                            display: true,
                            text: 'Amount ($)'
                        },
                        beginAtZero: false
                    }
                },
                plugins: {
                    legend: {
                        position: 'top',
                    },
                    tooltip: {
                        mode: 'index',
                        intersect: false
                    }
                }
            }
        });
    }
}

document.addEventListener('DOMContentLoaded', updateDonations2);

</script>



## Conclusion

In this article, we have looked at three frameworks for deciding how much to donate based on a changing portfolio:
1. Fixed percentage of portfolio or income each year based on religious rules.
2. Fixed percentage of portfolio each year based on desired portfolio value after \\(T\\) years.
3. Varying percentage of portfolio each year based on impact-weighted model.

These models have different degrees of complexity and parameters.
Hopefully by thinking about these models and looking at simulations, we can be more intentional about giving.



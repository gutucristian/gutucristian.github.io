This study guide is a collection of my notes for CS7646 @ Georgia Tech -- Machine Learning For Trading.

## Financial Time Series

Financial time series data is one of the most important types of data in finance. This is **data indexed by date and/or time**. For example, prices of stocks over time represent financial time series data.

## Changes over Time

Statistical analysis methods are often based on **changes over time** and **not the absolute values themselves**.

There are multiple options to calculate the changes in a time series over time, including:

- absolute differences

- percentage changes

- logarithmic (log) returns

From a statistics point of view, **absolute changes are not optimal** because they are dependent on the scale of the time series data itself. Percentage changes **normalize the change relative to the starting value**, allowing for meaningful comparisons regardless of the scale. Therefore, percentage changes are usually preferred.

As a side note, in mathematics, normalizing means adjusting values in a dataset or a mathematical object to bring them into a standard range, scale, or form.

Percentage change normalizes a value by expressing the change as a proportion of the original value, instead of as an absolute difference. This makes the change relative to the initial context, allowing for meaningful comparisons across values of different scales or magnitudes.

`Percentage change = ((New Value - Original Value) / Original Value) * 100`

This formula divides the absolute difference by the original value, transforming the raw change into a relative measure.

- By dividing the change `(New Value - Original Value)` by the original value, the change is scaled relative to the starting value.

- This ensures that larger values don’t dominate comparisons just because of their size.

As an alternative to percentage returns, log returns can be used. In some scenarios, they are easier to handle and therefore often preferred in a financial context.

A log return (also called logarithmic return or continuously compounded return) measures the rate of return of an asset using natural logarithms.

The formula for a log return is:

`Log Return = ln(P_t / P_t-1)`

Where:

- `P_t` = Price of the asset at time `t`.

- `P_t-1` = Price of the asset at the previous time period `(t-1)`.

- `ln` = Natural logarithm (logarithm with base `e`).

## Resampling

Resampling is an important operation on financial time series data. Usually this takes the form of downsampling, meaning that, for example, a tick data series is resampled to one-minute intervals or a time series with daily observations is resampled to one with weekly or monthly observations.

## Rolling Statistics

Rolling statistics involve calculating metrics like mean, standard deviation, or minimum over a moving time window within a dataset. For example, a rolling mean with a window size of 10 calculates the average of the last 10 data points and updates as new data becomes available.

Why Rolling Statistics Are Important in Finance

1. Trend Analysis:

- Rolling helps smooth out short-term fluctuations, helping identify underlying trends in stock prices or other financial metrics.

2. Volatility Measurement:

- Rolling standard deviation tracks changing volatility over time, aiding in risk assessment.

3. Dynamic Insights:

- They allow for time-varying analysis, reflecting how metrics evolve over a specific window rather than a fixed period.

4. Strategy Development:

- Used in trading strategies, such as comparing rolling short-term and long-term moving averages to generate buy/sell signals.

## Technical Analysis Example

Rolling statistics are a major tool in the so-called **technical analysis** of stocks.

A decades-old trading strategy based on technical analysis is using two simple moving averages (SMAs). The idea is that the trader should go long on a stock (or financial instrument in general) when the shorter-term SMA is above the longer-term SMA and should go short when the opposite holds true. 

Here's why this makes sense:

**Short-term SMA** reflects more recent price changes, making it **sensitive to the latest market movements**.

**Long-term SMA** smooths out fluctuations over a longer period, reflecting the **broader trend**.

When the short-term SMA crosses above the long-term SMA:

- It suggests that recent prices are rising faster than the long-term trend, signaling potential upward momentum (a bullish signal to “go long”).

When the short-term SMA crosses below the long-term SMA:

- It suggests that recent prices are falling compared to the long-term trend, signaling downward momentum (a bearish signal to “go short”).

**Example of the Strategy in Action**

- Short-term SMA: 10-day moving average.

- Long-term SMA: 50-day moving average.

- Buy Signal: The 10-day SMA crosses above the 50-day SMA.

- Sell Signal: The 10-day SMA crosses below the 50-day SMA.

This provides clear, rules-based guidance for entering and exiting trades.

**Limitations**

- **Lagging Indicator:** SMAs react after price movements, potentially causing delayed entry or exit signals.

- **False Signals:** During choppy or sideways markets, frequent crossovers can lead to false signals.

- **Doesn’t Predict Direction:** The strategy identifies trends but doesn’t guarantee their continuation.

# Machine Learning

The three major areas within Machine Learning are:
1. Supervised learning
2. Unsupervised learning
3. Reinforcement learning

A popular definition of Machine Learning (re-worded from Tom Mitchell):

A computer program is said to learn from experience `E` with respect to some class of tasks `T` if its performance as measured by `P` (at tasks `T`) improves with additional experience `E`.

## Artificial Intelligence (AI) vs Machine Learning (ML)

Think of AI as the goal (to replicate human intelligence) and ML as one of the tools to achieve that goal. Not all AI systems use ML, but most modern AI advancements heavily rely on ML.

## Supervised Learning

**What it is:** a ML technique where the algorithm is trained on a labeled dataset, meaning each input has a corresponding correct output. 

**The goal:** learn a relationship between inputs and outputs to make predictions on new, unseen data.

**Example:** 

- Predicting house prices (input: size, location; output: price).

- Classifying emails as “spam” or “not spam.”

**Common algorithms:** Linear Regression, Decision Trees, Neural Networks.

## Unsupervised Learning

**What it is:** a ML technique where the algorithm is given data without labeled outputs. It identifies patterns or groups in the data.

**Goal:** Explore the data to find hidden structures or clusters.

**Example:**

- Customer segmentation (grouping customers with similar behaviors).

- Identifying patterns in large datasets (like market trends).

**Common algorithms:** K-Means Clustering, Principal Component Analysis (PCA), DBSCAN.

## Reinforcement Learning

**What it is:** a ML technique where an _agent_ learns to make decisions by interacting with an environment, receiving rewards for good actions and penalties for bad ones.

**Goal:** maximize cumulative rewards over time.

**Example:**

- Teaching a robot to walk or play a game like chess or Go.

- Optimizing ad placements in marketing.

**Key concepts:**

- Agent: The learner.

- Environment: Where the agent operates.

- Reward: Feedback for actions.

**Common algorithms:** Q-Learning, Deep Q-Networks (DQN).

### Summary Table

|Type|Input|Output|Example|

|----|-----|------|-------|
|Supervised Learning|Labeled data|Predictions (future outputs)|Spam email detection|
|Unsupervised Learning|Unlabeled data|Clusters or patterns|Grouping customers|
|Reinforcement Learning|Feedback from environment|Sequence of actions to maximize reward|Training a robot to play chess|

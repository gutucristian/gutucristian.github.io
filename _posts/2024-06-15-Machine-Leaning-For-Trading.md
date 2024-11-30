This study guide is a collection of my notes for CS7646 @ Georgia Tech -- Machine Learning For Trading.

## Financial Time Series

Financial time series data is one of the most important types of data in finance. This is **data indexed by date and/or time**. For example, prices of stocks over time represent financial time series data.

## Changes over Time

Statistical analysis methods are often based on **changes over time** and **not the absolute values themselves**.

There are multiple options to calculate the changes in a time series over time, including absolute differences, percentage changes, and logarithmic (log) returns.

From a statistics point of view, **absolute changes are not optimal** because they are dependent on the scale of the time series data itself. Percentage changes **normalize the change relative to the starting value**, allowing for meaningful comparisons regardless of the scale. Therefore, percentage changes are usually preferred.

As a side note, in mathematics, normalizing means adjusting values in a dataset or a mathematical object to bring them into a standard range, scale, or form.

Percentage change normalizes a value by expressing the change as a proportion of the original value, instead of as an absolute difference. This makes the change relative to the initial context, allowing for meaningful comparisons across values of different scales or magnitudes.

`Percentage change = ((New Value - Original Value) / Original Value) * 100`

This formula divides the absolute difference by the original value, transforming the raw change into a relative measure.

- By dividing the change `(New Value - Original Value)` by the original value, the change is scaled relative to the starting value.

- This ensures that larger values don’t dominate comparisons just because of their size.


As an alternative to percentage returns, log returns can be used. In some scenarios, they are easier to handle and therefore often preferred in a financial context.

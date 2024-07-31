# Google Lightweight MMM Example

## Project Motivation

I embarked on this project because I realized the significant impact that observability over various strategies, such as ad campaigns, can have on business processes. By understanding how different media channels contribute to overall performance, businesses can optimize their budget allocation to maximize returns. This project allowed me to delve deeper into this area and explore how data-driven insights can inform and improve marketing strategies.

## Disclaimer

This notebook skips all exploratory data analysis and preprocessing (besides scaling) and assumes the user will do or has done it prior to engaging with this demo. The notebook uses dummy data, so the numbers and results might not be representative of what one might get on a real dataset.

## Organizing the Data for Modelling

### Data Splitting

The dataset is split into training and testing sets, with the last 13 weeks reserved for testing.

### Scaling

Scaling is essential for many modeling problems, and this one is no exception. We provide a custom scaler class, `CustomScaler`, which behaves similarly to `sklearn` scalers. Typically, you will need three or four scalers: one for media data, one for the target, and one for costs. If you are adding extra features, they might need an additional scaler. It's crucial to save and carry these scalers throughout your MMM journey, as LightweightMMM allows you to re-insert these scalers at different points to ensure everything is correctly scaled.

### CustomScaler Usage

This scaler can be used in two ways for both multiplication and division operations:
1. By specifying a value for the scaling operation.
2. By specifying an operation used at the column level to calculate the value for the actual scaling operation.

For example, to scale the dataset by multiplying by 100, you can directly pass `multiply_by=100`. Alternatively, if you want to multiply by the mean value of each column, you can pass `multiply_operation=jnp.mean` (or any other desired operation).

## Media Insights

### Media & Baseline Contribution Over Time

We can visualize the estimated media and baseline contribution over time. This helps in understanding how each media channel contributes to the overall performance and how the baseline (organic growth) evolves.

### Media Contributions with Credibility Intervals

Visualizing the estimated media contributions with their respective credibility intervals provides a clearer picture of the uncertainty around each estimate. This is crucial for making informed decisions.

### Media Channel Behavior

Another vital question we can solve with MMMs is how each media channel behaves individually as we invest more in it. We can plot the curve response of all media channels, which shows how changes in investment levels affect outcomes.

## Optimization

### Budget Allocation

The optimization process helps answer budget allocation questions. You need to specify the duration for which you want to optimize your budget (e.g., 15 weeks). The optimization values are bounded by ±20% of the maximum and minimum historic values used for training, preventing drastic changes in strategy and recommending budget re-allocation.

### Parameters for Optimization

- `bounds_lower_pct`
- `bounds_upper_pct`

These parameters can hold one value for all channels or one value per channel. The prices array should contain the average price expected for the media units of each channel. If your data is already in a monetary unit (e.g., dollars), your prices should be an array of ones. The budget is the total amount you want to allocate throughout the specified time period.

### Cost Considerations

If your media data is not in monetary units (e.g., impressions, clicks, GRPs), you need to store the cost per value (e.g., CPC) in the prices array and multiply it by `solution.x` to get the recommended budget allocation.

## Visualization of Results

We can plot the following:
1. Pre- and post-optimization budget allocation comparison for each channel.
2. Pre- and post-optimization predicted target variable comparison.

## Saving the Model

The model can be saved to disk for future use and analysis.

---

This project provides a comprehensive view of how media mix modeling can enhance the observability of marketing strategies and drive better business decisions. By understanding the contributions and behaviors of different media channels, businesses can allocate their budgets more effectively and achieve better outcomes.

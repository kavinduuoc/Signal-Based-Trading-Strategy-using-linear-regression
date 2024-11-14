# Signal-Based-Trading-Strategy-using-linear-regression

This code is implementing a signal-based trading strategy using linear regression to predict the future returns of the S&P 500 index (SPY) based on historical data from multiple stock market indices. Here's a breakdown of the steps involved:

 1. Data Preparation and Cleaning
The stock market data for several indices (U.S. markets: SP500, NASDAQ, DJI; European markets: DAXI, CAC40; Asian markets: Nikkei, HSI, All Ordinaries) are imported and processed. The returns (differences between the opening prices of consecutive days) are calculated for each index, and a new DataFrame (`indicepanel`) is created, which contains these returns along with the target variable, which is the return of SPY (`Price`).

 2. Data Splitting
The dataset is split into two parts:
- Training Set: The first 1000 rows (`-2000:-1000`)
- Test Set: The last 1000 rows (`-1000:`)

 3. Exploring the Data
A scatter matrix is generated to visualize the relationships among different stock indices, and the correlation between each index and SPY is checked. The correlation values are printed to help understand how each index is related to SPY.

 4. Linear Regression Model
A multiple linear regression model is created using `statsmodels.formula.api.ols`. The formula used is:

```python
formula = 'spy ~ spy_lag1 + sp500 + nasdaq + dji + cac40 + aord + daxi + nikkei + hsi'
```

The model tries to predict the SPY returns (`spy`) based on the returns of other indices and the previous day's SPY return (`spy_lag1`). The results of the regression show how well each predictor explains the variation in SPY returns.

 5. Making Predictions
The model is used to generate predictions on both the training and test datasets. The predicted values (`PredictedY`) are compared against the actual values to evaluate model performance visually (scatter plot of actual vs predicted values).

 6. Model Evaluation
The model is evaluated using the following metrics:
- Adjusted R²: Measures how well the model explains the variance in the target variable (SPY returns).
- RMSE (Root Mean Squared Error): Measures the average error between the predicted and actual values. A higher RMSE on the test set compared to the training set suggests that the model performs slightly worse on unseen data.

 7. Profit Calculation from Signal-Based Strategy
A trading strategy is implemented using the model's predictions. The strategy generates buy and sell signals:
- If the predicted SPY return is positive (`PredictedY > 0`), a buy signal is generated (1).
- If the predicted return is negative, a sell signal is generated (-1).

The profits from the strategy are calculated as the product of the predicted return (`spy`) and the signal. The wealth is calculated by taking the cumulative sum of the profits.

 8. Strategy Performance Visualization
The strategy's performance is visualized by plotting the cumulative wealth (profit) of the signal-based strategy against a simple buy-and-hold strategy (buying and holding SPY).

 9. Sharpe Ratio Calculation
The Sharpe ratio is calculated for both the training and test sets. It is a measure of the risk-adjusted return. The daily and yearly Sharpe ratios are calculated based on the logarithmic returns of the strategy's wealth.

 10. Maximum Drawdown (Not Fully Shown)
Maximum drawdown is another key metric that evaluates the largest peak-to-trough loss during the investment period. Although this metric isn't fully shown in the code provided, it can be calculated in a similar way to the Sharpe ratio.

 Summary of Results:
- Low R² and RMSE: The model explains only a small portion of the variation in SPY returns (around 5-6%), suggesting it may not capture the more complex relationships in stock market data. The relatively modest RMSE indicates the model isn't too far off but may not be highly accurate.
- Profit from Strategy: The strategy generated a total profit in both the training and test datasets, but this isn't necessarily a reflection of model accuracy—more complex strategies could potentially improve returns.
- Sharpe Ratio: The Sharpe ratios for both training and test sets help evaluate the risk-adjusted performance of the strategy.

In practice, this strategy could be used as a baseline, but further improvements may be necessary to better capture market trends and reduce noise. You could consider exploring more advanced models like time-series forecasting (ARIMA, LSTM) or machine learning techniques for better performance in such a noisy environment.

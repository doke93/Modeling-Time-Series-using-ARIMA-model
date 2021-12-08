

# Modeling-Time-Series-using-ARIMA-model


Exploiting different standard temporal structures seen in time series processes

## ARIMA vs Exponential Smoothing

- Exponential Smoothing is very specific(Linear trend, Seasonality)

- ARIMA imposes no such structure
- It is more "machine learning"-like (where you take a model and try to fit it to your data, whatever structure your data may have)




## Key Features
### 1. **Auto Regressive (AR)**:
- Regression of a time series process onto itself (use the past data points in the series to predict the next point)
- A time series process is AR if its present value depends on a linear combination of past observations.
- In financial time series, an AR model attempts to explain the mean reversion and trending behaviours that we observe in asset prices.

### 2. **Integrated (I)**:

- A time series process is integrated of order d, if differencing the observations d times, makes the process close to stationary(Does not change over time).

### 3. **Moving Average (MA)**:

-  A time series process is MA if its present value can be written as a linear combination of past error terms(part that we cannot predict).
- MA models try to capture the idiosyncratic shocks observed in financial markets.
- We can think of events like terrorist attacks, earnings surprises, sudden political changes, etc. 
  as the random shocks affecting the asset price movements.

## Special case - Random Walk
- ARIMA(0,1,0), this is random walk 
  because there is no auto Regressive part and no moving average part.
  All that we are left with is noise  
- If we fit an ARIMA on log return and find it to be random walk, this says that the return can't be predicted from previous values

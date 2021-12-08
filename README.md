
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

### Checking for stationarity

There are three ways of checking for stationarity in a time series.
1. Visual inspection
2. Statistical tests
3. ACF/PACF plots

### 3. **Moving Average (MA)**:

-  A time series process is MA if its present value can be written as a linear combination of past error terms(part that we cannot predict).
- MA models try to capture the idiosyncratic shocks observed in financial markets.
- We can think of events like terrorist attacks, earnings surprises, sudden political changes, etc. 
  as the random shocks affecting the asset price movements.

### Special case - Random Walk
- ARIMA(0,1,0), this is random walk 
  because there is no auto Regressive part and no moving average part.
  All that we are left with is noise  
- If we fit an ARIMA on log return and find it to be random walk, this says that the return can't be predicted from previous values

### Limitation
-  A well-known deficiency of ARIMA applications on financial time series is its failure to capture the phenomenon of volatility clustering.
-  However, despite their inaccurate point estimates, they give rise to informative confidence intervals.

### Roadmap
When we use the ARIMA class to model a time series process, each of the above components are specified in the model as parameters (with the notations p, d, and q respectively).

That is, the classification ARIMA(p, d, q) process can be thought of as AR(p)I(d)MA(q) 

Here,
1. p: The number of past observations (we usually call them *lagged terms*) of the process included in the model.

2. d: The number of times we difference the original process to make it stationary.

3. q: The number of past error terms (we usually call them *lagged error terms* or *lagged residuals*) of the process included in the model.


### Applying ARIMA model on  Netflix weekly adjusted close prices

![App Screenshot](https://user-images.githubusercontent.com/69301816/145212724-912441a9-97f9-4439-b402-5f41263e3f03.png)

First we will checks the stationarity of a pandas Series
- Using ADF test, plots and correlograms

![App Screenshot](https://user-images.githubusercontent.com/69301816/145214721-4036e3c5-97c6-4dfe-84a3-1dda5bee70f9.png)

![App Screenshot](https://user-images.githubusercontent.com/69301816/145215101-ffefcae0-220c-40e2-bd51-aeb9228c7c33.png)

Points to note:

- The p-value is nearly 1 (and equivalently the test statistic is greater than the critical values at all 3 significance levels). So the ADF test result is that the price series is non-stationary.
- The rolling means and volatility plots are time-varying. So we arrive at the same conclusion by examining the plots.
- From the ACF, there are significant autocorrelations above the 95% confidence interval at all lags. From the PACF, we have spikes at lags 1, 8, 9, 13, 18, 23 and 38.
## Fit model to the weekly stock prices 

We now fit an ARIMA model to the weekly stock prices (from mid-2010 to mid-2019) of `Netflix` and learn to evaluate it.


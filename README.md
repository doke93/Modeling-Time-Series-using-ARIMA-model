
# Modeling-Time-Series-using-ARIMA-model


- ARIMA model are pretty good at sales forecast, weather forecast and all kinds of time series data.
- In this project we will check whether it is possible to predict stock prices using ARIMA model.
- We will also learn about the fundamental characteristics of stock prices. 
## The Naive Forecast

- In time series analysis, the simplest baseline is the naive forecast
- What is Naive forecast ? 
  that is to copy the previous known value forward in time. 
- If the data follows a random walk, then the naive forecast is the **best forecast**.
- If our model cannot beat the naive forecast then we can conclude that our model is actually **worse than a random walk model**
- In other words, a random walk model describes the data better than our model.



## Key Features
### 1. **Auto Regressive (AR)**:
- Regression of a time series process onto itself (use the past data points in the series to predict the next point)
- A time series process is AR if its present value depends on a linear combination of past observations.
- In financial time series, an AR model attempts to explain the mean reversion and trending behaviours that we observe in asset prices.

### 2. **Integrated (I)**:

- A time series process is integrated of order d, if differencing the observations d times, makes the process close to stationary(Does not change over time).

### Checking for stationarity

There are three ways of checking for stationarity in a time series.
-  Visual inspection
-  Statistical tests
-  ACF/PACF plots

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

We will again check stationarity on log_returns
![App Screenshot](https://user-images.githubusercontent.com/69301816/145365930-d449a490-f951-4197-8432-d85480e5fd6b.png)
![App Screenshot](https://user-images.githubusercontent.com/69301816/145366008-9ec4b81f-61d0-44da-bd52-475619fa7f72.png)

Points to note:
- As per the ADF test results, the `Netflix` returns are stationary since the p-value is almost 0 and the test statistic is less than all the critical values.
- The returns and rolling means of the returns are all centred around 0. As the time scale increases, the means become more and more constant. At shorter time scales, the noise tends to obscure the signal.
- The volatily is time-varying at both the faster and slower rolling levels.
- We can see bristles near or beyond the blue shadow at lags 17 and 26 in the ACF plot and lags 12, 16, 17, 18 and 26 in the PACF plot.
- Returns are log price differences. So we can also infer from the above two checks, that the price series is integrated with order *1*
## Fit model to the weekly stock prices 

We now fit an ARIMA model to the weekly stock prices (from mid-2010 to mid-2019) of `Netflix` and learn to evaluate it.

While creating the ARIMA model class, we select the order arbitrarily (p and q) and we inferred d from the result of above stationarity check

![App Screenshot](https://user-images.githubusercontent.com/69301816/145384839-5b72e9fa-7581-4330-888e-80b3132c8598.png)

Points to note:
- We chose an ARIMA(3, 1, 2) model to fit the price series of `Netflix`. Equivalently, we could have fit an ARIMA(3, 0, 2) to the returns instead. 
- The `summary()` method provides the results of the model fitting exercise on the **in-sample data set** (a.k.a. the training data).
- The most important part is the table at the centre which has the coefficient values, their 95% confidence intervals and their corresponding p-values.
- However, we also need to run model diagnostics by examining the residual errors closely. This will tell us if our model was a good fit to the underlying data.

#### Diagnosing the residual

![App Screenshot](https://user-images.githubusercontent.com/69301816/145386607-d863fffa-31a6-4255-8c76-ff273e186290.png)

Points to note:
    
- `Standardized residuals`: The mean of the residuals is approximately zero. However, it's variance is much higher in the second half of the series.
- `Distribution of standardized residuals` and `Q-Q plot`: Both plots indicate fatter tails compared to a normal distribution.
- `ACF plot`: There seems to be serial correlations at lags 8, 13, 14, 22 and a few more. 
- **If the fit is good, we should see residuals similar to Gaussian white noise. It's not so here.**
- So we can infer that the model is not a very good fit.

### Additional Statistical tests

1. *To check for autocorrelations in residuals: [`The Ljung-Box test`](https://en.wikipedia.org/wiki/Ljung%E2%80%93Box_test)*

- The null hypothesis is that the serial correlations of the time series are zero. We use it in addition to visual interpretation of ACF/PACF plots.

2. *To check for normality in residuals: [`The Jarque-Bera test`](https://en.wikipedia.org/wiki/Jarque-Bera_test)*

- The null hypothesis is that the time series is normally distributed. We use it in addition to visual interpretation of plots like the residual distribution and the Q-Q plots.

![App Screenshot](https://user-images.githubusercontent.com/69301816/145392163-80007d25-8193-4ac8-ad4d-4d562c5bc81c.png)

Points to note:
    
- There are no significant serial correlations until lag 12.
- However, many of the correlations from lag 13 are below the red line.
- So our model is not a good fit.

The Jarque-Bera test

![App Screenshot](https://user-images.githubusercontent.com/69301816/145392710-a1723753-cde0-4f4b-ab77-84a995497ce2.png)



## Automatically finding the best ARIMA fit (using the pmdarima library)

Calling auto arima function and pass seasonal equals to false

```bash
  model = pm.auto_arima(train,
                     error_action='ignore', trace=True,
                     suppress_warnings=True, maxiter=10,
                     seasonal=False)
```
Model summary

![App Screenshot](https://user-images.githubusercontent.com/69301816/145430315-aa67f4f6-eb2a-4e4e-947f-a4065ec6d29f.png)

Points to note:
- We can see that best model found is AIRMA(3,1,2).
- The auto regressive component has order 3(last 3 days of data might be useful in predicting the next return)
- The intergrated component has order 1 and moving average component has order 2

Next we will create a function 
to plot the data itself, the fitted value for the in-sample data,
forecast for the out-of-sample data and the confidence bound

![App Screenshot](https://user-images.githubusercontent.com/69301816/145439679-264915cb-1423-4c53-8abd-b42a14218ff2.png)

Points to note:
- The fitted model looks pretty good
- It is too small to see our whether our forecast is good

Therefore we write a function to plot only the test period,
to see closely how well our model actually performs.

![App Screenshot](https://user-images.githubusercontent.com/69301816/145441076-cd8586e3-830f-47f3-ac66-5ffee3df37f8.png)

Points to note:
- We see that our forecast is not that good.
- It does seem to capture the average quite well.
- The true price is also within the confidence bounds 

So finally we will check, do this model perform better than the naive forecast?


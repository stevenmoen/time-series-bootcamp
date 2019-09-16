Introduction to Time Series Modeling
========================================================
author: Steven Moen
date: 9/12/19 - 9/13/19
autosize: true

Day 1 Agenda
========================================================

- Examples of time series data
- Key Tools
- Important Assumptions
- Autoregressive (AR) Models
- Moving-Average (MA) Models
- Autoregressive Moving-Average (ARMA) Models

Day 2 Agenda
========================================================

- Autoregressive Moving-Average (ARMA) Models (continued)
- Model Diagnostics
- Real Data Analysis
- Introduction to Frequency Domain

Special Thanks
========================================================

Many of the examples and theory used in this presentation can be found in Time Series Analysis and Its Applications by Robert H. Shumway and David S. Stoffer. 

Thanks to Jess and Rick with all their help putting this together.


My Background
========================================================

- Rice University '14
- 4 years of work experience at Capital One
- M.S. in Statistics, UChicago
- Currently working with Dr. Wei Biao Wu

Examples of Time Series Data 
========================================================

A plot of quarterly earnings per share from Johnson and Johnson, from Shumway and Stoffer.

![plot of chunk unnamed-chunk-1](Slides_day2-figure/unnamed-chunk-1-1.png)

Examples of Time Series Data
========================================================

A plot of global temperature deviations, also from Shumway and Stoffer.

![plot of chunk unnamed-chunk-2](Slides_day2-figure/unnamed-chunk-2-1.png)

Key Tools
========================================================

A common assumption in statistical modeling is independent and identically distributed noise, often abbreviated as i.i.d.

Two random variables $A$ and $B$ are independent if and only if the following is true:

$$
  \begin{aligned}
  \mathbb{P}(A \cap B) = \mathbb{P}(A)\mathbb{P}(B)
  \end{aligned}
$$

Why would this pose a problem when modeling time series?

Key Tools
========================================================

- Intuitively, someone who studies time series often cares about intertemporal dependencies. 
- Time series analysis incorporates several tools designed used to analyze the intertemporal relationships within a time series and the relationships between time series.

Key Tools
========================================================

What are some of these tools?

- Expected Value
- Autocovariance Function
- Autocorrelation Function
- Cross-Covariance Function
- Cross-Correlation Function

Key Tools: Expected Value
========================================================

The mean function of a time series is defined as follows:

$$
  \begin{aligned}
  \mu_{xt} = \mathbb{E}(x_t) = \int_{-\infty}^{\infty} x f_t(x)dx
  \end{aligned}
$$

where $\mathbb{E}$ is the expected value operator.

Aside: Random Walk with Drift Model
========================================================

For many of these exercises, we will work with a random walk with drift model, which is defined by:

$$
  \begin{aligned}
  x_t = \delta t + \sum_{j=1}^t w_j,
  t = 1,2,....
  \end{aligned}
$$

where $w_t$ is a white noise series defined by:

$$
  \begin{aligned}
  w_t \sim \text{wn}(0, \sigma_m^2)
  \end{aligned}
$$


Aside: White noise
========================================================

White noise can take on many different formulations. The below are ordered from least restrictive to most restrictive:

- A series of uncorrelated random variables with mean 0 and finite variance $\sigma_w^2$
- A series of i.i.d. random variables with zero mean
- Gaussian white noise, where the noise random variables are $w_t \overset{\text{i.i.d.}}{\sim} \mathcal{N}(0, \sigma_w^2)$

Aside: White noise
========================================================

According to Shumway and Stoffer, the designation "white" refers to the "analogy with white light and indicates all possible periodic oscillations are present with equal strength".


Aside: Random Walk with Drift Model
========================================================

This model is very useful in applications such as financial economics, since a random walk often accurately captures the seemingly "random" developments of market participants quickly incorporating relevant information into an asset's price. See "A Random Walk Down Wall Street".


Key Tools: Autocovariance
========================================================

The autocovariance function of a time series is defined as follows:

$$
  \begin{aligned}
  \gamma_x(s,t) = cov(x_s, x_t) = \mathbb{E}[(x_s - \mu_s)(x_t - \mu_t)]
  \end{aligned}
$$

for all $s$ and $t$. 

Key Tools: Autocovariance
========================================================

- According to Shumway and Stoffer, the "autocovariance measures the linear dependence between two points on the same series observed at different times"
- Remember, a covariance of 0 does NOT in general imply that two points in a series are independent


Exercise: Finding the Autocovariance of a Random Walk with Drift
========================================================

Compute the expectation of a random walk with drift model given in the previous slides and restated below:

$$
  \begin{aligned}
  x_t = \delta t + \sum_{j=1}^t w_j,
  t = 1,2,....
  \end{aligned}
$$

where $w_t$ is a white noise series defined by:

$$
  \begin{aligned}
  w_t \sim \text{wn}(0, \sigma_m^2)
  \end{aligned}
$$

Feel free to compute this using a pencil and paper, or using LaTeX (if you're familiar with it) in the TS_Exercises.Rmd file.

Solution: Finding the Autocovariance of a Random Walk with Drift
========================================================

The autocovariance of a random walk with drift is as follows:

$$
  \begin{aligned}
  \mathbb{E}(x_t) = \delta t
  \end{aligned}
$$

Please refer to the .Rmd file for a more in-depth solution.


Exercise: Finding the Autocovariance of a Random Walk with Drift
========================================================

Compute the autocovariance of a random walk with drift

$$
  \begin{aligned}
  x_t = \delta t + \sum_{j=1}^t w_j,
  t = 1,2,....
  \end{aligned}
$$

where $w_t$ is a white noise series defined by:

$$
  \begin{aligned}
  w_t \sim \text{wn}(0, \sigma_m^2)
  \end{aligned}
$$

Feel free to compute this using a pencil and paper, or using LaTeX (if you're familiar with it) in the TS_Exercises.Rmd file.

Solution: Finding the Expected Value of a Random Walk with Drift
========================================================

The expected value of a random walk with drift is as follows:

$$
  \begin{aligned}
  \gamma_x(s,t) = \min(s,t) \sigma_w^2
  \end{aligned}
$$

Please refer to the .Rmd file for a more in-depth solution.

Important Assumptions: Strong Stationarity
========================================================

The definition of a strictly stationary time series, according to Shumway and Stoffer, is that the "probabilistic behavior of every collection of values" $x_{t1}, x_{t2}, ..., x_{tk}$ "is identical to that of the shifted set" $x_{t1+h}, x_{t2+h}, ..., x_{tk+h}$.

- This assumption is not widely used in applications as it implies that the distribution must be constant at every value in a time series

Important Assumptions: Weak Stationarity
========================================================

According to Shumway and Stoffer, a weakly stationary series $x_t$ is given by the following:

- The mean value function, $\mu_t$ is constant and does not depend on the time
- The autocovariance function, $\gamma(s,t)$ depends on s and t only through their difference $|s-t|$

Important Assumptions: Verification (Exercise)
========================================================

Are the following time series weakly stationary?

1. $x_t = \delta t + \sum_{j=1}^t w_j, t = 1,2,....$
2. $v_t = \frac{1}{3}(w_{t-1} + w_t + w_{t+1})$

Important Assumptions: Verification (Exercise)
========================================================


1. $x_t = \delta t + \sum_{j=1}^t w_j, t = 1,2,....$
- No
2. $v_t = \frac{1}{3}(w_{t-1} + w_t + w_{t+1})$
- Yes

Please refer to the .Rmd file for a more in-depth solution.



Autoregressive Models
========================================================

We will get to real data analysis soon, but we need to define a few key models which we will use soon.

An autoregressive model of order p is defined as follows:

$$
x_t = \phi_1 x_{t-1} + \phi_2 x_{t-2} + ... + \phi_p x_{t-p} + w_t
$$

where:

- $x_t$ is stationary
- $w_t \sim wn(0, \sigma_w^2)$
- $\phi_1, ..., \phi_p$ are non-zero constants 
- $x_t$ has mean 0

Aside: Backshift Operator
========================================================

The backshift operator is defined as follows:

$$
B x_t = x_{t-1}
$$

and can be extended as follows:

$$
B^2 x_t = B(Bx_{t}) = Bx_{t-1} = x_{t-2}
$$

Autoregressive Operator
========================================================

Below is the autoregressive operator. It will be useful when we discuss ARMA models.

$$
\phi(B) = 1 - \phi_1 B - \phi_2 B^2 - ... - \phi_p B^p
$$

Using this, the initial AR model can be written as:

$$
(1 - \phi_1 B - \phi_2 B^2 - ... - \phi_p B^p)x_t = w_t
$$

or alternatively as:

$$
(\phi(B))x_t = w_t
$$

Example: An AR(1) Model
========================================================

An autoregressive model can be written as follows:

$$
x_t = \phi x_{t-1} + w_t = \phi(\phi x_{t-2} + w_{t-1}) + w_t
$$
$$
= \phi^2 x_{t-2} + \phi w_{t-1} + w_t
$$
$$
= \phi^k x_{t-k} + \sum_{j=0}^{k-1} \phi^j w_{t-j}
$$


Example: An AR(1) Model
========================================================

An autoregressive model can be written as follows:

$$
x_t = \phi x_{t-1} + w_t = \phi(\phi x_{t-2} + w_{t-1}) + w_t
$$
$$
= \phi^2 x_{t-2} + \phi w_{t-1} + w_t
$$
$$
= \phi^k x_{t-k} + \sum_{j=0}^{k-1} \phi^j w_{t-j}
$$

Thus, if $|\phi| < 1 \text{ and } \mathbb{V}(x_t) < \infty$ for all t, then we can represent the AR(1) as a linear process given by:

$$
x_t = \sum_{j=0}^\infty \phi^j w_{t-j}
$$

Moving Average Models
========================================================

A moving average model of order q is defined as follows:

$$
x_t =  w_t + \theta_1 w_{t-1} + \theta_2 w_{t-2} + ... + \theta_p w_{t-p}
$$

where:

- $x_t$ is stationary
- $w_t \sim wn(0, \sigma_w^2)$
- $\theta_1, ..., \theta_p, \theta_p \neq 0$ are parameters 
- $x_t$ has mean 0

Moving Average Operator
========================================================

The moving average operator is given by

$$
\theta(B) = 1 + \theta_1 B + \theta_2 B^2 + ... + \theta_q B^q
$$

which allows the model on the previous slide to be written as:

$$
x_t = \theta(B) w_t
$$


Autoregressive Moving Average Models
========================================================

An ARMA(p,q) time series is described below

$$
x_t = \phi_1 x_{t-1} + ... + \phi_p x_{t-p} + w_t + \theta_1 w_{t-1} + ... + \theta_q w_{t-q}
$$

where:

- $\phi_p \ne 0$
- $\phi_q \ne 0$
- $\sigma_w^2 > 0$
- The series is mean 0
- The series is stationary

Diagnostics
========================================================

We've defined some useful models, but how do we work with them? We need tools to balance out the bias-variance tradeoff.

- Let's look at the plot from The Elements of Statistical Learning on Page 37

Diagnostics
========================================================

There are three tools commonly used to measure the bias-variance tradeoff in time series

- AIC
- AICc
- BIC

Diagnostics - AIC
========================================================

The AIC is defined as follows:

$$
AIC = \log \hat{\sigma_k}^2 + \frac{n+2k}{n}
$$

- The AIC is biased, but people use it still because "there is no true model" in applications

Diagnostics - AIC
========================================================

The AICc is defined as follows:

$$
AICc = \log \hat{\sigma_k}^2 + \frac{n+k}{n-k-2}
$$

Diagnostics - BIC
========================================================

The BIC is defined as follows:

$$
BIC = \log \hat{\sigma_k}^2 + \frac{k \log n}{n}
$$

- The BIC tends to choose smaller models

Thanks!
========================================================

More to come tomorrow!

Today - Real Data Analysis
========================================================

Yesterday, we got through some mathematical foundations that will be useful before we get into analyzing real data.

Housekeeping
========================================================

Please fill out the survey!

Packages to install
- astsa
- tseries
- forecast
- lmtest
- MTS

Problems for Today
========================================================

Let's work through a problem together - Problem 3.32 in the textbook.


```r
library(astsa)
plot(oil)
```

![plot of chunk unnamed-chunk-3](Slides_day2-figure/unnamed-chunk-3-1.png)

Does this series look stationary?

Aside - Summary Statistics Can Be Deceptive
========================================================

In the study of statistics, we love numbers that summarize our data. Indeed, the very definition of a statistic is...

Aside - Summary Statistics Can Be Deceptive
========================================================

According to Webster's diction, one definition of a statistic is "a quantity (such as the mean of a sample) that is computed from a sample"

Why is this problematic? Enter Anscomb's quartet.

Aside - Visualizations Can Be Deceptive Too
========================================================

![plot of chunk unnamed-chunk-4](Slides_day2-figure/unnamed-chunk-4-1.png)

Which of the following lines is stationary?


Aside - Visualizations Can Be Deceptive Too
========================================================

Answer - the black line on the previous plot is stationary; the red line is not.

The moral of the story is that you need both summary statistics and visualization to avoid pitfalls.

ARIMA Model Cookbook
========================================================

Per Shumway and Stoffer, there are a steps which will generally allow you to fit a decent time series model.

- Plot the data
- Transform the data (if necessary)
- Identify dependence orders of the models
- Estimate the parameters
- Diagnostics
- Pick a final model

Problems for Today
========================================================


```r
library(astsa)
plot(oil)
```

![plot of chunk unnamed-chunk-5](Slides_day2-figure/unnamed-chunk-5-1.png)

As before, the data are pretty clearly non-stationary, but let's check.

Testing for Stationarity
========================================================


```r
library(tseries)
adf.test(oil)
```

```

	Augmented Dickey-Fuller Test

data:  oil
Dickey-Fuller = -3.4217, Lag order = 8, p-value = 0.04983
alternative hypothesis: stationary
```

Well, that's interesting. But that's why we plot the data. Let's turn the time series.

Transforming the Data
========================================================

Note that diff(log(SERIES)) is approximately equal to the percentage change, so we'll work with that


```r
# Percentage change approximation
pc_oil = diff(log(oil))
# Plot and test the data
plot(pc_oil)
```

![plot of chunk unnamed-chunk-7](Slides_day2-figure/unnamed-chunk-7-1.png)

```r
adf.test(pc_oil)
```

```

	Augmented Dickey-Fuller Test

data:  pc_oil
Dickey-Fuller = -6.3708, Lag order = 8, p-value = 0.01
alternative hypothesis: stationary
```

This looks a lot better!

How Do We Select the Order of an ARMA Model?
========================================================

See page 150 in Shumway and Stoffer.

Examining the ACF and PACF
========================================================


```r
acf2(pc_oil)
```

![plot of chunk unnamed-chunk-8](Slides_day2-figure/unnamed-chunk-8-1.png)

```
         ACF  PACF
  [1,]  0.13  0.13
  [2,] -0.07 -0.09
  [3,]  0.13  0.16
  [4,] -0.01 -0.06
  [5,]  0.02  0.05
  [6,] -0.03 -0.08
  [7,] -0.03  0.00
  [8,]  0.13  0.12
  [9,]  0.08  0.05
 [10,]  0.02  0.03
 [11,]  0.01 -0.02
 [12,]  0.00  0.00
 [13,] -0.02 -0.03
 [14,]  0.06  0.09
 [15,] -0.05 -0.07
 [16,] -0.09 -0.06
 [17,]  0.03  0.01
 [18,]  0.05  0.04
 [19,] -0.05 -0.05
 [20,] -0.07 -0.05
 [21,]  0.04  0.05
 [22,]  0.09  0.06
 [23,] -0.05 -0.06
 [24,] -0.08 -0.05
 [25,] -0.07 -0.08
 [26,]  0.00  0.02
 [27,] -0.11 -0.11
 [28,] -0.07  0.01
 [29,]  0.02  0.00
 [30,] -0.02 -0.01
 [31,] -0.03 -0.05
 [32,] -0.05 -0.04
 [33,] -0.03  0.02
 [34,]  0.00  0.02
 [35,] -0.09 -0.08
 [36,] -0.01  0.02
 [37,] -0.04 -0.04
 [38,] -0.01  0.04
 [39,]  0.02 -0.01
 [40,] -0.01 -0.01
 [41,] -0.06 -0.05
 [42,]  0.01  0.03
 [43,]  0.00 -0.03
 [44,] -0.01  0.00
 [45,]  0.04  0.08
 [46,]  0.01  0.00
 [47,]  0.05  0.05
 [48,]  0.07  0.01
 [49,] -0.01  0.04
 [50,] -0.03 -0.08
 [51,]  0.01  0.01
 [52,] -0.04 -0.07
 [53,] -0.04  0.00
 [54,] -0.03 -0.06
 [55,]  0.00  0.00
 [56,] -0.01 -0.06
 [57,] -0.10 -0.11
 [58,] -0.01  0.01
 [59,] -0.05 -0.09
 [60,] -0.04 -0.01
 [61,] -0.03 -0.04
 [62,]  0.01  0.04
 [63,]  0.01 -0.01
 [64,] -0.01  0.00
 [65,] -0.04 -0.04
 [66,]  0.02  0.03
 [67,]  0.00  0.00
 [68,] -0.01  0.00
 [69,] -0.03 -0.04
 [70,] -0.02 -0.02
 [71,] -0.05 -0.04
 [72,] -0.01  0.00
 [73,] -0.01 -0.01
 [74,] -0.02  0.00
 [75,]  0.01  0.02
 [76,]  0.02 -0.01
 [77,]  0.04  0.04
 [78,] -0.01 -0.02
 [79,]  0.03  0.08
 [80,]  0.02 -0.03
 [81,] -0.04 -0.03
 [82,] -0.01 -0.03
 [83,]  0.02  0.03
 [84,]  0.03 -0.03
 [85,]  0.01 -0.02
 [86,]  0.03  0.03
 [87,]  0.08  0.04
 [88,] -0.04 -0.09
 [89,] -0.02 -0.01
 [90,]  0.01 -0.02
 [91,] -0.04 -0.03
 [92,]  0.05  0.03
 [93,]  0.07  0.05
 [94,] -0.04 -0.11
 [95,]  0.02  0.02
 [96,]  0.05 -0.01
 [97,]  0.01  0.02
 [98,]  0.00 -0.03
 [99,]  0.01  0.06
[100,]  0.04  0.01
[101,]  0.01 -0.05
[102,] -0.03  0.02
[103,] -0.04 -0.03
[104,] -0.01  0.01
[105,]  0.02  0.00
[106,]  0.01  0.04
[107,]  0.01 -0.01
[108,]  0.06  0.07
[109,]  0.08  0.04
[110,]  0.04  0.04
[111,]  0.02  0.00
[112,]  0.01  0.05
[113,]  0.03 -0.01
[114,]  0.02  0.00
[115,] -0.02 -0.04
[116,] -0.04 -0.03
[117,] -0.01 -0.03
[118,]  0.04  0.02
[119,]  0.05  0.04
[120,] -0.02 -0.01
[121,] -0.02 -0.04
[122,]  0.03 -0.01
[123,]  0.01  0.03
[124,] -0.04 -0.03
[125,] -0.08 -0.07
[126,]  0.02  0.00
[127,]  0.00 -0.02
[128,] -0.04 -0.04
[129,]  0.01  0.01
[130,]  0.02  0.01
[131,]  0.01 -0.01
[132,]  0.02  0.02
[133,]  0.00  0.05
[134,] -0.01  0.02
[135,]  0.00  0.01
[136,] -0.03  0.02
[137,] -0.06 -0.02
[138,]  0.01  0.04
[139,] -0.02  0.01
[140,] -0.02 -0.03
[141,]  0.02 -0.02
[142,] -0.01  0.02
[143,] -0.03 -0.01
[144,]  0.00  0.02
[145,]  0.00 -0.01
[146,] -0.04  0.02
[147,] -0.01 -0.02
[148,] -0.02 -0.03
[149,] -0.04 -0.02
[150,] -0.04 -0.01
[151,]  0.01  0.02
[152,]  0.01 -0.01
[153,]  0.04  0.04
[154,]  0.03  0.03
[155,]  0.01 -0.04
[156,]  0.05  0.03
[157,]  0.01  0.00
[158,] -0.06 -0.05
[159,]  0.02  0.02
[160,]  0.05  0.03
[161,] -0.02  0.00
[162,]  0.05  0.03
[163,]  0.00  0.01
[164,] -0.01  0.00
[165,]  0.00  0.00
[166,] -0.01  0.02
[167,] -0.02  0.01
[168,] -0.01 -0.01
[169,]  0.00  0.03
[170,] -0.03 -0.03
[171,] -0.01  0.00
[172,] -0.02 -0.04
[173,] -0.02  0.00
[174,]  0.04  0.02
[175,] -0.01 -0.03
[176,] -0.03 -0.01
[177,]  0.02  0.01
[178,]  0.01  0.02
[179,] -0.01 -0.01
[180,] -0.01  0.00
[181,] -0.04 -0.04
[182,]  0.07  0.08
[183,] -0.01 -0.05
[184,] -0.04  0.02
[185,]  0.05 -0.01
[186,] -0.02 -0.02
[187,] -0.01  0.03
[188,]  0.01  0.00
[189,] -0.05 -0.03
[190,] -0.04 -0.04
[191,] -0.01  0.01
[192,]  0.01 -0.01
[193,]  0.04  0.07
[194,] -0.01 -0.01
[195,]  0.00  0.02
[196,]  0.06 -0.01
[197,] -0.06 -0.03
[198,] -0.02  0.01
[199,]  0.02  0.00
[200,]  0.00  0.00
[201,]  0.00 -0.02
[202,]  0.00 -0.01
[203,] -0.04 -0.05
[204,]  0.00 -0.01
[205,]  0.04  0.02
[206,]  0.04  0.04
[207,]  0.04  0.05
[208,]  0.01 -0.01
```

```r
acf(pc_oil, 24)
```

![plot of chunk unnamed-chunk-8](Slides_day2-figure/unnamed-chunk-8-2.png)

Suggests 1, 3, and 8 are possible MA lags.

Examining the ACF and PACF
========================================================


```r
pacf(pc_oil, 24)
```

![plot of chunk unnamed-chunk-9](Slides_day2-figure/unnamed-chunk-9-1.png)

Suggests 1, 2, 3, and 7 are possible AR lags.

Fitting Several Models
========================================================


```r
# q = 1
m11=arima(pc_oil,order=c(1,0,1))
m21=arima(pc_oil,order=c(2,0,1))
m31=arima(pc_oil,order=c(3,0,1))
m71=arima(pc_oil,order=c(7,0,1))
# q = 3
m13=arima(pc_oil,order=c(1,0,3))
m23=arima(pc_oil,order=c(2,0,3))
m33=arima(pc_oil,order=c(3,0,3))
m73=arima(pc_oil,order=c(7,0,3))
# q = 8
m18=arima(pc_oil,order=c(1,0,8))
m28=arima(pc_oil,order=c(2,0,8))
m38=arima(pc_oil,order=c(3,0,8))
m78=arima(pc_oil,order=c(7,0,8))

m2020=arima(pc_oil,order=c(20,0,20))
```


```r
# ?AIC
AIC(m11)
```

```
[1] -1801.787
```

```r
AIC(m21)
```

```
[1] -1800.651
```

```r
AIC(m31)
```

```
[1] -1803.509
```

```r
AIC(m71)
```

```
[1] -1804.878
```

```r
AIC(m13)
```

```
[1] -1803.398
```

```r
AIC(m23)
```

```
[1] -1801.682
```

```r
AIC(m33)
```

```
[1] -1804.418
```

```r
AIC(m73)
```

```
[1] -1802.482
```

```r
AIC(m18)
```

```
[1] -1808.155
```

```r
AIC(m28)
```

```
[1] -1806.204
```

```r
AIC(m38)
```

```
[1] -1804.216
```

```r
AIC(m78)
```

```
[1] -1805.724
```

ARMA(1,8)


```r
BIC(m11)
```

```
[1] -1784.592
```

```r
BIC(m21)
```

```
[1] -1779.156
```

```r
BIC(m31)
```

```
[1] -1777.715
```

```r
BIC(m71)
```

```
[1] -1761.889
```

```r
BIC(m13)
```

```
[1] -1777.605
```

```r
BIC(m23)
```

```
[1] -1771.589
```

```r
BIC(m33)
```

```
[1] -1770.026
```

```r
BIC(m73)
```

```
[1] -1750.895
```

```r
BIC(m18)
```

```
[1] -1760.867
```

```r
BIC(m28)
```

```
[1] -1754.616
```

```r
BIC(m38)
```

```
[1] -1748.33
```

```r
BIC(m78)
```

```
[1] -1732.642
```

ARMA(1,1)


```r
tsdiag(m18)
```

![plot of chunk unnamed-chunk-13](Slides_day2-figure/unnamed-chunk-13-1.png)

```r
tsdiag(m11)
```

![plot of chunk unnamed-chunk-13](Slides_day2-figure/unnamed-chunk-13-2.png)


```r
m18
```

```

Call:
arima(x = pc_oil, order = c(1, 0, 8))

Coefficients:
         ar1      ma1      ma2     ma3      ma4     ma5      ma6      ma7
      0.4018  -0.2315  -0.1655  0.1903  -0.0665  0.0084  -0.0338  -0.0361
s.e.  0.2684   0.2662   0.0629  0.0519   0.0585  0.0455   0.0451   0.0433
         ma8  intercept
      0.1648     0.0017
s.e.  0.0431     0.0027

sigma^2 estimated as 0.002024:  log likelihood = 915.08,  aic = -1808.15
```


Fitting Several Models
========================================================


```r
# AR Models
m71=arima(pc_oil,order=c(7,0,1))
m73=arima(pc_oil,order=c(7,0,3))
m78=arima(pc_oil,order=c(7,0,8))

# m71
# m73
m78
```

```

Call:
arima(x = pc_oil, order = c(7, 0, 8))

Coefficients:
         ar1     ar2    ar3      ar4      ar5     ar6      ar7     ma1
      0.0628  0.2432  0.262  -0.0965  -0.7619  0.1809  -0.1198  0.1225
s.e.  0.1691  0.2031  0.106   0.1038   0.1122  0.1617   0.1866  0.1626
          ma2      ma3     ma4     ma5      ma6     ma7     ma8  intercept
      -0.3498  -0.1529  0.0731  0.7803  -0.1202  0.0065  0.2902     0.0017
s.e.   0.1952   0.1430  0.1065  0.1300   0.1802  0.1859  0.0739     0.0026

sigma^2 estimated as 0.001971:  log likelihood = 919.86,  aic = -1805.72
```

It looks like an ARMA(7,8) is a decent model.


```r
tsdiag(m78)
```

![plot of chunk unnamed-chunk-16](Slides_day2-figure/unnamed-chunk-16-1.png)



Other Problems for Today
========================================================

All in Shumway and Stoffer.

- Question 2.1
- Question 2.3
- Question 2.8
- Question 2.10
- Question 3.33 (All But Forecast)
- Question 3.34 (All But Prediction Intervals)


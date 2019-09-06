Introduction to Time Series Modeling
========================================================
author: Steven Moen
date: 9/12/19 - 9/13/19
autosize: true

Day 1 Agenda
========================================================

- Examples of time series data
- Key Assumptions and Tools
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

Many of the examples used in this presentation can be found in Time Series Analysis and Its Applications by Robert H. Shumway and David S. Stoffer.



Examples of Time Series Data 
========================================================

This example is from Shumway and Stoffer.

![plot of chunk unnamed-chunk-1](Slides-figure/unnamed-chunk-1-1.png)

Examples of Time Series Data
========================================================

Also from Shumway and Stoffer.

![plot of chunk unnamed-chunk-2](Slides-figure/unnamed-chunk-2-1.png)

Examples of Time Series Data
========================================================

Also from Shumway and Stoffer.

![plot of chunk unnamed-chunk-3](Slides-figure/unnamed-chunk-3-1.png)

Key Assumptions and Tools
========================================================

A common assumption in statistical modeling is independent and identically distributed noise, often abbreviated (i.i.d.).

Two random variables $A$ and $B$ are independent if and only if the following is true:

$$
  \begin{aligned}
  \mathbb{P}(A \cap B) = \mathbb{P}(A)\mathbb{P}(B)
  \end{aligned}
$$

Why would this pose a problem when modeling time series?

Key Assumptions and Tools
========================================================


Exercise
========================================================




Slide With Plot
========================================================

![plot of chunk unnamed-chunk-4](Slides-figure/unnamed-chunk-4-1.png)

Slide with Equations

========================================================


Did you know that $2 + 2 = 4$?

Special Thanks
========================================================


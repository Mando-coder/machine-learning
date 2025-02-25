# Evaluating Regression

It is important to evaluate how well a prediction model approximates a set of data. This section focuses on regression models, dealing with continuous features. 

## Accuracy of Fit
The accuracy of the fit can be measured by a metric describing the accuracy of the model as a whole for a given data set. 

### Mean Absolute Error (MAE)
As the name suggests the *Mean Absolute Error* takes the absolute value of the residual (error) and averages it out over the entire data set of length $n$

$$ MAE = \frac{1}{n} \sum_{i=1}^{n} |y_i - \hat y_i| $$
								

|Pro|Con|
|-|-|
|Easy to understand/implement|All errors are treated equally|
|Error units same as data||

### Mean Squared Error (MSE)
Larger errors are punished more than with MAE by squaring the residual (error)

$$ MSE = \frac{1}{n} \sum_{i=1}^{n} (y_i - \hat y_i)^2 $$

|Pro|Con|
|-|-|
|Punishes larger errors more than smaller errors|MSE gives an error in different units than the original data (unintuitive to know how far off you are)|

### Root Mean Squared Error (RMSE)
This punishes the larger errors more than the smaller errors, while keeping the units of the error the same as the units of the data. This is analogous to reporting the standard deviation instead of the variance. 

$$ RMSE = \sqrt{\frac{1}{n} \sum_{i=1}^{n} (y_i - \hat y_i)^2} $$


|Pro|Con|
|-|-|
|Punishes larger errors more than smaller errors|Slightly more computationally expensive (negligible)|
|Error same units as data||					

## Accuracy of Coefficients

When performing a linear regression analysis we are trying to approximate $y$ with $\hat y$. Normally the true relationship $f(X)$ (in case of linear regression this is known as the population regression line) is not known. 

 
However we can simulate a data set with a known population regression line $f(X)$ by generating random values of $X$ and using them to calculate $y$. 

$$ y = f(X) + \epsilon $$
$$ f(X) = 2 + 3X $$
$$\hat y = \beta_0 + \beta_1 X$$


<p align="center">
  <img src="../../images/coefficient_accuracy.png" alt="coefficient_accuracy" width="800px"/>
</p>

**Black Dots** measurements from the data set $y$  
<span style="color:#fa1c20">**Red Line**</span> 	true relationship (population regression) $f(X)$  
<span style="color:#6824ff">**Blue-Purple Line**</span>	linear regression fit (least squares line) $\hat y$ based on the black dots data set  
<span style="color:#9dd2fa">**Light Blue Lines**</span>	linear least square lines $\hat y$ based on separately generated data sets

In the graph above it can be seen that the prediction $\hat y$ (<span style="color:#6824ff">**Blue-Purple Line**</span>) is slightly different than the true relationship $f(X)$ (<span style="color:#fa1c20">**Red Line**</span>). 

However if different data sets based on y are generated and linear regression is applied, then it can be seen that the average values of the light blue lines $\hat y$ approximate the true value $f(X)$ quite well. 


### Null Hypothesis
When evaluating a model, the first question one should ask themselves is: *"is there a relationship between $X$ and $y$*? This question can be answered by setting up two hypotheses. In the case of ordinary linear regression the relationship between $X$ and $y$ is governed by $\beta_1$, the hypotheses can therefore be formulated as:

**Null Hypothesis**

> $H_0$ : There is **no** relationship between $X$ and $y$ $\rightarrow \beta_1 = 0$

**Alternative Hypothesis** 
> $H_a$ : There is **some** relationship between $X$ and $y$ $\rightarrow \beta_1 \neq 0$

The first step in evaluating a model is rejecting the null hypothesis (i.e. ensuring a relationship exists between $X$ and $y$)

### t-statistic

### p-value

### F-statistic

### Estimation Bias
Consider a random variable $Y$ with a true mean value of $μ$. If we take a set of measurements, we can compute the sample mean $\hat μ$ in order to estimate the true mean $μ$. Depending on the measurements we will sometimes overestimate the true mean, and sometimes underestimate the true mean, but if we repeat the process enough times and take the average of the sample means, it will converge to the true mean value. 

An estimate is **biased** if the estimate systematically over or underestimates the true value

An estimate is **unbiased**  if the estimate (on average) approximates the true value 

In the case of the simulated data from before it can be seen that the estimated coefficients are unbiased, as the estimates (light blue lines) approximate the true values quite well


### Residual Standard Error (RSE)
Going back to the analogy of the mean value $\mu$ of a random variable $Y$, it is of interest to determine the accuracy of the sample mean $\hat μ$; how far off do we expect the sample mean $\hat μ$  to be from the true mean $\mu$
 
Assuming the $n$ observations are uncorrelated, this question is answered through the standard error of the sample mean $SE(\hat μ)$. This shows us that the standard error of the sample mean decreases with the number of observations $n$

$$ \text{Var}(\hat μ)=\text{Var}(\frac{1}{n} \sum_{i=1}^n Y_i)= \frac{1}{n^2}  \text{Var}(\sum_{i=1}^n Y_i) =\frac{1}{n^2} \sum_{i=1}^n \sigma^2 =\frac{σ^2}{n} $$

$$ SE(\hat μ)= \sqrt{Var(\hat μ)}= \frac{\sigma}{\sqrt{n}} $$

Similarly we can determine the standard error of the estimate coefficients $β_0$  and $β_1$ for the ordinary linear regression case.

$$ \text{SE}(\beta_0) ^2  = \sigma^2 [\frac{1}{n} + \frac{\bar x^2}{\sum (x_i - \bar x)^2}]$$

$$ \text{SE}(\beta_1) ^2 = \frac{\sigma^2}{\sum (x_i - \bar x)^2}$$



In general, $\sigma$ is not known, but can be estimated by  Residual Standard Error (RSE) which depends on the RSS

$$ \sigma \approx RSE = \sqrt{\frac{RSS}{n-2}} =\sqrt{\frac{\sum(y_i - \hat y_i)^2}{n-2}} $$



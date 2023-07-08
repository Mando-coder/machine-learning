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
  <img src="../images/coefficient_accuracy.png" alt="adaboost_visualization" width="800px"/>
</p>

**Black Dots** measurements from the data set $y$  
<span style="color:#fa1c20">**Red Line**</span> 	true relationship (population regression) $f(X)$  
<span style="color:#6824ff">**Blue-Purple Line**</span>	linear regression fit (least squares line) $\hat y$ based on the black dots data set  
<span style="color:#9dd2fa">**Light Blue Lines**</span>	linear least square lines $\hat y$ based on separately generated data sets

In the graph above it can be seen that the prediction $\hat y$ (<span style="color:#6824ff">**$\hat y$**</span>) is slightly different than the true relationship $f(X)$ (<span style="color:#fa1c20">**Red Line**</span>). 

However if different data sets based on y are generated and linear regression is applied, then it can be seen that the average values of the light blue lines y ̂  approximate the true value f(X) quite well. 
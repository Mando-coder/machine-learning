# Logistic Regression
Logistic regression is a simple classification model based on the logistic function. 

## Logistic Function
### Generalized Logistic Function
Logistic regression transforms a Linear Regression into a classification model through the use of the logistic function. The generalized logistic function is in the form of:

$$ \sigma(x) = \frac{L}{1+e^{-k(x-x_0)}}$$

**Where**  
$L$ = Height of the logistic function  
$k$ = Steepness of the logistic function  
$x_0$ = Location of the logistic function  

<p align="center">
  <img src="images/logistic_function.png" alt="logistic_function" width="800px"/>
</p>

### Probability Logistic Function
We can use a specific version of the generalized logistic function to describe the probability of something occurring. In this case the value of $L$ will be equal to 1 as the maximum of the value should be 1/1. 

$$ p(x) = P(X = x) = \frac{1}{1+e^{-(\beta_0 + \beta_1 x)}} $$

**Where**  
$\beta_0$ = Shifts the logistic function sideways  
$\beta_1$ = Increases the steepness of the logistic function

<p align="center">
  <img src="images/probability_logistic_function.png" alt="probability logistic_function" width="800px"/>
</p>


## Binary Logistic Regression
The following data set shows the balance & income of the credit card holders, and whether or not they defaulted on their payments. There seems to be a strong correlation between the credit card holder's balance and whether or not they will default (center plot below).

<p align="center">
  <img src="images/binary_logistic_regression_data.png" alt="probability logistic_function" width="800px"/>
</p>

If linear regression is applied (left plot below) then the prediction model will perform quite poorly. However using the logistic function it is possible to predict the probability of default given a certain balance (right plot below).

$$ p(X) = P(y = 1  | x = X) = \frac{1}{1+e^{-(\beta_0 + \beta_1 X)}}$$

**Where**  
$y$ = default on payment (0 = "no", 1 = "yes")  
$X$ = credit card balance

<p align="center">
  <img src="images/linear_vs_logistic_regression.png" alt="probability logistic_function" width="800px"/>
</p>

### Odds
In betting the term odds is often used, which from a statistics point of view represents the ratio of the probability of an event occurring to the probability of an event not occurring.

$$ odds = \frac{p}{1-p} $$ 

If this is applied to the [probability logistic function](#probability-logistic-function) then a linear relationship can be obtained between the feature X and the natural log of the odds (a.k.a. log odds or logit)
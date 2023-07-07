# Boosting

Similar to bagging, boosting is a general approach that may be applied to various statistical models for both classification and regression. 

Bagging and random forest methods utilize many different versions of the same data set, sampled in different ways, where trees of similar complexity fitted on these subsets of the dataset are combined in order to give the final prediction. 

The boosting method does not use subsets, but utilizes the entire data set. It combines trees of increasing complexity until the desired degree of accuracy is obtained. 


## Gradient Boosting
Gradient boosting can be used on any model, but is often used for decision trees. You start by predicting $y$ using a simple model $\hat y_0$, which will result in a set of residuals $r_1$ 

$$ y \approx \hat y_0 + r_1$$


In order to improve the accuracy of the predictions, an additional model $\hat r_1$ can be trained which uses the data $X$ to try and predict the residuals $r_1$. However the predictions $\hat r_1$ of $r_1$ wil not be perfect, leading to a set of residuals $r_2$

$$y \approx \hat y_0 + \hat r_1 + r_2$$


This process can be repeated until the residual is sufficiently small. 


$$ y \approx \hat y_0 + \sum_n \hat r_n$$

In order to converge more quickly a learning-rate parameter $\lambda$ is introduced, which takes a value between 0 and 1. In a more generalized form the prediction of $y$ is given by $y_m$ which consists of $m$ weak models $\hat h_m$. 

$$ \hat y_m = \sum_{n=1}^m \lambda \hat h_n$$

$$ \hat y_m = \hat y_{m-1} +\lambda \hat h_m$$

The complexity of each weak learner $\hat h$ can be controlled. For tree based methods this parameter is $d$ and controls the number of splits ($d$ + 1 leaf nodes). This controls the interaction depth of the model. Often $d$ = 1 will work just fine, which is simple a tree stump (one root node and two leaf nodes). 


## Adaptive Boosting
Adaptive Boosting (AdaBoosting) is a meta-model. It uses a series of weak learners (simplistic models) and combines them through a weighted sum. Each new model in the sum is based on the previous tree in the sum, making the model “adaptive”. 

It is similar to gradient boosting in the sense that it is a linear sum of weak models. However with Gradient Boosting the learning rate $\lambda$ is constant, while with AdaBoosting the learning rate $\lambda_m$ depends on the error of the previous model. 



$$ \hat y_m = \sum_{n=1}^{m} \lambda_n \hat h_n $$

$$ \hat y_m = \hat y_{m-1} + \lambda_m \hat h_m $$



The learning rate $\lambda_m$ of the $m^{th}$ weak learner $\hat h_m$, is chosen such that the resulting error $E_m$ is minimized. By using a exponential loss function, it can be [shown](#loss-function---classification) that this can be achieved by using the *misclassified* data points of the previous iteration of the total model $\hat y_{m-1}$. 

<p align="center">
  <img src="images/adaboost_visualization.png" alt="adaboost_visualization" width="800px"/>
</p>





### Loss Function - Classification

When training the next model in the linear combination the previous linear combination is taken and an additional weak learner is added

$$ \hat y_m = \hat y_{m-1} + \lambda_m \hat h_m $$

<!-- $$ \hat y_{m-1} = \sum_{n=1}^{m-1} \lambda_n \hat h_n$$ -->

In order to strongly punish incorrect classifications, an [exponential loss function](https://en.wikipedia.org/wiki/Loss_functions_for_classification) is used in the algorithm. For a prediction model $\hat y_m$ which has $m$ sub-models, the loss function is given by


$$ E_m = \sum_i e^{-y_i \hat y_{m,i}} $$

$$ E_m = \sum_i e^{-y_i \cdot (\hat y_{m-1, i} + \lambda_m \hat h_{m,i})} = \sum_{i} e^{-y_i \cdot \hat y_{m-1,i}} \cdot e^{-y_i \cdot \lambda_m \hat h_m} $$

The parameter $\lambda_m$ is chosen such that, after training the weak learner $\hat h_m$, the error $E_m$ is minimized. As the first exponent is independent of parameter $\lambda_m$, it can be denoted as a "weight" $w_{m,i}$

$$ w_{m,i} =  e^{-y_i \cdot \hat y_{m-1, i}} $$

$$ E_m = \sum_{i} w_{m,i} \cdot e^{-y_i \cdot \lambda_m \hat h_m} $$

The summation can be split into two, one for data points that are correctly classified ($y_i = \hat h_{m,i}$), and one for data points that re incorrectly classified ($y_i \neq \hat h_{m, i}$). For correct classifications $y_i$ and $h_{m,i}$ will always have the same sign, resulting in a value of +1. Similarly for incorrect classifications always results in a value of -1. 

$$ E_m = \sum_{i; y_i = \hat h_{m,i}} w_{m,i} \cdot e^{- \lambda_m} + \sum_{i;y_i \neq \hat h_{m,i}} w_{m,i} \cdot e^{\lambda_m} $$

The exponents are now independent of $i$ and therefore no longer contribute to the summations. The summations now represent the sum of weights for the correctly and incorrectly (not-correct) classified predictions. 

$$ E_m = W_{m,c}\cdot e^{-\lambda_m} + W_{m,nc} \cdot e^{\lambda_m}$$


This expression can be rewritten

$$  e^{\lambda_m}E_m = W_{m,c}\cdot + W_{m,nc} \cdot e^{2\lambda_m}$$

$$  e^{\lambda_m}E_m = W_{m,c}\cdot + W_{m,nc}  + W_{m,nc} (e^{2\lambda_m} -1)$$

$$  e^{\lambda_m}E_m = W_m  + W_{m,nc} (e^{2\lambda_m} -1)$$

The value of the parameter $\lambda_m$ can be found by minimizing the  total error $E_m$, which can be done by setting the partial derrivative equal to zero. 

$$ \frac{\partial E_m}{\partial \lambda_m} =0$$

This results in a values for $\lambda_m$ of

$$ \lambda_m = \frac{1}{2} \ln (\frac{W_m - W_{m,nc}}{W_{m,nc}})$$

$$ \lambda_m = \frac{1}{2} \ln (\frac{1-\epsilon_m}{\epsilon_m})$$

$$ \epsilon_m = \frac{W_{m, nc}}{W_m} $$

The parameter $\lambda_m$ is therefore governed by the ratio of the sum of incorrect weights $W_{m,nc}$ to the sum of all the weights $W_m$. Here the weights represent the error terms of the previous ($m$ - 1) model

$$ W_m = \sum_i w_{m,i} =\sum_i e^{-y_i \cdot \hat y_{m-1, i}} = E_{m-1} $$

 


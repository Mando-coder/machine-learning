# Feature Scaling
Motivation
In the prediction model of linear regression each feature $X_i$ of a dataset has a weight/coefficient $i$ attached to it. 

$$y=\beta_0+\beta_1X_1+\beta_2X_2+ ... + \beta_pX_p$$
 
When these features $X_i$ have different scales (orders of magnitude), the gradient descent method may update certain weights $i$ slower than others. Feature scaling aims to improve the convergence of steepest descent algorithms, and is an absolute necessity for certain algorithms. 

|Pros|Cons|
|--|--|
|Allows features $X_i$ of different units to be compared more easily| New data used for prediction has to be scaled as well|
Allows direct comparison of coefficient $i$| Harder to relate the results back to the original unscaled data |
Improves performance of steepest descent algorithms| |

## Types of Feature Scaling

There are different ways of scaling the data of the various features, the most common are Standardization and Normalization

### Standardization
This scales all the features according to a normal standard distribution, which means the scaled data has a mean $\mu_{scaled}$ = 0  and a standard derivative of $\sigma_{scaled}$ = 1
 
$$ X_{scaled} = \frac{X-\mu}{\sigma}$$


### Normalization
This normalizes the values of the data such that all values are between 0 and 1
 
$$X_{scaled} = \frac{X-X_{min}}{X_{max}-X_{min}}$$


## Scikit-learn
When applying machine learning algorithms using the scikit-learn library, the model objects contain a fit and transform function.

### Fit
The <code>model.fit()</code> method calculates useful statistics of the data ($X_{min}$, $X_{max}$, $\mu$, $\sigma$) necessary later on the analysis. The final prediction is not allowed to "know" anything about the test data, so only the training data should be "fit".

### Transform
The <code>model.transform()</code> method changes the data: calculate higher order features for Polynomial Regression, or scales the data for feature scaling
 
In order for the model to work - for the input to match the output - the features of both the training and test data needs to be scaled using the  <code>model.transform()</code> method.

### Data Leakage
If you call the <code>model.fit()</code> method, information from the test set will "leak" information into the training set. The best practice is to follow the following steps
* Perform train-test-split
* Apply <code>model.fit()</code> to training data
* Apply <code>model.transform()</code> to training data
* Apply <code>model.transform()</code> to test data
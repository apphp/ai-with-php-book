# Gradient Descent

## Gradient Descent in Linear Regression

We know that in any machine learning project our main aim relies on how good our project accuracy is or how much our model prediction differs from the actual data point. Based on the difference between model prediction and actual data points we try to find the parameters of the model which give better accuracy on our dataset\\, In order to find these parameters we apply gradient descent on the cost function of the machine learning model.&#x20;

### What is Gradient Descent

Gradient Descent is an iterative optimization algorithm that tries to find the optimum value (Minimum/Maximum) of an objective function. It is one of the most used optimization techniques in machine learning projects for updating the parameters of a model in order to minimize a cost function. &#x20;

The main aim of gradient descent is to find the best parameters of a model which gives the highest accuracy on training as well as testing datasets. In gradient descent, The gradient is a vector that points in the direction of the steepest increase of the function at a specific point. Moving in the opposite direction of the gradient allows the algorithm to gradually descend towards lower values of the function, and eventually reaching to the minimum of the function.

#### Steps Required in Gradient Descent Algorithm&#x20;

* **Step 1** we first initialize the parameters of the model randomly&#x20;
* **Step 2** Compute the gradient of the cost function with respect to each parameter. It involves making partial differentiation of cost function with respect to the parameters.&#x20;
* **Step 3** Update the parameters of the model by taking steps in the opposite direction of the model. Here we choose a hyperparameter learning rate which is denoted by alpha. It helps in deciding the step size of the gradient.&#x20;
* **Step 4** Repeat steps 2 and 3 iteratively to get the best parameter for the defined model&#x20;

#### Pseudocode for Gradient Descent&#x20;

```
t ← 0
max_iterations ← 1000
w, b ← initialize randomly

while t < max_iterations do
    t ← t + 1
    w_t+1 ← w_t − η ∇w_t
    b_t+1 ← b_t − η ∇b_t
end
```

> Here    max\_iterations is the number of iteration we want to do to update our parameter&#x20;
>
> W,b are the weights and bias parameter&#x20;
>
> η is the learning parameter also denoted by alpha &#x20;

To apply this gradient descent on data using any programming language we have to make four new functions using which we can update our parameter and apply it to data to make a prediction. We will see each function one by one and understand it&#x20;

1. &#x20;**gradient\_descent –** In the gradient descent function we will make the prediction on a dataset and compute the difference between the predicted and actual target value and accordingly we will update the parameter and hence it will return the updated parameter.
2. **compute\_predictions –** In this function, we will compute the prediction using the parameters at each iteration.
3. **compute\_gradient –** In this function we will compute the error which is the difference between the actual and predicted target value and then compute the gradient using this error and training data.&#x20;
4. **update\_parameters –** In this separate function we will update the parameter using learning rate and gradient that we got from the compute\_gradient function.&#x20;

```
function gradient_descent(X, y, learning_rate, num_iterations):
    Initialize parameters  = θ
    for iter in range(num_iterations):
        predictions = compute_predictions(X, θ)
        gradient = compute_gradient(X, y, predictions)
        update_parameters(θ, gradient, learning_rate)
    return θ

function compute_predictions(X, θ):
    return X*θ

function compute_gradient(X, y, predictions):
    error = predictions - y
    gradient = Xᵀ * error / m
    return gradient

function update_parameters(θ, gradient, learning_rate):
    θ = θ - learning_rate ⨉ gradient
```

### Mathematics Behind Gradient Descent&#x20;

In the Machine Learning Regression problem, our model targets to get the best-fit regression line to predict the value y based on the given input value (x). While training the model, the model calculates the cost function like Root Mean Squared error between the predicted value (pred) and true value (y). Our model targets to minimize this cost function. \
To minimize this cost function, the model needs to have the best value of θ1 and θ2(for Univariate linear regression problem). Initially model selects θ1 and θ2 values randomly and then iteratively update these value in order to minimize the cost function until it reaches the minimum. By the time model achieves the minimum cost function, it will have the best θ1 and θ2 values. Using these updated values of θ1 and θ2 in the hypothesis equation of linear equation, our model will predict the output value y. &#x20;

#### **How do θ1 and θ2 values get updated?** &#x20;

**Linear Regression Cost Function:** J(θ)=12m∑i=1m(hθ(x(i))–y(i))2J(θ)=2m1​∑i=1m​(hθ​(x(i))–y(i))2

so our model aim is to Minimize  \frac{1}{2m} \sum\_{i=1}^{m} (h\_\theta(x^{(i)}) – y^{(i)})^2  and store the parameters which makes it minimum.&#x20;

#### **Gradient Descent Algorithm For Linear Regression**&#x20;

![Gradient descent algorithm for linear regression](https://media.geeksforgeeks.org/wp-content/uploads/Cost-Function.jpg)

Gradient descent algorithm for linear regression&#x20;

<pre><code><strong>-> θj     : Weights of the hypothesis.
</strong><strong>-> hθ(xi) : predicted y value for ith input.
</strong><strong>-> i     : Feature index number (can be 0, 1, 2, ......, n).
</strong><strong>-> α     : Learning Rate of Gradient Descent.
</strong></code></pre>

#### How Does Gradient Descent Work &#x20;

Gradient descent works by moving downward toward the pits or valleys in the graph to find the minimum value. This is achieved by taking the derivative of the cost function, as illustrated in the figure below. During each iteration, gradient descent step-downs the [cost function](https://www.geeksforgeeks.org/ml-cost-function-in-logistic-regression/) in the direction of the steepest descent. By adjusting the parameters in this direction, it seeks to reach the minimum of the cost function and find the best-fit values for the parameters. The size of each step is determined by parameter **α** known as **Learning Rate**. \
In the Gradient Descent algorithm, one can infer two points :&#x20;

* **If slope is +ve** : θj = θj – (+ve value). Hence the value of θj decreases.

![If slope is +ve in Gradient Descent](https://media.geeksforgeeks.org/wp-content/uploads/theta-decrease.jpg)

If slope is +ve in Gradient Descent&#x20;

* **If slope is -ve** : θj = θj – (-ve value). Hence the value of θj increases.

![If slope is -ve in Gradient Descent](https://media.geeksforgeeks.org/wp-content/uploads/theta-increase.jpg)

If slope is -ve in Gradient Descent&#x20;

#### How To Choose Learning Rate&#x20;

The choice of correct learning rate is very important as it ensures that Gradient Descent converges in a reasonable time. :&#x20;

* If we choose **α to be very large**, Gradient Descent can overshoot the minimum. It may fail to converge or even diverge. \
  &#x20;

![Effect of large alpha value on Gradient Descent](https://media.geeksforgeeks.org/wp-content/uploads/big-learning.jpg)

Effect of large alpha value on Gradient Descent&#x20;

* If we choose α to be very small, Gradient Descent will take small steps to reach local minima and will take a longer time to reach minima. \
  &#x20;

![](https://media.geeksforgeeks.org/wp-content/uploads/small-learning.jpg)

Effect of small alpha value on Gradient Descent&#x20;

### &#x20; TODO: implement in PHP PHP Implementation of Gradient Descent&#x20;

\
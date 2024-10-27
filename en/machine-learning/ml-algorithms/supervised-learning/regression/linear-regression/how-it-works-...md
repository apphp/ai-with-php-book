# How It Works ?..

### **How Linear Regression Works**

Linear regression aims to find the best-fitting line, or **regression line**, that minimizes the difference between the actual and predicted values. For a single independent variable, the relationship is represented as:

$$y=β_{0} + β_{1​}x_{1} + ϵ$$

Where:

* y is the dependent variable (the outcome we want to predict),
* x 1 ​ ,x 2 ​ ,…,x n ​ is the independent variable (input feature),
* β0​ is the intercept (the predicted value when x is zero),
* β1​ is the slope of the line (the rate of change in y for each unit change in x),
* ϵ is the error term, capturing the variability in y that cannot be explained by y.

When there are multiple input features, the model becomes **Multiple Linear Regression**:

y=β 0 ​ +β 1 ​ x 1 ​ +β 2 ​ x 2 ​ +⋯+β n ​ x n ​ +ϵ

Where:

* y is the predicted output,
* x 1 ​ ,x 2 ​ ,…,x n ​ are input features,
* β0 is the intercept (constant term),
* β 1 ​ ,β 2 ​ ,…,β n​ are coefficients (weights),
* ϵ is the error term.

The goal is to estimate the coefficients β 0 ​ ,β 1 ​ ,…,β n ​​ to minimize the difference between predicted and actual values. This difference is typically minimized by **Ordinary Least Squares (OLS)**, which finds the coefficients that minimize the **sum of squared residuals**.

#### **Assumptions of Linear Regression**

Linear regression relies on several key assumptions for optimal performance:

1. **Linearity**: The relationship between independent and dependent variables is linear.
2. **Independence**: Observations are independent of each other.
3. **Homoscedasticity**: The variance of residual errors is constant across all levels of the independent variables.
4. **Normality**: Residuals (errors) are normally distributed.
5. **No Multicollinearity** (in multiple linear regression): Independent variables should not be highly correlated with each other.

Violating these assumptions can lead to inaccurate predictions, so it’s essential to check them before finalizing a model.

#### **Types of Linear Regression**

1. **Simple Linear Regression**: Used when there is only one independent variable. For example, predicting a person’s weight based on their height.
2. **Multiple Linear Regression**: Involves two or more independent variables, such as predicting house prices based on factors like area, number of rooms, and location.
3. **Polynomial Regression**: An extension where the relationship between variables is non-linear. Polynomial regression transforms input variables to higher powers (e.g., x2,x3x^2, x^3x2,x3) but remains a linear model concerning the parameters, making it suitable for more complex patterns.
4. **Regularized Linear Regression**: Techniques like **Lasso** (Least Absolute Shrinkage and Selection Operator) and **Ridge Regression** add penalties to the model for higher coefficients, reducing overfitting and improving model generalization.

**Strengths and Limitations of Linear Regression**

**Strengths**:

* **Interpretability**: The linear model provides a straightforward understanding of the relationships between variables.
* **Speed and Efficiency**: Linear regression is computationally efficient, making it suitable for large datasets.
* **Foundation for More Complex Models**: It serves as a baseline model, often used before deploying more complex approaches.

**Limitations**:

* **Sensitivity to Outliers**: Linear regression can be heavily influenced by extreme values, which may skew results.
* **Assumes Linear Relationships**: It may perform poorly if the true relationship between variables is non-linear.
* **Overfitting in Multiple Linear Regression**: When many features are included, the model can overfit, capturing noise rather than the actual pattern.

TODO: add charts

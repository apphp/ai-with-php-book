# Types of Linear Regression ?..

There are two main types of linear regression and two additional types, Let's take a look at them.

### **1. Simple Linear Regression**

Used when there is only one independent variable. For example, predicting a person’s weight based on their height.

This is the simplest form of linear regression, and it involves only one independent variable and one dependent variable. The equation for simple linear regression is: $$y=β_{0} ​ +β _{1} ​*X$$

where:

* Y is the dependent variable
* X is the independent variable
* β0 is the intercept
* β1 is the slope

**Example:**

<div align="left">

<figure><img src="../../../../../.gitbook/assets/image (122).png" alt="" width="563"><figcaption><p>Linear Regression</p></figcaption></figure>

</div>

<div align="left">

<figure><img src="../../../../../.gitbook/assets/image (125).png" alt="" width="563"><figcaption><p>Simple Linear Regression</p></figcaption></figure>

</div>

### **2. Multiple Linear Regression**

Involves two or more independent variables, such as predicting house prices based on factors like area, number of rooms, and location.

This involves more than one independent variable and one dependent variable. The equation for multiple linear regression is:\
y=β0+β1X1+β2X2+………βnXny=β0​+β1​X1+β2​X2+………βn​Xn\
where:

* Y is the dependent variable
* X1, X2, …, Xn are the independent variables
* β0 is the intercept
* β1, β2, …, βn are the slopes



<div align="left">

<figure><img src="../../../../../.gitbook/assets/image (123).png" alt="" width="563"><figcaption></figcaption></figure>

</div>

<div align="left">

<figure><img src="../../../../../.gitbook/assets/image (5).png" alt=""><figcaption></figcaption></figure>

</div>

### **3. Polynomial Regression**

An extension where the relationship between variables is non-linear. Polynomial regression transforms input variables to higher powers (e.g., x2,x3x^2, x^3x2,x3) but remains a linear model concerning the parameters, making it suitable for more complex patterns.

In polynomial regression, we aim to model a non-linear relationship by transforming the input variable  x  to include higher powers. The model equation for a polynomial regression of degree  d  is:

$$y = \beta_0 + \beta_1 x + \beta_2 x^2 + \beta_3 x^3 + \dots + \beta_d x^d + \epsilon$$\
\
where:

• y  is the dependent variable,

• $$\beta_0, \beta_1, \beta_2, \dots, \beta_d$$ are the coefficients,

• $$x, x^2, x^3, \dots, x^d$$ represent the transformed input features up to the  d -th degree,

• $$\epsilon$$  is the error term.

\
This transformation allows the model to fit a curve that better matches non-linear patterns in the data.

**Example:**

<div align="left">

<figure><img src="../../../../../.gitbook/assets/image (120).png" alt="" width="563"><figcaption><p>Polynomial Regression</p></figcaption></figure>

</div>

### **4. Regularized Linear Regression**

Techniques like **Lasso** (Least Absolute Shrinkage and Selection Operator) and **Ridge Regression** add penalties to the model for higher coefficients, reducing overfitting and improving model generalization.

Regularization introduces a penalty term to the linear regression cost function, helping prevent overfitting by constraining large coefficients.

\
1\. Lasso Regression (Least Absolute Shrinkage and Selection Operator)

In Lasso regression, the penalty added is proportional to the absolute value of each coefficient. The cost function to minimize is:

$$J(\beta) = \sum_{i=1}^{n} (y_i - \hat{y_i})^2 + \lambda \sum_{j=1}^{p} |\beta_j|$$\
\
where:

• $$J(\beta)$$  is the cost function,

• y\_i  is the actual value,

• \hat{y\_i}  is the predicted value,

• \lambda  is the regularization parameter controlling the penalty’s strength,

• \beta\_j  represents the coefficients of each feature.

The  L1 -norm penalty ( \sum\_{j=1}^{p} |\beta\_j| ) often results in some coefficients being reduced to zero, effectively performing feature selection.

<figure><img src="../../../../../.gitbook/assets/image (2) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>



2\. Ridge Regression

Ridge regression uses an  L2 -norm penalty, where the sum of the squared coefficients is added to the cost function:

$$J(\beta) = \sum_{i=1}^{n} (y_i - \hat{y_i})^2 + \lambda \sum_{j=1}^{p} \beta_j^2$$\
\
Here:

• $$\sum_{j=1}^{p} \beta_j^2$$  is the  L2 -norm, which penalizes large coefficients more smoothly than Lasso.

This penalty tends to reduce the impact of all coefficients but doesn’t force any to zero, making Ridge regression useful when all features are considered potentially valuable.\


<figure><img src="../../../../../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

These formulas are essential for understanding how polynomial and regularized linear regressions adjust the model’s complexity and enhance its ability to generalize across diverse datasets.




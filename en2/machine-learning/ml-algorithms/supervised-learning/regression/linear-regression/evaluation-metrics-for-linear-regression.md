# Evaluation Metrics for Linear Regression

A variety of evaluation measures can be used to determine the strength of any linear regression model. These assessment metrics often give an indication of how well the model is producing the observed outputs.

The most common measurements are:

#### Mean Square Error (MSE)

Mean Squared Error (MSE) is an evaluation metric that calculates the average of the squared differences between the actual and predicted values for all the data points. The difference is squared to ensure that negative and positive differences don’t cancel each other out.\
\
$$MSE = \frac{1}{n} \sum_{i=1}^{n} (y_i - \hat{y}_i)^2$$

Here,

* n is the number of data points.
* yi is the actual or observed value for the ith data point.
* yi^yi​​ is the predicted value for the ith data point.

MSE is a way to quantify the accuracy of a model’s predictions. MSE is sensitive to outliers as large errors contribute significantly to the overall score.

#### Mean Absolute Error (MAE)

Mean Absolute Error is an evaluation metric used to calculate the accuracy of a regression model. MAE measures the average absolute difference between the predicted values and actual values.

Mathematically, MAE is expressed as:

MAE=1n∑i=1n∣Yi–Yi^∣MAE=n1​∑i=1n​∣Yi​–Yi​​∣

Here,

* n is the number of observations
* Yi represents the actual values.
* Yi^Yi​​ represents the predicted values

Lower MAE value indicates better model performance. It is not sensitive to the outliers as we consider absolute differences.

#### **Root Mean Squared Error (RMSE)**

The square root of the residuals’ variance is the Root Mean Squared Error. It describes how well the observed data points match the expected values, or the model’s absolute fit to the data.

\
In mathematical notation, it can be expressed as:\
RMSE=RSSn=∑i=2n(yiactual−yipredicted)2nRMSE=nRSS​​=n∑i=2n​(yiactual​−yipredicted​)2​​\
Rather than dividing the entire number of data points in the model by the number of degrees of freedom, one must divide the sum of the squared residuals to obtain an unbiased estimate. Then, this figure is referred to as the Residual Standard Error (RSE).

In mathematical notation, it can be expressed as:\
RMSE=RSSn=∑i=2n(yiactual−yipredicted)2(n−2)RMSE=nRSS​​=(n−2)∑i=2n​(yiactual​−yipredicted​)2​​\


RSME is not as good of a metric as R-squared. Root Mean Squared Error can fluctuate when the units of the variables vary since its value is dependent on the variables’ units (it is not a normalized measure).

#### Coefficient of Determination (R-squared)

R-Squared is a statistic that indicates how much variation the developed model can explain or capture. It is always in the range of 0 to 1. In general, the better the model matches the data, the greater the R-squared number.\
In mathematical notation, it can be expressed as:\
R2=1−(RSSTSS)R2=1−(TSSRSS​)

* **Residual sum of Squares (RSS): The** sum of squares of the residual for each data point in the plot or data is known as the residual sum of squares, or RSS. It is a measurement of the difference between the output that was observed and what was anticipated.\
  RSS=∑i=2n(yi−b0−b1xi)2RSS=∑i=2n​(yi​−b0​−b1​xi​)2
* **Total Sum of Squares (TSS):** The sum of the data points’ errors from the answer variable’s mean is known as the total sum of squares, or TSS.\
  TSS=∑(y−yi‾)2TSS=∑​(y−yi​​)2

R squared metric is a measure of the proportion of variance in the dependent variable that is explained the independent variables in the model.

#### Adjusted R-Squared Error

Adjusted R2 measures the proportion of variance in the dependent variable that is explained by independent variables in a regression model. [Adjusted R-square](https://www.geeksforgeeks.org/ml-adjusted-r-square-in-regression-analysis/) accounts the number of predictors in the model and penalizes the model for including irrelevant predictors that don’t contribute significantly to explain the variance in the dependent variables.

Mathematically, adjusted R2 is expressed as:

AdjustedR2=1–((1−R2).(n−1)n−k−1)AdjustedR2=1–(n−k−1(1−R2).(n−1)​)

Here,

* n is the number of observations
* k is the number of predictors in the model
* R2 is coeeficient of determination

Adjusted R-square helps to prevent overfitting. It penalizes the model with additional predictors that do not contribute significantly to explain the variance in the dependent variable.


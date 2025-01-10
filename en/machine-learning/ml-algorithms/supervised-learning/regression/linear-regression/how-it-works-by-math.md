# How It Works by Math

## Mathematical explanation for Linear Regression working



Suppose we are given a dataset:

| Experience (X) | Salary (y) (in lakhs) |
| -------------- | --------------------- |
| 2              | 3                     |
| 6              | 10                    |
| 5              | 4                     |
| 7              | 13                    |

Given is a Work vs Experience dataset of a company and the task is to predict the salary of a employee based on his / her work experience. \
This article aims to explain how in reality Linear regression mathematically works when we use a pre-defined function to perform prediction task. \
Let us explore **how the stuff works when Linear Regression algorithm gets trained.**&#x20;

**Iteration 1** – In the start, $$θ_0$$ and $$θ_1$$  values are randomly chosen. Let us suppose, $$θ_0 = 0$$  and $$θ_1 = 0$$.&#x20;

* **Predicted values after iteration 1 with Linear regression hypothesis.**  $$h_\theta =  \begin{bmatrix} \theta_0 & \theta_1 \end{bmatrix} \begin{bmatrix} x_0 & x_0 & x_0 & x_0 \\ x_1 & x_2 & x_3 & x_4 \end{bmatrix}$$ \
  \
  $$=  \begin{bmatrix} 0 & 0 \end{bmatrix} \cdot \begin{bmatrix} 1 & 1 & 1 & 1 \\ 2 & 6 & 5 & 7 \end{bmatrix} = \begin{bmatrix} 0 & 0 & 0 & 0 \end{bmatrix}$$\

* **Cost Function – Error**   \
  \
  $$J(\theta) = \frac{1}{2m} \sum_{i=1}^{m} \left[ h_\theta(x_i) - y_i \right]^2$$ &#x20;

&#x20;      $$= \frac{1}{2 \times 4} \left[ (0 - 3)^2 + (0 - 10)^2 + (0 - 4)^2 + (0 - 3)^2 \right]$$ &#x20;

&#x20;      $$= \frac{1}{8} \left[ 9 + 100 + 16 + 9 \right]$$&#x20;

&#x20;      $$= 16.75$$



*   **Gradient Descent – Updating θ0 value** \
    Here, j = 0 \
    \
    $$\theta_j := \theta_j - \frac{\alpha}{m} \sum_{i=1}^{m} \left[ \left(h_\theta(x_i) - y_i\right) x_i \right]$$

    &#x20;  \
    $$= 0 - \frac{0.001}{4} \left[ \left(0 - 3\right) + \left(0 - 10\right) + \left(0 - 4\right) + \left(0 - 3\right) \right]$$   \
    \
    $$= \frac{0.001}{4} \left[ -3 + (-10) + (-4) + (-3) \right]$$ \
    \
    $$= \frac{0.001}{4} \left[ 20 \right]$$ \
    \
    $$= 0.005$$




* **Gradient Descent – Updating θ1 value** \
  Here, j = 1&#x20;

![](https://media.geeksforgeeks.org/wp-content/uploads/iteration-1-1gradient-descent.jpg)

**Iteration 2** – θ0 = 0.005 and θ1 = 0.02657

* **Predicted values after iteration 1 with Linear regression hypothesis.**&#x20;

![](https://media.geeksforgeeks.org/wp-content/uploads/iteration-2-hypothesis.jpg)

Now, similar to iteration no. 1 performed above we will again calculate Cost function and update θj values using Gradient Descent.\
We will keep on iterating until Cost function doesn’t reduce further. At that point, model achieves best θ values. Using these θ values in the model hypothesis will give the best prediction results.\
&#x20;

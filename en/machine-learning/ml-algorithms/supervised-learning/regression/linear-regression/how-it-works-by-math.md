# How It Works by Math

## Mathematical explanation for Linear Regression working



Suppose we are given a dataset:

![](https://media.geeksforgeeks.org/wp-content/uploads/data-8.jpg)

Given is a Work vs Experience dataset of a company and the task is to predict the salary of a employee based on his / her work experience. \
This article aims to explain how in reality Linear regression mathematically works when we use a pre-defined function to perform prediction task. \
Let us explore **how the stuff works when Linear Regression algorithm gets trained.**&#x20;

**Iteration 1** – In the start, θ0 and θ1 values are randomly chosen. Let us suppose, θ0 = 0 and θ1 = 0.&#x20;

* **Predicted values after iteration 1 with Linear regression hypothesis.**&#x20;

![](https://media.geeksforgeeks.org/wp-content/uploads/iteration-1-hypothesis-1.jpg)

* **Cost Function – Error**&#x20;

![](https://media.geeksforgeeks.org/wp-content/uploads/iteration-1-cost-function-1.jpg)

* **Gradient Descent – Updating θ0 value** \
  Here, j = 0&#x20;

![](https://media.geeksforgeeks.org/wp-content/uploads/iteration-1-theta-zero-1.jpg)

* **Gradient Descent – Updating θ1 value** \
  Here, j = 1&#x20;

![](https://media.geeksforgeeks.org/wp-content/uploads/iteration-1-1gradient-descent.jpg)

**Iteration 2** – θ0 = 0.005 and θ1 = 0.02657

* **Predicted values after iteration 1 with Linear regression hypothesis.**&#x20;

![](https://media.geeksforgeeks.org/wp-content/uploads/iteration-2-hypothesis.jpg)

Now, similar to iteration no. 1 performed above we will again calculate Cost function and update θj values using Gradient Descent.\
We will keep on iterating until Cost function doesn’t reduce further. At that point, model achieves best θ values. Using these θ values in the model hypothesis will give the best prediction results.\
&#x20;

\


Are you passionate about data and looking to make one giant leap into your career? Our **Data Science Course** will help you change your game and, most importantly, allow students, professionals, and working adults to tide over into the data science immersion. Master state-of-the-art methodologies, powerful tools, and industry best practices, hands-on projects, and real-world applications. Become the executive head of industries related to **Data Analysis**, **Machine Learning**, and **Data Visualization** with these growing skills. Ready to Transform Your Future? _**Enroll Now to Be a Data Science Expert!**_

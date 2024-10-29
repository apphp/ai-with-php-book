# Implementation in PHP ?..

### Coding Linear Regression in PHP

Two popular PHP libraries for machine learning are **Rubix ML** and **PHP-ML**. Rubix ML provides a comprehensive suite of tools with support for Linear Regression and other algorithms, while PHP-ML is a simpler library with a wide range of algorithms.

### Implementing Linear Regression with Rubix ML

Using Rubix ML, we’ll set up a simple example that predicts housing prices based on square footage.

#### **Step 1: Prepare the Data**

For this example, let’s use a small dataset with square footage and price.

```php
use Rubix\ML\Datasets\Labeled;
use Rubix\ML\Regressors\Ridge;
use Rubix\ML\CrossValidation\Metrics\MeanSquaredError;

// Sample data: [Square Footage, Number of Bedrooms] => Price
$samples = [
    [850, 2],
    [900, 2],
    [1200, 3],
    [1500, 3],
    [2000, 4],
];
$labels = [200000, 210000, 300000, 350000, 450000];
$dataset = Labeled::fromIterator($samples, $labels);
```

#### **Step 2: Train the Model**

Now, we’ll create a **Ridge Regression** model (a regularized form of linear regression) to prevent overfitting. We then train it on our dataset.

```php
$estimator = new Ridge(1.0);  // 1.0 is the regularization strength
$estimator->train($dataset);
```

#### **Step 3: Make Predictions**

Once trained, we can use the model to make predictions on new data.

```php
// New house with 1600 sq ft and 3 bedrooms
$newSample = [1600, 3];
$prediction = $estimator->predict([$newSample]);

echo "Predicted Price: $" . round($prediction[0], 2);
```

#### **Step 4: Evaluate the Model**

To measure the model’s accuracy, we can use a metric like **Mean Squared Error** (MSE), which calculates the average of squared differences between predicted and actual values.

```php
$predictions = $estimator->predict($dataset->samples());
$mse = new MeanSquaredError();

echo "Mean Squared Error: " . $mse->score($predictions, $labels);
```

Result:

..........

Fill code

...........

Code source:&#x20;



### Implementing Linear Regression with PHP-ML

In **PHP-ML**, the process is similar. We’ll use the **Linear Regression** class provided by the library.

#### **Step 1: Prepare the Data**

```php
use Phpml\Regression\LeastSquares;

// Training data
$samples = [
    [850, 2],
    [900, 2],
    [1200, 3],
    [1500, 3],
    [2000, 4],
];
$labels = [200000, 210000, 300000, 350000, 450000];
```

**Step 2: Train the Model**

```php
$regression = new LeastSquares();
$regression->train($samples, $labels);
```

**Step 3: Make Predictions**

```php
// Predict the price of a house with 1600 sq ft and 3 bedrooms
$predictedPrice = $regression->predict([1600, 3]);
echo "Predicted Price: $" . round($predictedPrice, 2);
```

Result:

..........

Full code:

...........

Code source:&#x20;



### Advanced Techniques: Feature Scaling and Regularization

When working with Linear Regression, it’s essential to consider **feature scaling**. Differences in feature scales (e.g., square footage vs. number of bedrooms) can cause the model to weigh one feature more heavily than another. Standardizing features to a similar scale improves model accuracy.

In Rubix ML, you can apply feature scaling using **Z Scale Standardizer**:

```php
use Rubix\ML\Transformers\ZScaleStandardizer;

$dataset->apply(new ZScaleStandardizer());
$estimator->train($dataset);
```

### Summary

Linear Regression is a powerful tool for predicting continuous values, and implementing it in PHP with **Rubix ML** and **PHP-ML** provides a straightforward approach to creating predictive models. By understanding the underlying mechanics, using regularization to prevent overfitting, and scaling features, you can build effective regression models in PHP.



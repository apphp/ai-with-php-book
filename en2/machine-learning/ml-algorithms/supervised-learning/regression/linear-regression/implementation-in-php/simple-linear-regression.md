# Simple Linear Regression

### Coding Linear Regression in PHP

Two popular PHP libraries for machine learning are **Rubix ML** and **PHP-ML**. Rubix ML provides a comprehensive suite of tools with support for Linear Regression and other algorithms, while PHP-ML is a simpler library with a wide range of algorithms.

### Implementing Linear Regression with Rubix ML

Using Rubix ML, we’ll set up a simple example that predicts housing prices based on square footage.

#### **Step 1: Prepare the Data**

For this example, let’s use a small dataset with square footage and price.

```php
use Rubix\ML\Datasets\Labeled;
use Rubix\ML\Datasets\Unlabeled;
use Rubix\ML\Regressors\Ridge;
use Rubix\ML\CrossValidation\Metrics\MeanSquaredError;

// Sample data: [Square Footage] => Price
$samples = [
    [800, 160000],
    [900, 180000],
    [1000, 200000],
    [1100, 220000],
    [1200, 240000],
    [1300, 260000],
    [1400, 280000],
];
$dataset = Labeled::fromIterator($samples);
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
// Make prediction for new house with 2200 sq ft
$newSample = [2200];
$newDataset = new Unlabeled([$newSample]);
$prediction = $estimator->predict($newDataset);

// Show results
echo "Sample size: 2200 sq.ft";
echo "\nPredicted Price for: $" . number_format($prediction[0], decimals: 2);
```

#### **Step 4: Evaluate the Model**

To measure the model’s accuracy, we can use a metric like **Mean Squared Error** (MSE), which calculates the average of squared differences between predicted and actual values.

```php
// Calculate Mean Squared Error
$predictions = $estimator->predict($dataset);
$mse = new MeanSquaredError();

echo "\nMean Squared Error: " . $mse->score($predictions, $dataset->labels());
```

**Full Code:**

<details>

<summary>Full Code of Example</summary>

```php
use Rubix\ML\Datasets\Labeled;
use Rubix\ML\Regressors\Ridge;
use Rubix\ML\CrossValidation\Metrics\MeanSquaredError;

// Sample data: [Square Footage] => Price
$samples = [
    [800, 160000],
    [900, 180000],
    [1000, 200000],
    [1100, 220000],
    [1200, 240000],
    [1300, 260000],
    [1400, 280000],
];

// Create a dataset from our samples (splits into features and labels)
$dataset = Labeled::fromIterator($samples);

// Create and train Ridge regression model
// 1.0 controls how much we prevent overfitting
$estimator = new Ridge(1.0);
$estimator->train($dataset);

// Predict price for a 2200 sq ft house
$newSample = [2200];
$newDataset = new Unlabeled([$newSample]);
$prediction = $estimator->predict($newDataset);

// Show results
echo 'Sample size: 2200 sq.ft';
echo "\nPredicted Price for: $" . number_format($prediction[0], decimals: 2);

// Check how accurate our model is using Mean Squared Error
// Lower number = better predictions
$predictions = $estimator->predict($dataset);
$mse = new MeanSquaredError();
echo "\n\nMean Squared Error: " . number_format($mse->score($predictions, $dataset->labels()), 10);
```

</details>

**Result:**

```
Sample size: 2250 sq.ft
Predicted Price for: $263,495.68
```

**Chart:**

<div align="left"><figure><img src="../../../../../../.gitbook/assets/image (2) (1) (1) (1) (1).png" alt="" width="563"><figcaption><p>House Prices by Square Footage with Trend Line</p></figcaption></figure></div>

***

### Implementing Linear Regression with PHP-ML

In **PHP-ML**, the process is similar. We’ll use the **Linear Regression** class provided by the library.

#### **Step 1: Prepare the Data**

For this example, let’s use a small dataset with square footage and price.

```php
use Phpml\Regression\LeastSquares;

// Training data
$samples = [
    [800, 160000],
    [900, 180000],
    [1000, 200000],
    [1100, 220000],
    [1200, 240000],
    [1300, 260000],
    [1400, 280000],
];
$labels = [160000, 180000, 200000, 220000, 240000, 260000, 280000];
```

**Step 2: Train the Model**

Now, we’ll create a **LeastSquares** model. We then train it on our dataset.

```php
$regression = new LeastSquares();
$regression->train($samples, $labels);
```

**Step 3: Make Predictions**

Once trained, we can use the model to make predictions on new data.

```php
// Predict price for a 2250 sq ft house
$newSample = [2250];
$predictedPrice = $regression->predict($newSample);

// Show results
echo 'Sample size: 2250 sq.ft';
echo "\nPredicted Price for: $" . number_format($predictedPrice, decimals: 2);
```

**Full Code:**

<details>

<summary>Full Code of Example</summary>

```php
use Phpml\Regression\LeastSquares;

// Training data
$samples = [
    [800, 160000],
    [900, 180000],
    [1000, 200000],
    [1100, 220000],
    [1200, 240000],
    [1300, 260000],
    [1400, 280000],
];
$labels = [160000, 180000, 200000, 220000, 240000, 260000, 280000];

$regression = new LeastSquares();
$regression->train($samples, $labels);

// Predict price for a 2250 sq ft house
$newSample = [2250];
$predictedPrice = $regression->predict($newSample);

// Show results
echo 'Sample size: 2250 sq.ft';
echo "\nPredicted Price for: $" . number_format($predictedPrice, decimals: 2);
```

</details>

**Result:**

```
Sample size: 2250 sq.ft
Predicted Price for: $263,495.68
```

**Chart:**

<div align="left"><figure><img src="../../../../../../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt="" width="563"><figcaption></figcaption></figure></div>

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

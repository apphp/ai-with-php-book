# Multiple Linear Regression

### Coding Multiple Linear Regression in PHP

**Multiple Linear Regression** is a statistical technique that models the relationship between a dependent variable and multiple independent variables by fitting a linear equation to observed data. This method is commonly used for predictive modeling, where the aim is to predict the value of the dependent variable based on values of independent variables.

The general equation of multiple linear regression is: $$Y = b_0 + b_1 X_1 + b_2 X_2 + \dots + b_n X_n + \epsilon$$

where:

* $$Y$$ is the dependent variable (target).
* $$X_1, X_2, \dots, X_n$$ are independent variables (features).
* $$b_0, b_1, \dots, b_n$$ are the coefficients (weights) of the model.
* $$\epsilon$$ is the error term.

In PHP, we can implement multiple linear regression using two popular libraries: **RubixML** and **PHP-ML**. Let's dive into each.

***

### Implementing Multiple Linear Regression with Rubix ML

#### Example: Predicting House Prices

Let's say we want to predict house prices based on the following features:

* Number of rooms
* Square footage
* Distance to the nearest city center

**Step 1: Prepare the Data**

```php
use Rubix\ML\Datasets\Labeled;
use Rubix\ML\Datasets\Unlabeled;
use Rubix\ML\Regressors\Ridge;

// Sample data: [rooms, size, miles to city center] => Price
$samples = [
    [3, 1500, 5],  // 3 rooms, 1500 sqft, 5 miles to city center
    [4, 2000, 3],
    [2, 800, 10],
    [5, 2500, 1],
    [3, 1600, 4],
];

// House prices
$labels = [300000, 500000, 200000, 750000, 350000];

// Create new dataset with float values
$dataset = new Labeled($samples, $labels);
```

**Step 2: Initialize the Model**

RubixML offers several regression algorithms. For this example, we'll use **Ridge Regression**, which is a form of linear regression suitable for multicollinearity (when features are correlated).

```php
// Alpha parameter for regularization
$estimator = new Ridge(1e-3);  
```

**Step 3: Train the Model**

```php
$estimator->train($dataset);
```

**Step 4: Make Predictions**

Now, we can make predictions on new data points.

```php
// Create new samples for prediction
// Important: Each sample must be its own array within the main array
$newSamples = [
    [4, 1800, 3],  // First house
    [2, 1200, 8]   // Second house
];

// Create Unlabeled dataset for prediction
$newDataset = new Unlabeled($newSamples);

// Make predictions
$predictions = $estimator->predict($newDataset);

// Print predictions
echo "Predictions for new houses:\n";
foreach ($predictions as $index => $prediction) {
    echo sprintf(
        "House %d: $%s\n",
        $index + 1,
        number_format($prediction, 2)
    );
}
```

**Full Code:**

In this example, RubixML handles the complexity of matrix operations internally, allowing you to focus on high-level machine learning tasks.

<details>

<summary>Full Code of Example</summary>

```php
use Rubix\ML\Datasets\Labeled;
use Rubix\ML\Datasets\Unlabeled;
use Rubix\ML\Regressors\Ridge;

// Sample data: [rooms, size, miles to city center] => Price
$samples = [
    [3, 1500, 5],  // 3 rooms, 1500 sqft, 5 miles to city center
    [4, 2000, 3],
    [2, 800, 10],
    [5, 2500, 1],
    [3, 1600, 4],
];

// House prices
$labels = [300000, 500000, 200000, 750000, 350000];

// Create new dataset with float values
$dataset = new Labeled($samples, $labels);

// Alpha parameter for regularization
$estimator = new Ridge(1e-3);  

$estimator->train($dataset);

// Create new samples for prediction
// Important: Each sample must be its own array within the main array
$newSamples = [
    [4, 1800, 3],  // First house
    [2, 1200, 8]   // Second house
];

// Create Unlabeled dataset for prediction
$newDataset = new Unlabeled($newSamples);

// Make predictions
$predictions = $estimator->predict($newDataset);

// Print predictions
echo "Predictions for new houses:\n";
foreach ($predictions as $index => $prediction) {
    echo sprintf(
        "House %d: $%s\n",
        $index + 1,
        number_format($prediction, 2)
    );
}
```

</details>

**Result:**

```
Predictions for new houses:
House 1: $577,025.89
House 2: $351,275.14
```

**Chart:**

<div align="left"><figure><img src="../../../../../../.gitbook/assets/image (2) (1) (1) (1) (1) (1) (1) (1).png" alt="" width="563"><figcaption></figcaption></figure></div>

***

### Using PHP-ML for Multiple Linear Regression

#### Example: Predicting House Prices

Similar to our example with RubixML, we'll predict house prices based on rooms, square footage, and distance to the city center.

**Step 1: Prepare the Data**

```php
use Phpml\Dataset\ArrayDataset;
use Phpml\Regression\LeastSquares;

$samples = [
    [3, 1500, 5],
    [4, 2000, 3],
    [2, 800, 10],
    [5, 2500, 1],
    [3, 1600, 4],
];

$labels = [300000, 500000, 200000, 750000, 350000];
```

**Step 2: Initialize the Model**

PHP-ML provides a **Least Squares** regression algorithm, which fits a linear model to minimize the sum of squared residuals.

```php
$regressor = new LeastSquares();
```

**Step 3: Train the Model**

```php
$regressor->train($samples, $labels);
```

**Step 4: Make Predictions**

You can now use the trained model to predict prices for new house data.

```php
$newData = [
    [4, 1800, 3],
    [2, 1200, 8],
];

$predictions = $regressor->predict($newData);

print_r($predictions);
```

**Full Code:**

<details>

<summary>Full Code of Example</summary>

```php
use Phpml\Dataset\ArrayDataset;
use Phpml\Regression\LeastSquares;

$samples = [
    [3, 1500, 5],
    [4, 2000, 3],
    [2, 800, 10],
    [5, 2500, 1],
    [3, 1600, 4],
];

$labels = [300000, 500000, 200000, 750000, 350000];

$regressor = new LeastSquares();

$regressor->train($samples, $labels);

$newData = [
    [4, 1800, 3],
    [2, 1200, 8],
];

$predictions = $regressor->predict($newData);

print_r($predictions);
```

</details>

**Result:**

```
Predictions for new houses:
House 1: $577,026.62
House 2: $351,276.24
```

**Chart:**

<div align="left"><figure><img src="../../../../../../.gitbook/assets/image (3) (1) (1) (1) (1) (1).png" alt="" width="563"><figcaption></figcaption></figure></div>

***

### Comparing RubixML and PHP-ML

Both libraries provide similar functionality for linear regression, with differences in the underlying algorithms and options available.

| Feature           | RubixML               | PHP-ML          |
| ----------------- | --------------------- | --------------- |
| Model             | Ridge Regression (L2) | Least Squares   |
| Data Input Format | Dataset Objects       | ArrayDataset    |
| Flexibility       | High                  | Moderate        |
| Model Variety     | Broad                 | Limited         |
| Installation      | `rubix/ml`            | `php-ai/php-ml` |

### Conclusion

This chapter demonstrates how to implement multiple linear regression in PHP using RubixML and PHP-ML libraries. Each library has strengths: **RubixML** offers flexibility and a broader set of machine learning algorithms, while **PHP-ML** provides a straightforward interface for quick prototyping. By following the examples, you can start building your predictive models in PHP and apply them to various real-world scenarios like price prediction, trend analysis, and forecasting.

# Polynomial Regression

### Coding Polynomial Regression in PHP

Polynomial regression is an extension of linear regression that allows us to model nonlinear relationships between variables by adding polynomial terms to the predictor variables. In this article, we'll explore how to implement polynomial regression in PHP using two popular machine learning libraries: **RubixML** and **PHP-ML**.

***

### Implementing Polynomial Regression with Rubix ML

RubixML provides a powerful and flexible implementation of polynomial regression. Here's how to use it:

#### **Step 1: Prepare the Data**

```php
use Rubix\ML\Datasets\Labeled;
use Rubix\ML\Transformers\PolynomialExpander;
use Rubix\ML\Regressors\Ridge;

// Prepare your training data
$samples = [
    [1], [2], [3], [4], [5], [6]
];
$labels = [
    2.1, 4.2, 9.3, 16.4, 25.5, 36.6
];

// Create a labeled dataset
$dataset = new Labeled($samples, $labels);
```

#### **Step 2:** Create a Polynomial Expander Transformer

Create a polynomial expander transformer. The argument '2' means we'll create quadratic features $$x^2$$.

```php
$expander = new PolynomialExpander(2);
```

#### **Step 3:** Create Model

Create the model. We use Ridge regression with regularization strength of 0.1. Ridge regression helps prevent overfitting.

```php
$estimator = new Ridge(0.1); 
```

#### **Step 4:** Transform the Features

This creates polynomial features from original data.

```php
$dataset->transformFeatures($expander);
```

#### **Step 5:** Train the Model&#x20;

The model learns the relationships between features and targets.

```php
$estimator->train($dataset);
```

**Step 4: Make Predictions**

Create test data and predict their values.

```php
$predictions = $estimator->predict(new Labeled([
    [7], [8]
], ['unknown', 'unknown']));

print_r($predictions);
```

**Full Code:**

<details>

<summary>Full Code of Example</summary>

```php
use Rubix\ML\Datasets\Labeled;
use Rubix\ML\Transformers\PolynomialExpander;
use Rubix\ML\Regressors\Ridge;

// Step 1: Prepare your training data
$samples = [
    [1], [2], [3], [4], [5], [6]
];
$labels = [
    2.1, 4.2, 9.3, 16.4, 25.5, 36.6
];

// Step 2: Create a labeled dataset
// This combines features (samples) with their corresponding target values (labels)
$dataset = new Labeled($samples, $labels);

// Step 3: Create a polynomial expander transformer
// The argument '2' means we'll create quadratic features (x²)
$expander = new PolynomialExpander(2);

// Step 4: Create the model
// We use Ridge regression with regularization strength 0.1
// Ridge regression helps prevent overfitting
$estimator = new Ridge(0.1);

// Step 5: Transform the features
// This creates polynomial features from original data
$dataset->transformFeatures($expander);

// Step 6: Train the model
// The model learns the relationships between features and targets
$estimator->train($dataset);

// Step 7: Make predictions
// Create test data and predict their values
$predictions = $estimator->predict(new Labeled([
    [7], [8]
], ['unknown', 'unknown']));

print_r($predictions);
```

</details>

**Result:**

```
Predictions .....
```

**Chart:**





#### Key Features of RubixML Implementation:

1. **PolynomialExpander**: This transformer automatically creates polynomial features up to the specified degree.
2. **Ridge Regression**: Used instead of standard linear regression to prevent overfitting.
3. **Regularization**: The Ridge regressor includes L2 regularization to control model complexity.

***

### Implementing Polynomial Regression with PHP-ML

PHP-ML offers a different approach to polynomial regression. Here's how to implement it:

```php
use Phpml\Preprocessing\PolynomialFeatures;
use Phpml\Regression\LeastSquares;

// Step 1: Prepare training data
// Each sample is an array containing one feature
$samples = [[1], [2], [3], [4], [5], [6]];
$targets = [2.1, 4.2, 9.3, 16.4, 25.5, 36.6];

// Step 2: Create polynomial features
// This transforms original features into polynomial features
$polynomialFeatures = new PolynomialFeatures(2);
$samplesTransformed = $polynomialFeatures->transform($samples);

// Step 3: Create and train the model
$regression = new LeastSquares();
$regression->train($samplesTransformed, $targets);

// Step 4: Prepare test data
$testSamples = [[7], [8]];
$testSamplesTransformed = $polynomialFeatures->transform($testSamples);

// Step 5: Make predictions
$predictions = $regression->predict($testSamplesTransformed);

print_r($predictions);
```

#### Key Features of PHP-ML Implementation:

1. **PolynomialFeatures**: Transforms input features into polynomial features.
2. **LeastSquares**: Implements ordinary least squares regression.
3. **Simple API**: More straightforward API compared to RubixML.

***

### Best Practices

When implementing polynomial regression in PHP, consider these best practices:

1. **Feature Scaling**: Always scale your features before creating polynomial terms to prevent numerical instability:

```php
// RubixML example with scaling
use Rubix\ML\Transformers\MinMaxScaler;

$dataset->transformFeatures(new MinMaxScaler());
$dataset->transformFeatures(new PolynomialExpander(2));
```

2. **Cross-Validation**: Use cross-validation to prevent overfitting:

```php
// RubixML example with cross-validation
use Rubix\ML\CrossValidation\HoldOut;

$validator = new HoldOut(0.2);
$score = $validator->test($estimator, $dataset);
```

3. **Degree Selection**: Choose the polynomial degree carefully to balance between underfitting and overfitting:

```php
// Test different polynomial degrees
foreach (range(1, 5) as $degree) {
    $expander = new PolynomialExpander($degree);
    // ... evaluate model performance
}
```

### Error Handling and Validation

Always include proper error handling in your implementation:

```php
phpCopytry {
    $dataset->transformFeatures($expander);
    $estimator->train($dataset);
} catch (Exception $e) {
    echo "Error during training: " . $e->getMessage();
}
```

### Conclusion

Both RubixML and PHP-ML provide robust implementations of polynomial regression, each with its own advantages. RubixML offers more advanced features and better scalability, while PHP-ML provides a simpler interface that's great for learning and smaller projects. Choose the library that best fits your specific needs, considering factors like dataset size, required features, and performance requirements.

Remember to always preprocess your data, validate your model's performance, and handle errors appropriately. With proper implementation, polynomial regression can be a powerful tool for modeling nonlinear relationships in your PHP applications.

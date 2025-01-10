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
    [6.575], [6.421], [7.185], [6.998], [7.147], [6.430], [6.012], [6.172],
    [5.631], [6.004], [6.377], [6.009], [5.889], [5.949], [6.096], [5.834],
    [5.989], [8.259], [8.183], [7.853], [7.255], [6.383], [6.816], [7.420],
    [7.685],
];

$labels = [
    25.0, 22.6, 33.4, 33.4, 36.2, 28.7, 20.6, 22.9, 16.9, 18.9, 21.6,
    18.9, 21.7, 20.4, 21.2, 19.9, 22.2, 37.7, 37.3, 40.1, 37.2, 25.7,
    31.6, 38.7, 38.1,
]; 

// Create a labeled dataset
$dataset = new Labeled($samples, $labels);
```

#### **Step 2:** Create a Polynomial Expander Transformer and Normalizer

Create a polynomial expander transformer. The argument '3' means we'll create cubing features $$x^3$$.\
Expands the dataset by adding polynomial terms to increase the model’s capacity to capture non-linear relationships.

```php
$expander = new PolynomialExpander(3);
```

Normalizer scales the features to a standard range (e.g., 0 to 1 or standard Gaussian distribution) to stabilize and improve model performance.

```php
$normalizer = new Normalizer();
```

#### **Step 3:** Create Model

Create the model. We use `LeastSquares` regression.

```php
$regression = new LeastSquares(0.1); 
```

#### **Step 4:** Transform the Features

This creates polynomial features from original data. This transformer expands features into higher-degree polynomial terms, allowing the model to capture non-linear relationships effectively. In this case, the degree is set to 3 to include cubed features.

```php
// Transform features using PolynomialExpander
$expander->transform($samples);
```

Normalizes the features to improve numerical stability and ensure consistent scaling across the dataset.

```php
// Normalize features
$normalizer->transform($samples);
```

#### **Step 5:** Train the Model&#x20;

Ridge Regression (Least Squares).  A regression algorithm that introduces regularization to minimize overfitting by penalizing large coefficients - 0.1 (regularization strength). Smaller values increase regularization.

```php
$regression->train($samples, $labels);
```

**Step 4: Make Predictions**

Create test data and predict their values.

```php
// Prepare test samples
$testSamples = [[5.5], [6], [8], [6.945], [5.631], [8.259]];

// Transform test data
$expander->transform($testSamples);

// Normalize test features
$normalizer->transform($testSamples);

// Make predictions
$predictions = $regression->predict($testSamples);

print_r($predictions);
```

**Full Code:**

<details>

<summary>Full Code of Example</summary>

```php
use Phpml\Preprocessing\Normalizer;
use Phpml\Regression\LeastSquares;
use Rubix\ML\Datasets\Labeled;
use Rubix\ML\Transformers\PolynomialExpander;

// Step 1: Prepare your training data
$samples = [
    [6.575], [6.421], [7.185], [6.998], [7.147], [6.430], [6.012], [6.172],
    [5.631], [6.004], [6.377], [6.009], [5.889], [5.949], [6.096], [5.834],
    [5.989], [8.259], [8.183], [7.853], [7.255], [6.383], [6.816], [7.420],
    [7.685],
];

$labels = [
    25.0, 22.6, 33.4, 33.4, 36.2, 28.7, 20.6, 22.9, 16.9, 18.9, 21.6,
    18.9, 21.7, 20.4, 21.2, 19.9, 22.2, 37.7, 37.3, 40.1, 37.2, 25.7,
    31.6, 38.7, 38.1,
];

// Step 2: Create a labeled dataset
$dataset = new Labeled($samples, $labels);

// Step 3: Create a polynomial expander transformer and normalizer
$expander = new PolynomialExpander(3);
$normalizer = new Normalizer();

// Step 4: Create the model
$regression = new LeastSquares(0.1);

// Step 5: Transform and normalize the features
$expander->transform($samples);
$normalizer->transform($samples);

// Step 6: Train the model
$regression->train($samples, $labels);

// Step 7: Make predictions
$testSamples = [[5.5], [6], [8], [6.945], [5.631], [8.259]];
// Transform test data
$expander->transform($testSamples);
// Normalize test features
$normalizer->transform($testSamples);
// Make predictions
$predictions = $regression->predict($testSamples);

print_r($predictions);

```

</details>

**Result:**

```
Price Predictions:
-----------------
A house with 5.5 rooms is predicted to cost $20,506.43
A house with 6.0 rooms is predicted to cost $20,082.20
A house with 8.0 rooms is predicted to cost $38,730.49
A house with 6.9 rooms is predicted to cost $32,757.74
A house with 5.6 rooms is predicted to cost $19,049.67
A house with 8.3 rooms is predicted to cost $38,018.52
```

**Chart:**

<div align="left"><figure><img src="broken-reference" alt="" width="563"><figcaption></figcaption></figure></div>

#### Key Features of RubixML Implementation:

1. **PolynomialExpander**: This transformer automatically creates polynomial features up to the specified degree.
2. **LeastSquares Regression**: Used instead of standard linear regression to prevent overfitting.
3. **Regularization**: A regression algorithm that incorporates L2 regularization (also known as Ridge Regression) to reduce the risk of overfitting and improve model generalization.
4. **Training Pipeline**: The workflow follows a structured pipeline where training data is first transformed (using PolynomialExpander and Normalizer) before training the model.

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

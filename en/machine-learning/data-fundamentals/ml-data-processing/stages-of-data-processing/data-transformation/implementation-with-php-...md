# Implementation with PHP ?..

Data transformation is essential for machine learning, as it prepares raw data into a format suitable for analysis and modeling. This chapter explores four key transformation techniques using **RubixML** and **PHP-ML**: **Encoding Categorical Variables**, **Normalizing and Scaling Numerical Features**, **Reshaping Data Structures**, and **Feature Engineering**.

***

### **RubixML** Examples

#### **1. Encoding Categorical Variables**

Categorical data, such as "color" or "size," needs to be converted into numerical format so machine learning models can interpret it. **One-Hot Encoding** is a common method that transforms each category into a binary vector.

**Example with RubixML:**

```php
use Rubix\ML\Datasets\Unlabeled;
use Rubix\ML\Transformers\OneHotEncoder;

// Create the dataset
$dataset = new Unlabeled([
    ['red', 'small'],
    ['blue', 'medium'],
    ['green', 'large'],
]);

$encoder = new OneHotEncoder();
$encoder->fit($dataset);
$samples = $dataset->samples();
$encoder->transform($samples);

echo "\nAfter Encoding:\n";
foreach ($samples as $sample) {
    echo implode('', $sample) . "\n";
}
```

Here, `OneHotEncoder` from RubixML converts each unique category into binary values, making it compatible with machine learning algorithms.

```
After Encoding:
100100
010010
001001
```

#### **2. Normalizing and Scaling Numerical Features**

Normalization adjusts numerical data to a standard range (often \[0, 1]), which helps with model performance when features are on different scales.

**Example with RubixML:**

```php
use Rubix\ML\Datasets\Labeled;
use Rubix\ML\Transformers\MinMaxNormalizer;

// Create the dataset
$dataset = new Labeled([
    [2000, 300],
    [2400, 450],
    [3000, 500],
], ['low', 'medium', 'high']);

$normalizer = new MinMaxNormalizer();
$normalizer->fit($dataset);

$samples = $dataset->samples();
$labels = $dataset->labels();
$normalizer->transform($samples);

echo "\nNormalized data:\n";
foreach ($samples as $ind => $sample) {
    echo implode(',', $sample) . ',' . $labels[$ind] . "\n";
}
```

In this example, `MinMaxNormalizer` scales values to the \[0, 1] range, ensuring each feature is comparable.

```
Normalized data:
0,0,low
0.4,0.75,medium
1,1,high
```

#### **3. Reshaping Data Structures**

Reshaping allows us to organize data into structures required by specific algorithms. For example, in time series analysis, data can be reshaped into rolling windows for sequence modeling.

**Example of Reshaping for Time Series:**

```php
$data = [[100], [150], [200], [250], [300], [350]];

// Create windows manually since RubixML doesn't have built-in windowing
function reshapeIntoRollingWindows(array $data, int $windowSize): array {
    // If input is a flat array, convert each element to an array
    $isFlat = !is_array(reset($data));
    $formattedData = $isFlat ? array_map(fn($value) => [$value], $data) : $data;

    $windows = [];
    for ($i = 0; $i <= count($formattedData) - $windowSize; $i++) {
        $window = array_slice($formattedData, $i, $windowSize);
        $windows[] = array_column($window, 0);
    }
    return $windows;
}

$reshapedData = reshapeIntoRollingWindows($dataset->samples(), 3);

// Convert back to RubixML dataset if needed
$windowedDataset = new Unlabeled($reshapedData);

```

In this example, `reshapeIntoRollingWindows` manually reshaping the dataset into sequences of three-day periods, making it suitable for time series models.

```
After Reshaping: 
[[100, 150, 200], [150, 200, 250], [200, 250, 300], [250, 300, 350]]
```

#### **4. Feature Engineering**

Feature engineering enhances model performance by creating new attributes from existing data. For instance, polynomial expansion generates interaction terms between features, which can reveal complex patterns.

**Example with RubixML:**

```php
use Rubix\ML\Transformers\PolynomialExpander;

$dataset = new Labeled([
    [2000, 300],
    [2500, 400],
    [3000, 500],
], ['low', 'medium', 'high']);

$expander = new PolynomialExpander(2); // Creates second-degree polynomial features
$expander->transform($dataset);

print_r($dataset->samples());
```

The `PolynomialExpander` in RubixML generates interaction terms for each feature pair, allowing the model to capture non-linear relationships between attributes.

***

### PHP-ML Examples

#### **1. Encoding Categorical Variables**

PHP-ML also provides one-hot encoding for categorical data, which is crucial for converting non-numerical values into binary format.

**Example with PHP-ML:**

```php
use Phpml\Preprocessing\OneHotEncoder;

$samples = [
    ['red', 'small'],
    ['blue', 'medium'],
    ['green', 'large'],
];

$encoder = new OneHotEncoder();
$encoder->fit($samples);
$encoder->transform($samples);

print_r($samples);
```

`OneHotEncoder` in PHP-ML performs the same categorical transformation, making categorical values accessible to models.

#### **2. Normalizing and Scaling Numerical Features**

PHP-ML provides a `Normalizer` class to scale numerical values, ensuring that all features are on a comparable scale.

**Example with PHP-ML:**

```php
use Phpml\Preprocessing\Normalizer;

$samples = [
    [2000, 300],
    [2500, 400],
    [3000, 500],
];

$normalizer = new Normalizer();
$normalizer->transform($samples);

print_r($samples);
```

Here, the `Normalizer` class in PHP-ML scales each feature within the dataset, which can significantly enhance compatibility with algorithms sensitive to differing feature scales.

#### **3. Reshaping Data Structures**

While PHP-ML does not have a built-in reshaping function, reshaping data can be done manually to suit sequence or time series modeling requirements.

**Example of Reshaping for Time Series:**

```php
$data = [100, 150, 200, 250, 300, 350];
$windowSize = 3;
$reshapedData = [];

for ($i = 0; $i <= count($data) - $windowSize; $i++) {
    $reshapedData[] = array_slice($data, $i, $windowSize);
}

print_r($reshapedData);
```

This custom reshaping structure groups data into rolling windows, allowing it to be used for models that require sequential data.

#### **4. Feature Engineering**

PHP-ML does not include specific feature engineering tools like polynomial expansion, but custom features can be added manually to enhance model performance.

**Manual Feature Engineering Example in PHP-ML:**

```php
$samples = [
    [2000, 300],
    [2500, 400],
    [3000, 500],
];

foreach ($samples as &$sample) {
    $sample[] = $sample[0] * $sample[1]; // Creating an interaction term between the first and second feature
}

print_r($samples);
```

In this example, we manually add a new feature by calculating the product of two existing features, introducing a potential interaction term that may help the model identify more complex patterns.

***

### Summary

Data transformation prepares raw data for analysis by encoding, normalizing, reshaping, and engineering features. Using RubixML and PHP-ML, these transformations can be efficiently implemented in PHP, enhancing data compatibility and model accuracy. In the next chapter, we will explore **feature selection**, discussing ways to retain the most relevant features for improving model efficiency and accuracy.

{% hint style="info" %}
To try this code yourself, install the example files from the official GitHub repository: [https://github.com/apphp/ai-with-php-examples](https://github.com/apphp/ai-with-php-examples)
{% endhint %}

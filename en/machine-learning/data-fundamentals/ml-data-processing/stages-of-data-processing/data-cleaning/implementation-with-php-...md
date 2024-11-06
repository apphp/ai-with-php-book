# Implementation with PHP ?..

Sure! Here’s a detailed example for data cleaning using **RubixML** and **PHP-ML**, two machine learning libraries for PHP. We'll look at how to handle missing values, normalization, and standardization.

For this example, let’s assume you’re working with a dataset containing information about customer transactions, with fields like `age`, `income`, `spending_score`, and some missing values. We'll demonstrate each data cleaning step using the RubixML library, followed by some alternative approaches using PHP-ML.

***

#### 1. Setting Up the Dataset

Let's assume our dataset is a CSV file named `customers.csv` with the following fields:

```
age,income,spending_score,tag
25,55000,45,yes
32,?,75,yes
40,72000,?,yes
?,82000,60,yes
28,63000,30,yes
```

#### RubixML Example

RubixML provides dedicated transformers for handling missing values, normalization, and standardization. Let’s break down each step.

#### **Step 1: Handling Missing Values**

RubixML provides the `MissingDataImputer` for handling missing values. This imputer allows you to fill in missing values using strategies like `Mean`, `Median`, or `Constant`.

```php
require 'vendor/autoload.php';

use Rubix\ML\Datasets\Labeled;
use Rubix\ML\Transformers\MissingDataImputer;
use Rubix\ML\Extractors\CSV;
use Rubix\ML\Strategies\Mean;
use Rubix\ML\Strategies\Prior;

// Load the dataset using CSV instead of CsvIterator
$dataset = Labeled::fromIterator(new CSV(dirname(__FILE__) . '/customers.csv'), true);

// Create imputer with mean strategy for numeric values and
// Prior (most frequent value) strategy for categorical values
$imputer = new MissingDataImputer(new Mean(), new Prior());

$dataset->apply($imputer);

echo "After Imputation: \n";
print_r($dataset->samples());
```

Here, `MissingDataImputer` will replace missing values with the mean of the respective column.

#### **Step 2: Normalization**

RubixML has a `MinMaxNormalizer` that scales values to a range (usually between 0 and 1). This is especially useful for features like `income` and `spending_score` that vary widely.

```php
use Rubix\ML\Transformers\MinMaxNormalizer;

// Apply normalization
$normalizer = new MinMaxNormalizer(0, 1); // Scale values between 0 and 1
$dataset->apply($normalizer);

echo "After Normalization: \n";
print_r($dataset->samples());
```

The `MinMaxNormalizer` will now adjust each feature to the 0–1 range, ensuring uniformity across features like `income` and `spending_score`.

#### **Step 3: Standardization**

If standardization is more appropriate (for instance, if we’re using algorithms like SVMs that are sensitive to variance), we can apply the `ZScaleStandardizer`.

```php
use Rubix\ML\Transformers\ZScaleStandardizer;

// Apply standardization
$standardizer = new ZScaleStandardizer();
$dataset->apply($standardizer);

echo "After Standardization: \n";
print_r($dataset->samples());
```

The `ZScaleStandardizer` adjusts the features to have a mean of 0 and a standard deviation of 1, which is ideal for models like Support Vector Machines (SVM) and Principal Component Analysis (PCA).

***

### PHP-ML Example

PHP-ML offers similar functionality, although it is less feature-rich than RubixML. Here’s how to handle some of these tasks with PHP-ML.

#### **Handling Missing Values**

PHP-ML doesn’t have a built-in `MissingDataImputer`, but you can write custom code to handle missing values.

```php
use Phpml\Dataset\CsvDataset;

// Load the dataset
$dataset = new CsvDataset('customers.csv', 3);

// Custom function to replace missing values with the mean of the column
function imputeMissingValues($dataset) {
    $samples = $dataset->getSamples();
    $colMeans = [];

    // Calculate the mean for each column
    foreach (range(0, 2) as $colIndex) {
        $colValues = array_column($samples, $colIndex);
        $filteredValues = array_filter($colValues, fn($val) => $val !== null && $val !== '');
        $colMeans[$colIndex] = array_sum($filteredValues) / count($filteredValues);
    }

    // Replace missing values with the column mean
    foreach ($samples as &$sample) {
        foreach ($sample as $i => &$value) {
            if ($value === null || $value === '') {
                $value = $colMeans[$i];
            }
        }
    }
    
    return $samples;
}

// Apply missing value imputation
$samples = imputeMissingValues($dataset);
print_r($samples);
```

This function calculates the mean for each column and replaces missing values with the respective column mean.

#### **Normalization and Standardization**

Normalization in PHP-ML can be done manually or by looping through each feature. However, PHP-ML also includes some transformers, though they are more limited. Here’s an example of manual Min-Max normalization.

```php
function normalize($samples) {
    $minMax = [];

    // Find min and max for each column
    foreach (range(0, count($samples[0]) - 1) as $colIndex) {
        $colValues = array_column($samples, $colIndex);
        $minMax[$colIndex] = [min($colValues), max($colValues)];
    }

    // Normalize each value
    foreach ($samples as &$sample) {
        foreach ($sample as $i => &$value) {
            $min = $minMax[$i][0];
            $max = $minMax[$i][1];
            $value = ($value - $min) / ($max - $min);
        }
    }

    return $samples;
}

$samples = normalize($samples);
print_r($samples);
```

### Summary

Using **RubixML**, we streamlined data cleaning with `MissingDataImputer`, `MinMaxNormalizer`, and `ZScaleStandardizer`. With **PHP-ML**, custom functions were needed to perform imputation, normalization, and standardization. RubixML is more convenient and feature-rich for these data preprocessing tasks, making it a good choice for machine learning in PHP.

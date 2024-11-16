# Implementation NN in PHP



```php
use Rubix\ML\Classifiers\MLPClassifier;
use Rubix\ML\Datasets\Labeled;
use Rubix\ML\CrossValidation\Reports\MulticlassBreakdown;

// Define the training dataset
$samples = [
    [1200, 3, 1], // square footage, bedrooms, location code
    [2000, 4, 2],
];
$labels = ['200000', '350000']; // House prices

$dataset = Labeled::build($samples, $labels);

// Define the neural network
$estimator = new MLPClassifier([
    10, // 10 neurons in the hidden layer
], 100, new Sigmoid());

// Train the model
$estimator->train($dataset);

// Make predictions
$predicted = $estimator->predict([[1500, 3, 1]]);
echo "Predicted Price: $predicted[0]";
```


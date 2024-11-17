# NN with Layers

Using the Rubix ML library, let’s build a simple neural network to classify data. This example demonstrates how input, hidden, and output layers interact.

```php
use Rubix\ML\Classifiers\MLPClassifier;
use Rubix\ML\Datasets\Labeled;

// Define the dataset
$samples = [
    [1200, 3, 1], // Input: square footage, bedrooms, location
    [2000, 4, 2],
];
$labels = ['200000', '350000']; // Output: House prices

$dataset = Labeled::build($samples, $labels);

// Configure the neural network
$network = new MLPClassifier([
    3,  // Input layer: 3 neurons for 3 features
    10, // Hidden layer: 10 neurons
    1,  // Output layer: 1 neuron for regression output
]);

// Train the model
$network->train($dataset);

// Make a prediction
$prediction = $network->predict([[1500, 3, 1]]);
echo "Predicted Price: $prediction[0]";
```


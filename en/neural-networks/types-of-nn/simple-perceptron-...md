# Simple Perceptron ?..

Using the **Rubix ML** library, we can create a basic neural network:



Schema:

<div align="left">

<figure><img src="../../.gitbook/assets/image (141).png" alt="" width="563"><figcaption></figcaption></figure>

</div>

```php
use Rubix\ML\Datasets\Labeled;
use Rubix\ML\Datasets\Unlabeled;
use Rubix\ML\NeuralNet\Layers\Dense;
use Rubix\ML\NeuralNet\Layers\Activation;
use Rubix\ML\NeuralNet\Optimizers\Adam;
use Rubix\ML\NeuralNet\ActivationFunctions\ReLU;
use Rubix\ML\Classifiers\MultilayerPerceptron;

// Create a simple dataset for binary classification
// Example: Predict if a student will pass (1) or fail (0) based on study hours and previous test score
$samples = [
    [2, 65],  // 2 hours study, 65% previous score
    [1, 45],
    [8, 85],
    [4, 75],
    [7, 90],
    [3, 55],
    [6, 78],
    [5, 80],
];

$labels = ['fail', 'fail', 'pass', 'pass', 'pass', 'fail', 'pass', 'pass'];

// Create a labeled dataset
$dataset = new Labeled($samples, $labels);

// Initialize neural network with no hidden layers (simple perceptron)
$estimator = new MultilayerPerceptron([
    new Dense(1), // Output layer with single neuron
    new Activation(new ReLU()), // ReLU activation function
], 100, // Maximum number of epochs
    new Adam(0.01) // Learning rate of 0.01
);

// Train the network
$estimator->train($dataset);

// Make predictions using Unlabeled dataset
$testSamples = [
    [6, 82],  // New student: 6 hours study, 82% previous score
    [1, 50],  // New student: 1 hour study, 50% previous score
];

// Create an unlabeled dataset for predictions
$testDataset = new Unlabeled($testSamples);

// Make predictions
$predictions = $estimator->predict($testDataset);

// Output predictions
foreach ($predictions as $index => $prediction) {
    echo "Student " . ($index + 1) . " prediction: " . $prediction . PHP_EOL;
}
```

This example highlights how single neuron comes in practice.

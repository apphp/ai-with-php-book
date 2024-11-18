# Implementation in PHP

### Building and Training a Basic Neural Network (Simple Perceptron)&#x20;

In this chapter, we will walk through the process of building, training, and making predictions using a simple neural network. The code presented demonstrates a basic binary classification task: predicting whether a student will pass or fail based on their study hours and previous test scores.

### Implementing Simple Perceptron with Rubix ML

#### Step 1: Import Necessary Classes

```php
use Rubix\ML\Datasets\Labeled;
use Rubix\ML\Datasets\Unlabeled;
use Rubix\ML\NeuralNet\Layers\Dense;
use Rubix\ML\NeuralNet\Layers\Activation;
use Rubix\ML\NeuralNet\Optimizers\Adam;
use Rubix\ML\NeuralNet\ActivationFunctions\ReLU;
use Rubix\ML\Classifiers\MultilayerPerceptron;
```

We begin by importing the necessary classes from the Rubix ML library. These include tools for handling datasets (Labeled and Unlabeled), building neural network layers (Dense and Activation), setting optimization strategies (Adam), applying activation functions (ReLU), and using the MultilayerPerceptron classifier.

#### Step 2: Prepare the Dataset

```php
$samples = [
    [2, 65],
    [1, 45],
    [8, 85],
    [4, 75],
    [7, 90],
    [3, 55],
    [6, 78],
    [5, 80],
];
$labels = ['fail', 'fail', 'pass', 'pass', 'pass', 'fail', 'pass', 'pass'];
```

The dataset comprises samples and labels:

* Samples: Each sample represents a student with two features: hours of study and their previous test score.
* Labels: These are the corresponding outcomes (pass or fail), indicating whether a student is predicted to pass or fail.

#### Step 3: Create a Labeled Dataset

```php
$dataset = new Labeled($samples, $labels);
```

We encapsulate the samples and labels into a Labeled dataset. This dataset is suitable for training supervised learning models.

#### Step 4: Define the Neural Network

```php
$estimator = new MultilayerPerceptron([
    new Dense(1),
    new Activation(new ReLU()),
], 100, new Adam(0.01));
```

Here, we define the architecture of a simple neural network using MultilayerPerceptron:

**1. Layers:**

* Dense(1) specifies the output layer with a single neuron.
* Activation(new ReLU()) applies the ReLU (Rectified Linear Unit) activation function to the output.

**2. Training Parameters:**

* Epochs: The model will train for up to 100 iterations.
* Optimizer: We use the Adam optimizer with a learning rate of 0.01 to adjust weights during training.

#### Step 5: Train the Neural Network

```php
$estimator->train($dataset);
```

The train method trains the neural network using the labeled dataset. During this process:

* The network adjusts its weights to minimize error.
* The model learns the relationship between inputs (study hours and test scores) and outputs (pass or fail).

#### Step 6: Make Predictions

```php
$testSamples = [
    [6, 82],
    [1, 50],
];
$testDataset = new Unlabeled($testSamples);
$predictions = $estimator->predict($testDataset);
```

To test the model, we create an Unlabeled dataset containing new samples:

* Each sample represents a new student with their study hours and test scores.
* The predict method generates predictions (pass or fail) based on the trained model.

#### Step 7: Output Predictions

```php
foreach ($predictions as $index => $prediction) {
    echo "Student " . ($index + 1) . " prediction: " . $prediction . PHP_EOL;
}
```

Finally, we loop through the predictions and display the results. Each prediction corresponds to whether a student is likely to pass or fail.

**Full Code:**

<details>

<summary>Full Code of Example</summary>

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

</details>

**Result:**

```
Student 1 prediction: pass
Student 2 prediction: fail
```

**Flow Chart:**&#x20;

<div align="left">

<figure><img src="../../../.gitbook/assets/image (141).png" alt="" width="563"><figcaption></figcaption></figure>

</div>

\

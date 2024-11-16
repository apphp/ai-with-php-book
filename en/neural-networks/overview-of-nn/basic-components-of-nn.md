# Basic Components of NN

Neural networks are the backbone of many artificial intelligence (AI) systems, mimicking the way the human brain processes information. At their core, neural networks are composed of three primary components: neurons, layers, and the connections between them, defined by weights. In this chapter, we’ll explore these elements and understand how they work together to process information.

<div align="left">

<figure><img src="../../.gitbook/assets/image (138).png" alt="" width="375"><figcaption><p>Basic Components of NN</p></figcaption></figure>

</div>

### 1. Neurons: The Fundamental Processing Units

#### What Are Neurons?

In artificial neural networks, neurons, also known as nodes or units, are computational units that mimic the biological neurons of the brain. They take input, process it, and produce an output that is passed to the next layer of the network.

#### Neuron Functionality

Each neuron performs the following operations:

**1. Receive Input:** A neuron receives one or more inputs, each with an associated weight.

**2. Weighted Sum Calculation:** The inputs are multiplied by their respective weights and summed.&#x20;

$$z = \sum_{i=1}^n w_i \cdot x_i + b$$

Here:

* $$x_i$$ is the input.
* $$w_i$$ is the weight for the -th input.
* $$b$$ is the bias term, which allows the neuron to shift its activation.

**3. Activation Function Application:** The result  is passed through an activation function to introduce non-linearity, enabling the network to solve complex problems. Common activation functions include:

* Sigmoid: $$f(z) = \frac{1}{1 + e^{-z}}$$
* ReLU (Rectified Linear Unit): $$f(z) = \max(0, z)$$
* Tanh: $$f(z) = \frac{e^z - e^{-z}}{e^z + e^{-z}}$$

#### Role of Neurons in Information Processing

Neurons act as decision-makers. By adjusting the weights and biases, they learn patterns from data, enabling tasks like image recognition, language translation, and more. The combination of neurons in layers allows neural networks to model increasingly complex relationships.

### 2. Layers: Organizing the Network

Neural networks are structured in layers, each serving a unique purpose. These layers work together to transform the input data into meaningful outputs.

#### Types of Layers

1\. Input Layer:

• The entry point of the network.

• Each neuron in this layer corresponds to a feature of the input data. For example, in image processing, each pixel value might represent a neuron.

• No computation occurs here; it simply forwards the input data to the next layer.

2\. Hidden Layers:

• These are the intermediate layers where the actual computations take place.

• The number of hidden layers and neurons per layer determines the network’s complexity and capacity to model intricate patterns.

• Hidden layers use activation functions to learn non-linear transformations.

3\. Output Layer:

• Produces the final output of the network.

• The number of neurons here corresponds to the number of output classes or the dimension of the predicted value.

• For example:

• In binary classification, the output layer has one neuron with a sigmoid activation function.

• In multi-class classification, the output layer uses softmax activation to produce probabilities for each class.

\


3\. Connections and Weights

\


Connections Between Neurons

\


Neurons are interconnected, with each connection represented by a weight. These connections determine how information flows through the network.

\


• Forward Propagation:

During forward propagation, inputs are passed through the network layer by layer, with each connection applying its weight to the input data.

• Backpropagation:

During training, errors are propagated backward through the network, and weights are updated to minimize the error using techniques like gradient descent.

\


Weights: The Learnable Parameters

\


Weights are the core learnable components of a neural network. They determine the influence of one neuron on another. Initially, weights are set randomly or using specialized initialization techniques, such as Xavier or He initialization. Through training, these weights are adjusted to optimize the network’s performance.

\


Bias: Adjusting the Decision Boundary

\


Biases are additional parameters that help shift the activation function, improving the network’s flexibility to fit data patterns.

\


Example: Neural Network in Action

\


Consider a neural network designed to predict house prices based on features like square footage, number of bedrooms, and location.

\


1\. Input Layer:

• Neurons represent square footage, number of bedrooms, and a location encoding.

2\. Hidden Layer:

• Processes these inputs to learn patterns, such as how square footage influences price.

3\. Output Layer:

• Produces a single output: the predicted price.

\


PHP Implementation

\


Using the Rubix ML library, we can create a basic neural network:

\


use Rubix\ML\Classifiers\MLPClassifier;

use Rubix\ML\Datasets\Labeled;

use Rubix\ML\CrossValidation\Reports\MulticlassBreakdown;

\


// Define the training dataset

$samples = \[

&#x20;   \[1200, 3, 1], // square footage, bedrooms, location code

&#x20;   \[2000, 4, 2],

];

$labels = \['200000', '350000']; // House prices

\


$dataset = Labeled::build($samples, $labels);

\


// Define the neural network

$estimator = new MLPClassifier(\[

&#x20;   10, // 10 neurons in the hidden layer

], 100, new Sigmoid());

\


// Train the model

$estimator->train($dataset);

\


// Make predictions

$predicted = $estimator->predict(\[\[1500, 3, 1]]);

echo "Predicted Price: $predicted\[0]";

\


This example highlights how neurons, layers, and connections come together in practice.

\


Conclusion

\


Understanding the basic components of neural networks—neurons, layers, and connections—is crucial to mastering how they function. Each component plays a vital role in processing information, transforming raw data into actionable insights. By manipulating these elements, neural networks achieve remarkable feats in AI, from diagnosing diseases to driving cars.

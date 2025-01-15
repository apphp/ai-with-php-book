# Connections and Weights ?..

### Connections Between Neurons

Neurons are interconnected, with each connection represented by a weight. These connections determine how information flows through the network.

* Forward Propagation:\
  \
  During forward propagation, inputs are passed through the network layer by layer, with each connection applying its weight to the input data.
* Backpropagation:\
  \
  During training, errors are propagated backward through the network, and weights are updated to minimize the error using techniques like gradient descent.

### Weights: The Learnable Parameters

Weights are the core learnable components of a neural network. They determine the influence of one neuron on another. Initially, weights are set randomly or using specialized initialization techniques, such as Xavier or He initialization. Through training, these weights are adjusted to optimize the network’s performance.

### Bias: Adjusting the Decision Boundary

Biases are additional parameters that help shift the activation function, improving the network’s flexibility to fit data patterns.

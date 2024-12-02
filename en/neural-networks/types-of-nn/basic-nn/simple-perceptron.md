# Simple Perceptron

### Structure of a Simple Perceptron

A Simple Perceptron consists of:

1. **Input Layer:** Takes the input features as a vector $$\mathbf{x} = [x_1, x_2, \dots, x_n]$$.
2. **Weights:** Each input is associated with a weight , which determines the input’s influence on the perceptron.
3. **Bias Term ():** Adds flexibility by shifting the activation function.
4. **Summation Function:** Computes the weighted sum of inputs: $$z = \sum_{i=1}^n w_i x_i + b$$
5. **Activation Function:** Applies a threshold to the weighted sum to determine the output: \
   \
   $$y = \begin{cases} 1 & \text{if } z \geq 0 \\ 0 & \text{if } z < 0 \end{cases}$$

<div align="left"><figure><img src="../../../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1) (1).png" alt="" width="563"><figcaption></figcaption></figure></div>

### Working Mechanism

1. **Initialization:** Randomly initialize the weights and bias.
2. **Forward Pass:** Compute the perceptron’s output using the summation and activation functions.
3. **Error Calculation:** Compare the predicted output  with the actual label .
4. **Weight Update:** Adjust the weights based on the error using the perceptron learning rule: $$w_i = w_i + \eta \cdot (y_{\text{true}} - y) \cdot x_i$$\
   Here, $$\eta$$ is the learning rate.
5. **Iteration:** Repeat steps 2-4 for a specified number of epochs or until convergence.

#### Strengths of the Simple Perceptron

* Efficiency: Computationally lightweight and easy to implement.
* Binary Classification: Effective for linearly separable problems, such as distinguishing between two classes.

### Limitations

* Linear Separability: Fails for datasets that are not linearly separable (e.g., XOR problem).
* Limited Capability: Cannot handle complex patterns or nonlinear decision boundaries.

### Conclusion

The Simple Perceptron is a key stepping stone in machine learning, laying the foundation for more sophisticated models like multi-layer perceptrons and deep neural networks. While it has limitations, understanding its structure and function is crucial for grasping the evolution of neural networks.

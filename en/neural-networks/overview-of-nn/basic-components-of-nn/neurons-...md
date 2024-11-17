# Neurons ?..

### What Are Neurons?

The Fundamental Processing Units. In artificial neural networks, neurons, also known as nodes or units, are computational units that mimic the biological neurons of the brain. They take input, process it, and produce an output that is passed to the next layer of the network.

### Difference Between Biological Neurons and Artificial Neurons

Biological neurons and artificial neurons share some conceptual similarities but are fundamentally different in structure, function, and operation. Here's a comparison:

#### **Biological Neurons**

<div align="left">

<figure><img src="../../../.gitbook/assets/image.png" alt=""><figcaption><p>Biological Neurons</p></figcaption></figure>

</div>

* **Major Components**: Axons, dendrites, and synapses.
* **Functioning**: Electrical impulses from other neurons enter dendrites at synapses, where they are processed in the cell body. The processed signal is transmitted via the axon to synapses connected to other neurons.
* **Information Storage**: Synapses can strengthen or weaken connections, enabling dynamic information storage.
* **Scale**: The human brain contains approximately  neurons.

#### **Artificial Neurons**

<div align="left">

<figure><img src="../../../.gitbook/assets/image (1).png" alt="" width="563"><figcaption></figcaption></figure>

</div>

* **Structure**: Modeled after biological neurons, with layers representing axons, dendrites, and synapses.
* **Layered Network**:
  * **Input Layer**: Receives external signals.
  * **Hidden Layer**: Processes signals, extracting relevant patterns or features.
  * **Output Layer**: Generates the final output of the network.
* **Information Processing**: Signal strength is modulated by weights, akin to synaptic adjustments in biological neurons.
* **Scale**: Current artificial neural networks (ANNs) typically contain $$10^2 - 10^4$$ neurons.

#### **Comparison: Human Brain vs. Artificial Neural Networks (ANNs)**

|                          |                                                                      |                                                       |
| ------------------------ | -------------------------------------------------------------------- | ----------------------------------------------------- |
| **Feature**              | **Human Brain (Biological Network)**                                 | **Computers (Artificial Neural Network)**             |
| **Processing Style**     | Asynchronous                                                         | Synchronous                                           |
| **Speed of Computation** | Slow (\~milliseconds per computation)                                | Fast (\~nanoseconds per computation)                  |
| **Reliability**          | Distributed representation; tolerates neuron failure                 | Sensitive to errors; every bit must function          |
| **Adaptability**         | Connectivity changes over time based on new information and demands. | Fixed connectivity unless components are replaced.    |
| **Topology**             | Complex and highly interconnected                                    | Typically organized in simpler, tree-like structures. |
| **Learning Mechanism**   | Still under research                                                 | Relies on Gradient Descent.                           |

While artificial neurons emulate some aspects of biological neurons, the human brain's complexity, adaptability, and distributed resilience far surpass current artificial networks.

### Neuron Functionality

Each neuron performs the following operations:

#### **1. Receive Input:**&#x20;

A neuron receives one or more inputs, each with an associated weight.

#### **2. Weighted Sum Calculation:**&#x20;

The inputs are multiplied by their respective weights and summed.&#x20;

$$z = \sum_{i=1}^n w_i \cdot x_i + b$$

Here:

* $$x_i$$ is the input.
* $$w_i$$ is the weight for the -th input.
* $$b$$ is the bias term, which allows the neuron to shift its activation.

#### **3. Activation Function Application:**&#x20;

The result  is passed through an activation function to introduce non-linearity, enabling the network to solve complex problems. Common activation functions include:

* Sigmoid: $$f(z) = \frac{1}{1 + e^{-z}}$$
* ReLU (Rectified Linear Unit): $$f(z) = \max(0, z)$$
* Tanh: $$f(z) = \frac{e^z - e^{-z}}{e^z + e^{-z}}$$

### Role of Neurons in Information Processing

Neurons act as decision-makers. By adjusting the weights and biases, they learn patterns from data, enabling tasks like image recognition, language translation, and more. The combination of neurons in layers allows neural networks to model increasingly complex relationships.

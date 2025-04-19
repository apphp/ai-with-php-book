# Linear vs Non-Linear Problems in NN

A single neuron (or a single-layer perceptron) can only solve linearly separable problems. This means it can find a straight line (or a hyperplane in higher dimensions) that separates data into distinct classes. For example, it can distinguish between two types of points if they lie on opposite sides of a straight line. However, it fails with more complex problems—like the classic XOR problem—where no straight line can separate the classes.

To handle non-linear problems, we need a multi-layer neural network, also known as a multi-layer perceptron (MLP). By stacking layers of neurons with non-linear activation functions (like ReLU or sigmoid), the network gains the ability to learn and represent highly complex, non-linear decision boundaries.

In fact, it has been mathematically proven that a neural network with at least one hidden layer and non-linear activation can approximate any continuous function to any desired level of accuracy, given enough neurons. This property is known as the Universal Approximation Theorem. It shows why deep learning is so powerful: multi-layer networks are capable of modeling extremely complex relationships in data that a single neuron could never handle.

....

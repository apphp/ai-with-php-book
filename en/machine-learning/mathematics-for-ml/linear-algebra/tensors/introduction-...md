# Introduction ?..

## Tensors

### Introduction

Tensors are the fundamental data structures in modern machine learning, generalizing scalars, vectors, and matrices to higher dimensions. They form the backbone of deep learning frameworks like PyTorch and TensorFlow.

### Understanding Tensor Order

A tensor's order (or rank) refers to the number of dimensions it contains:

* Order 0: Scalar (single number)
* Order 1: Vector (1D array)
* Order 2: Matrix (2D array)
* Order 3+: Higher-dimensional arrays

#### Visual Representation

```
Scalar (Order 0):  3
Vector (Order 1):  [1, 2, 3]
Matrix (Order 2):  [[1, 2],
                    [3, 4]]
3D Tensor (Order 3): [[[1, 2],
                       [3, 4]],
                      [[5, 6],
                       [7, 8]]]
```

### Key Properties

#### Shape

The shape of a tensor describes its dimensions. For example:

* Vector: (n,)
* Matrix: (m, n)
* 3D Tensor: (l, m, n)
* Batch of Images: (batch\_size, height, width, channels)

#### Operations

**1. Element-wise Operations**

* Addition: C\[i,j,k] = A\[i,j,k] + B\[i,j,k]
* Multiplication: C\[i,j,k] = A\[i,j,k] \* B\[i,j,k]
* Activation Functions: Applied element-wise

**2. Reduction Operations**

* Sum across dimensions
* Mean across dimensions
* Maximum/minimum values

**3. Linear Transformations**

* Matrix multiplication
* Tensor contraction
* Convolution operations

### Common Applications in Machine Learning

#### 1. Neural Networks

* Input tensors: Batches of data
* Weight tensors: Learnable parameters
* Activation tensors: Layer outputs

#### 2. Computer Vision

* Image tensors: (batch\_size, height, width, channels)
* Convolutional kernels: (kernel\_height, kernel\_width, in\_channels, out\_channels)
* Feature maps: Outputs of convolutional layers

#### 3. Natural Language Processing

* Word embeddings: (vocabulary\_size, embedding\_dim)
* Sequence data: (batch\_size, sequence\_length, feature\_dim)
* Attention matrices: (batch\_size, num\_heads, sequence\_length, sequence\_length)

### Tensor Manipulation

#### 1. Reshaping

```python
# Example in PyTorch
x = torch.randn(10, 20)  # 2D tensor
y = x.reshape(200)       # Flatten to 1D
z = x.view(2, 10, 10)    # Reshape to 3D
```

#### 2. Broadcasting

Rules for operating on tensors with different shapes:

1. Add leading dimensions of size 1
2. Expand dimensions of size 1 to match larger size
3. Shapes must be compatible after expansion

#### 3. Indexing and Slicing

```python
# Example in NumPy
x = np.random.randn(4, 3, 2)
slice_1 = x[1:3, :, :]    # Select specific items
slice_2 = x[:, None, :]   # Add new dimension
```

### Best Practices

1. **Memory Management**
   * Use appropriate data types
   * Release unused tensors
   * Utilize in-place operations when possible
2. **Performance Optimization**
   * Batch operations when possible
   * Use appropriate device placement (CPU/GPU)
   * Minimize unnecessary copying between devices
3. **Debugging Tips**
   * Check tensor shapes before operations
   * Monitor memory usage
   * Use framework-specific debugging tools

### Common Pitfalls

1. **Shape Mismatches**
   * Incorrect tensor dimensions in operations
   * Broadcasting failures
   * Batch dimension inconsistencies
2. **Memory Issues**
   * Out of memory errors
   * Memory leaks from retained references
   * Inefficient tensor operations
3. **Numerical Stability**
   * Overflow/underflow in calculations
   * NaN propagation
   * Gradient explosion/vanishing

### Further Reading

* Deep Learning Frameworks Documentation
* Linear Algebra in Machine Learning
* Tensor Calculus
* Scientific Computing with Tensors

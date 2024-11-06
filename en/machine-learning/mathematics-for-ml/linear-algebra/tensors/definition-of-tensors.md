# Definition of Tensors

### Visual Representation

To illustrate a tensor in comparison to other mathematical quantities, let’s look at the hierarchy of dimensions in terms of their structure and complexity:

1\. Scalar (0D): A single number that represents magnitude only, such as a temperature or a count, with no direction.

A single value, e.g., $$3$$

2\. Vector (1D): A quantity with both magnitude and direction, represented as an ordered list of scalars. Think of a vector as a point in a straight line with n coordinates (e.g., force in physics).

A list of values, e.g., \[1, 2, 3], where each element has a position along one dimension.

&#x20;$$\begin{bmatrix} 1 & 2 & 3 \end{bmatrix}$$

3\. Matrix (2D): A 2-dimensional array of numbers, organized into rows and columns. Matrices are useful for transformations in linear algebra, such as rotations and scaling in 2D or 3D space.

A 2D grid of values, e.g.,&#x20;

$$\begin{bmatrix} 1 & 2 \\ 3 & 4 \end{bmatrix}$$

4\. Tensor (3D and higher): A multidimensional array that generalizes scalars, vectors, and matrices to higher dimensions. Tensors can have multiple dimensions (also called “orders” or “ranks”), which can represent complex relationships among data points in machine learning, physics, or computer vision.

A collection of matrices stacked in a third dimension, e.g.,&#x20;

$$\begin{bmatrix} \begin{bmatrix} 1 & 2 \\ 3 & 4 \end{bmatrix}, \begin{bmatrix} 5 & 6 \\ 7 & 8 \end{bmatrix} \end{bmatrix}$$

This breakdown presents the increasing complexity of each structure:

| Dimensionality | Representation | Representation              |
| -------------- | -------------- | --------------------------- |
| Scalar         | 0D             | Temperature, count          |
| Vector         | 1D             | Force, velocity             |
| Matrix         | 2D             | Image processing, rotations |
| Tensor         | 3D and higher  | Deep learning, physics      |

### Formal Definitions

#### 1. Multilinear Map Definition

A tensor is formally defined as a multilinear map from a product of vector spaces to a field F (typically real numbers ℝ):

$$T: V_1^* \times V_2^* \times \ldots \times V_n^* \times W_1 \times W_2 \times \ldots \times W_m \rightarrow F$$

where:

* $$V_1^* \quad V_2^*$$ etc. are dual vector spaces
* $$W_1, \quad W_2$$ etc. are vector spaces
* The tensor is contravariant in the $$W$$ arguments
* The tensor is covariant in the $$V^*$$ arguments

#### 2. Component-Based Definition

A tensor of rank $$(m,n)$$ has components that transform according to: $$T{\prime}{i_1 \ldots i_m j_1 \ldots j_n} = \sum{k_1 \ldots k_m \ell_1 \ldots \ell_n} \left( \frac{\partial x{\prime}{i_1}}{\partial x{k_1}} \right) \ldots \left( \frac{\partial x{\prime}{i_m}}{\partial x{k_m}} \right) \left( \frac{\partial x_{\ell_1}}{\partial x{\prime}{j_1}} \right) \ldots \left( \frac{\partial x{\ell_n}}{\partial x{\prime}{j_n}} \right) T{k_1 \ldots k_m \ell_1 \ldots \ell_n}$$

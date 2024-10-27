# Definition of Tensors

### Visual Representation

To illustrate a tensor in comparison to other mathematical quantities, let’s look at the hierarchy of dimensions in terms of their structure and complexity:

1\. Scalar (0D): A single number that represents magnitude only, such as a temperature or a count, with no direction.

A single value, e.g., 3.

```
3
```

2\. Vector (1D): A quantity with both magnitude and direction, represented as an ordered list of scalars. Think of a vector as a point in a straight line with n coordinates (e.g., force in physics).

A list of values, e.g., \[1, 2, 3], where each element has a position along one dimension.

```
[1, 2, 3]
```

3\. Matrix (2D): A 2-dimensional array of numbers, organized into rows and columns. Matrices are useful for transformations in linear algebra, such as rotations and scaling in 2D or 3D space.

A 2D grid of values, e.g.,

```
[[1, 2],
 [3, 4]]
```

4\. Tensor (3D and higher): A multidimensional array that generalizes scalars, vectors, and matrices to higher dimensions. Tensors can have multiple dimensions (also called “orders” or “ranks”), which can represent complex relationships among data points in machine learning, physics, or computer vision.

A collection of matrices stacked in a third dimension, e.g.,

```
[[[1, 2],
  [3, 4]],
 [[5, 6],
  [7, 8]]]
```

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

$$T: V₁* × V₂* × ... × Vₙ* × W₁ × W₂ × ... × Wₘ → F$$

where:

* $$V₁*, V₂*,$$ etc. are dual vector spaces
* $$W₁, W₂,$$ etc. are vector spaces
* The tensor is contravariant in the $$W$$ arguments
* The tensor is covariant in the $$V*$$ arguments

#### 2. Component-Based Definition

A tensor of rank $$(m,n)$$ has components that transform according to:

$$T'ᵢ₁...ᵢₘⱼ₁...ⱼₙ = ∑ₖ₁...ₖₘₗ₁...ₗₙ (∂x'ᵢ₁/∂xₖ₁)...(∂x'ᵢₘ/∂xₖₘ)(∂xₗ₁/∂x'ⱼ₁)...(∂xₗₙ/∂x'ⱼₙ) Tₖ₁...ₖₘₗ₁...ₗₙ$$


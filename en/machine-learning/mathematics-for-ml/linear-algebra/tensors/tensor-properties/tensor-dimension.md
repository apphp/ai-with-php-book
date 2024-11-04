# Tensor Dimension

### Tensor Dimension

A tensor's dimension refers to the number of indices required to uniquely specify an element within the tensor. Mathematically, it can be expressed as:

#### **Formal Definition**:&#x20;

For a tensor T, its dimension n is the number of independent indices $$(i₁, i₂, ..., iₙ)$$ needed to specify any component $$Tᵢ₁ᵢ₂...ᵢₙ$$

#### Mathematical Examples:

* Scalar (0D):  $$a$$
* Vector (1D):  $$a_i$$
* Matrix (2D):  $$a_{ij}$$
* 3D Tensor:  $$a_{ijk}$$&#x20;

#### Real Mathematical Examples:

* CopyScalar: 5
* Vector: $$v = [2, 3, 4]$$
* Matrix:\
  $$\mathbf{M} = \begin{bmatrix} 1 & 2 \\ 3 & 4 \end{bmatrix}$$
* 3D Tensor:\
  $$\mathbf{T} = \begin{bmatrix} \begin{bmatrix} 1 & 2 \\ 3 & 4 \end{bmatrix}, \begin{bmatrix} 5 & 6 \\ 7 & 8 \end{bmatrix} \end{bmatrix}$$

#### Classification by Dimension

**a. Scalar (0-dimension)**

* **Definition**: No indices needed
* **Notation**: $$s$$ or $$s₀$$
* **Examples**:
  * Temperature: 25°C
  * Mass: 5 kg
  * Potential: $$V = 10V$$

**b. Vector (1-dimension)**

* **Definition**: One index needed
* **Notation**: $$vᵢ$$ or $$v[i]$$
* **Examples**:
  * Position vector: $$r = [x, y, z]$$
  * Force vector: $$F = [Fx, Fy, Fz]$$
  * Momentum: $$p = [px, py, pz]$$

#### c. Matrix (2-dimension)

* **Definition**: Two indices needed
* **Notation**: $$Mᵢⱼ$$ or $$M[i,j]$$
* **Examples**:\
  \
  Stress tensor:\
  $$\sigma_{ij} = \begin{bmatrix} \sigma_{xx} & \sigma_{xy} & \sigma_{xz} \\ \sigma_{yx} & \sigma_{yy} & \sigma_{yz} \\ \sigma_{zx} & \sigma_{zy} & \sigma_{zz} \end{bmatrix}$$\
  \
  Inertia tensor:\
  $$I_{ij} = \begin{bmatrix} I_{xx} & I_{xy} & I_{xz} \\ I_{yx} & I_{yy} & I_{yz} \\ I_{zx} & I_{zy} & I_{zz} \end{bmatrix}$$

**d. 3-Dimensional Tensor**

* **Definition**: Three indices needed
* **Notation**: $$Tᵢⱼₖ$$ or $$T[i,j,k]$$
* **Examples**:
  * Piezoelectric tensor: $$d_{ᵢⱼₖ}$$
  * Elastic stiffness tensor: $$C_{ᵢⱼₖₗ}$$

#### Mathematical Properties Based on Dimension

**a. Transformation Rules**

For a tensor $$T$$ of dimension $$n$$, under coordinate transformation $$R$$:

$$T{\prime}{i_1 i_2 \ldots i_n} = R{i_1 j_1} R_{i_2 j_2} \ldots R_{i_n j_n} T_{j_1 j_2 \ldots j_n}$$

**b. Components Count**

For a tensor in n-dimensional space:

* 0D (Scalar): $$1$$ component
* 1D (Vector): $$n$$ components
* 2D (Matrix): $$n²$$ components
* 3D Tensor: $$n³$$ components

#### Mathematical Operations by Dimension

**a. Inner Products**

* Vectors (1D): $$\mathbf{a} \cdot \mathbf{b} = \sum_{i} a_i b_i$$
* Matrices (2D): $$\mathbf{A} : \mathbf{B} = \sum_{i,j} A_{ij} B_{ij}$$
* 3D Tensors: $$T_1 \cdot T_2 = \sum_{i,j,k} T_{1_{ijk}} T_{2_{ijk}}$$

**b. Outer Products**

* Vectors to Matrix: $$(a \otimes b)_{ij} = a_i b_j$$
* Matrices to 4D Tensor: $$(A \otimes B){ijkl} = A{ij} B_{kl}$$

### Implementation with PHP

<details>

<summary>Tensor Dimensions</summary>

```php
<?php

// 0-dimensional tensor (Scalar)
$scalar = 5;

// 1-dimensional tensor (Vector)
$vector = [1, 2, 3];

// 2-dimensional tensor (Matrix)
$matrix = [
    [1, 2, 3],
    [4, 5, 6]
];

// 3-dimensional tensor
$tensor_3d = [
    [[1, 2], [3, 4]],
    [[5, 6], [7, 8]]
];

// Function to determine dimension
function getTensorDimension($tensor) {
    if (!is_array($tensor)) {
        return 0;  // Scalar
    }
    
    $dim = 1;
    $current = $tensor;
    
    while (is_array(current($current))) {
        $dim++;
        $current = current($current);
    }
    
    return $dim;
}

// Examples
echo "Scalar dimension: " . getTensorDimension($scalar) . "\n";  // 0
echo "Vector dimension: " . getTensorDimension($vector) . "\n";  // 1
echo "Matrix dimension: " . getTensorDimension($matrix) . "\n";  // 2
echo "3D Tensor dimension: " . getTensorDimension($tensor_3d) . "\n";  // 3
```

</details>

# Tensor Dimension

### Tensor Dimension

A tensor's dimension refers to the number of indices required to uniquely specify an element within the tensor. Mathematically, it can be expressed as:

#### **Formal Definition**:&#x20;

For a tensor T, its dimension n is the number of independent indices $$(i₁, i₂, ..., iₙ)$$ needed to specify any component $$Tᵢ₁ᵢ₂...ᵢₙ$$

#### Mathematical Examples:

```
Scalar (0D): a
Vector (1D): aᵢ
Matrix (2D): aᵢⱼ
3D Tensor:   aᵢⱼₖ
```

#### Real Mathematical Examples:

```
CopyScalar: 5
Vector:     v = [2, 3, 4]
Matrix:     M = [1 2]
                [3 4]
3D Tensor:  T = [[[1,2], [3,4]],
                [[5,6], [7,8]]]
```

#### Classification by Dimension

**a. Scalar (0-dimension)**

* **Definition**: No indices needed
* **Notation**: $$s$$ or $$s₀$$
* **Examples**:

```
Temperature: 25°C
Mass: 5 kg
Potential: V = 10V
```

**b. Vector (1-dimension)**

* **Definition**: One index needed
* **Notation**: $$vᵢ$$ or $$v[i]$$
* **Examples**:

```
Position vector: r = [x, y, z]
Force vector:    F = [Fx, Fy, Fz]
Momentum:        p = [px, py, pz]
```

#### c. Matrix (2-dimension)

* **Definition**: Two indices needed
* **Notation**: $$Mᵢⱼ$$ or $$M[i,j]$$
* **Examples**:

```
Stress tensor: σᵢⱼ = [σxx σxy σxz]
                     [σyx σyy σyz]
                     [σzx σzy σzz]

Inertia tensor: I = [Ixx Ixy Ixz]
                    [Iyx Iyy Iyz]
                    [Izx Izy Izz]
```

**d. 3-Dimensional Tensor**

* **Definition**: Three indices needed
* **Notation**: $$Tᵢⱼₖ$$ or $$T[i,j,k]$$
* **Examples**:

```
Piezoelectric tensor: dᵢⱼₖ
Elastic stiffness tensor: Cᵢⱼₖₗ
```

#### Mathematical Properties Based on Dimension

**a. Transformation Rules**

For a tensor $$T$$ of dimension $$n$$, under coordinate transformation $$R$$:

$$T'ᵢ₁ᵢ₂...ᵢₙ = Rᵢ₁ⱼ₁Rᵢ₂ⱼ₂...Rᵢₙⱼₙ Tⱼ₁ⱼ₂...ⱼₙ$$

```
T'ᵢ₁ᵢ₂...ᵢₙ = Rᵢ₁ⱼ₁Rᵢ₂ⱼ₂...Rᵢₙⱼₙ Tⱼ₁ⱼ₂...ⱼₙ
```

**b. Components Count**

For a tensor in n-dimensional space:

* 0D (Scalar): $$1$$ component
* 1D (Vector): $$n$$ components
* 2D (Matrix): $$n²$$ components
* 3D Tensor: $$n³$$ components

#### Mathematical Operations by Dimension

**a. Inner Products**

```
Vectors (1D): a·b = Σᵢ aᵢbᵢ
Matrices (2D): A:B = Σᵢⱼ AᵢⱼBᵢⱼ
3D Tensors: T₁·T₂ = Σᵢⱼₖ T₁ᵢⱼₖT₂ᵢⱼₖ
```

**b. Outer Products**

```
Vectors → Matrix: (a⊗b)ᵢⱼ = aᵢbⱼ
Matrices → 4D Tensor: (A⊗B)ᵢⱼₖₗ = AᵢⱼBₖₗ
```

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

Run this code by yourself:

{% embed url="https://onlinephp.io/c/9d623a0c-bda7-4ca8-ade9-f048c73a8a12" %}
Tensor Dimensions
{% endembed %}

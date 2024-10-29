# Tensor Dimension ?..

### 1. Tensor Dimension

A tensor's dimension refers to the number of indices required to uniquely specify an element within the tensor. Mathematically, it can be expressed as:

#### **1.1 Formal Definition**:&#x20;

For a tensor T, its dimension n is the number of independent indices (i₁, i₂, ..., iₙ) needed to specify any component Tᵢ₁ᵢ₂...ᵢₙ

#### 1.2 Mathematical Examples:

```
CopyScalar (0D): a
Vector (1D): aᵢ
Matrix (2D): aᵢⱼ
3D Tensor: aᵢⱼₖ
```

#### 1.3 Real Mathematical Examples:

```
CopyScalar: 5
Vector: v = [2, 3, 4]
Matrix: M = [1 2]
           [3 4]
3D Tensor: T = [[[1,2], [3,4]],
                [[5,6], [7,8]]]
```

#### 1.4 Classification by Dimension

#### a. Scalar (0-dimension)

* **Definition**: No indices needed
* **Notation**: s or s₀
* **Examples**:

```
CopyTemperature: 25°C
Mass: 5 kg
Potential: V = 10V
```

#### b. Vector (1-dimension)

* **Definition**: One index needed
* **Notation**: vᵢ or v\[i]
* **Examples**:

```
CopyPosition vector: r = [x, y, z]
Force vector: F = [Fx, Fy, Fz]
Momentum: p = [px, py, pz]
```

#### c. Matrix (2-dimension)

* **Definition**: Two indices needed
* **Notation**: Mᵢⱼ or M\[i,j]
* **Examples**:

```
CopyStress tensor: σᵢⱼ = [σxx σxy σxz]
               [σyx σyy σyz]
               [σzx σzy σzz]

Inertia tensor: I = [Ixx Ixy Ixz]
                    [Iyx Iyy Iyz]
                    [Izx Izy Izz]
```

#### d. 3-Dimensional Tensor

* **Definition**: Three indices needed
* **Notation**: Tᵢⱼₖ or T\[i,j,k]
* **Examples**:

```
CopyPiezoelectric tensor: dᵢⱼₖ

Elastic stiffness tensor: Cᵢⱼₖₗ
```

#### 1.5 Mathematical Properties Based on Dimension

**a. Transformation Rules**

For a tensor T of dimension n, under coordinate transformation R:

```
CopyT'ᵢ₁ᵢ₂...ᵢₙ = Rᵢ₁ⱼ₁Rᵢ₂ⱼ₂...Rᵢₙⱼₙ Tⱼ₁ⱼ₂...ⱼₙ
```

#### b. Components Count

For a tensor in n-dimensional space:

* 0D (Scalar): 1 component
* 1D (Vector): n components
* 2D (Matrix): n² components
* 3D Tensor: n³ components

### 1.6 Mathematical Operations by Dimension

#### a. Inner Products

```
Vectors (1D): a·b = Σᵢ aᵢbᵢ
Matrices (2D): A:B = Σᵢⱼ AᵢⱼBᵢⱼ
3D Tensors: T₁·T₂ = Σᵢⱼₖ T₁ᵢⱼₖT₂ᵢⱼₖ
```

#### b. Outer Products

```
Vectors → Matrix: (a⊗b)ᵢⱼ = aᵢbⱼ
Matrices → 4D Tensor: (A⊗B)ᵢⱼₖₗ = AᵢⱼBₖₗ
```



### 5. Physical Examples

#### 5.1 Electromagnetic Field Tensor (2D)

```
CopyFμν = [0   -Ex  -Ey  -Ez ]
      [Ex   0   -Bz  By  ]
      [Ey   Bz   0   -Bx ]
      [Ez  -By   Bx   0  ]
```

#### 5.2 Riemann Curvature Tensor (4D)

```
CopyRᵢⱼₖₗ (20 independent components in 4D spacetime)
```

#### 5.3 Elastic Modulus Tensor (4D)

```
CopyCᵢⱼₖₗ (81 components, reduced to 21 by symmetry)
```

### 6. Applications in Physics

#### 6.1 Quantum Mechanics

* Wavefunction: ψ(x,t) - Complex scalar field
* Density matrix: ρᵢⱼ - 2D tensor
* Spin operators: Sᵢ - Vector operators

#### 6.2 General Relativity

* Metric tensor: gμν - 2D tensor
* Einstein tensor: Gμν - 2D tensor
* Ricci tensor: Rμν - 2D tensor

#### 6.3 Continuum Mechanics

* Strain tensor: εᵢⱼ - 2D tensor
* Stress tensor: σᵢⱼ - 2D tensor
* Elasticity tensor: Cᵢⱼₖₗ - 4D tensor





Implementation with PHP

<details>

<summary>Tensor Dimension</summary>

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

...

# Tensor Shape

### 1. Mathematical Definition

The shape of a tensor is an ordered tuple (n₁, n₂, ..., nₖ) where each nᵢ represents the size of the i-th dimension. It describes the number of elements along each dimension of the tensor.

### 2. Mathematical Examples by Dimensionality

#### 2.1 Scalar (Shape: ())

```mathematica
Shape: ()
Example: 5
Components: Single value
```

#### 2.2 Vector (Shape: (n))

```mathematica
Shape: (3)
Examples:
v = [1]
    [2]
    [3]

Basis vectors in ℝ³:
e₁ = [1]  e₂ = [0]  e₃ = [0]
    [0]       [1]       [0]
    [0]       [0]       [1]
```

#### 2.3 Matrix (Shape: (m,n))

```mathematica
Shape: (2,3)
Example:
A = [1 2 3]
    [4 5 6]

Shape: (3,3)
Identity Matrix:
I = [1 0 0]
    [0 1 0]
    [0 0 1]
```

#### 2.4 3D Tensor (Shape: (l,m,n))

```mathematica
Shape: (2,2,3)
Example:
T = [[[1,2,3], [4,5,6]],
     [[7,8,9], [10,11,12]]]

RGB Image Example (Shape: (height,width,3)):
Shape: (2,3,3)
[[[255,0,0], [0,255,0], [0,0,255]],
 [[128,0,0], [0,128,0], [0,0,128]]]
```

### 3. Mathematical Operations with Shapes

#### 3.1 Reshaping Operations

```mathematica
Original shape: (6,) → vector
[1, 2, 3, 4, 5, 6]

Reshaped to (2,3) → matrix
[1 2 3]
[4 5 6]

Reshaped to (3,2) → matrix
[1 2]
[3 4]
[5 6]
```

#### 3.2 Shape Compatibility in Operations

```mathematica
Matrix Multiplication (A × B):
A shape: (m,n)
B shape: (n,p)
Result shape: (m,p)

Example:
(2,3) × (3,2) → (2,2)
[1 2 3]   [7 8]   [58  64]
[4 5 6] × [9 10] = [139 154]
          [11 12]
```

#### 3.3 Broadcasting Rules

```mathematica
Vector + Scalar:
Shape (3,) + () → (3,)
[1]     [4]
[2] + 4 = [6]
[3]     [7]

Matrix + Vector:
Shape (2,3) + (3,) → (2,3)
[1 2 3]   [1]   [2 3 4]
[4 5 6] + [1] = [5 6 7]
          [1]
```

### 4. Special Tensor Shapes in Mathematics

#### 4.1 Square Matrices (n,n)

```mathematica
Shape: (2,2)
[a b]
[c d]

Shape: (3,3)
[a b c]
[d e f]
[g h i]
```

#### 4.2 Diagonal Matrices

```mathematica
Shape: (3,3)
[λ₁ 0  0 ]
[0  λ₂ 0 ]
[0  0  λ₃]
```

#### 4.3 Symmetric Tensors

```mathematica
Shape: (3,3)
[a  b  c]
[b  d  e]
[c  e  f]
```

### 5. Applications with Specific Shapes

#### 5.1 Physics Examples

```mathematica
Stress Tensor (Shape: (3,3)):
[σxx σxy σxz]
[σyx σyy σyz]
[σzx σzy σzz]

Electromagnetic Field Tensor (Shape: (4,4)):
[0   -Ex  -Ey  -Ez ]
[Ex   0   -Bz  By  ]
[Ey   Bz   0   -Bx ]
[Ez  -By   Bx   0  ]
```

#### 5.2 Linear Algebra Examples

```mathematica
yRotation Matrix (Shape: (3,3)):
[cos θ  -sin θ  0]
[sin θ   cos θ  0]
[0       0      1]

Projection Matrix (Shape: (n,n)):
P = A(AᵀA)⁻¹Aᵀ
```

#### 5.3 Data Science Examples

```mathematica
Feature Matrix (Shape: (samples, features)):
Shape: (3,4)
[x₁₁ x₁₂ x₁₃ x₁₄]
[x₂₁ x₂₂ x₂₃ x₂₄]
[x₃₁ x₃₂ x₃₃ x₃₄]

Neural Network Layer (Shape: (batch_size, neurons)):
Shape: (2,3)
[n₁₁ n₁₂ n₁₃]
[n₂₁ n₂₂ n₂₃]
```

### 6. Shape Transformations

#### 6.1 Transpose Operation

```mathematica
Original Shape: (2,3)
[1 2 3]
[4 5 6]

Transposed Shape: (3,2)
[1 4]
[2 5]
[3 6]
```

#### 6.2 Flatten Operation

```mathematica
Original Shape: (2,2,2)
[[[1,2],
  [3,4]],
 [[5,6],
  [7,8]]]

Flattened Shape: (8,)
[1,2,3,4,5,6,7,8]
```

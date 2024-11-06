# Tensor Shape ?..

### 1. Mathematical Definition

The shape of a tensor is an ordered tuple (n₁, n₂, ..., nₖ) where each nᵢ represents the size of the i-th dimension. It describes the number of elements along each dimension of the tensor.

### 2. Mathematical Examples by Dimensionality

#### 2.1 Scalar (Shape: ())

Shape: ()&#x20;

Example: 5&#x20;

Components: Single value

#### 2.2 Vector (Shape: (n))

Shape: (3)

Example: $$v = \begin{bmatrix} 1 \\ 2 \\ 3 \end{bmatrix}$$

Basis vectors in ℝ³:  $$e_1 = \begin{bmatrix} 1 \\ 0 \\ 0 \end{bmatrix}, \quad e_2 = \begin{bmatrix} 0 \\ 1 \\ 0 \end{bmatrix}, \quad e_3 = \begin{bmatrix} 0 \\ 0 \\ 1 \end{bmatrix}$$

#### 2.3 Matrix (Shape: (m,n))

Shape: (2,3)

Example: $$A = \begin{bmatrix} 1 & 2 & 3 \\ 4 & 5 & 6 \end{bmatrix}$$

Shape: (3,3)

Identity Matrix: $$I = \begin{bmatrix} 1 & 0 & 0 \\ 0 & 1 & 0 \\ 0 & 0 & 1 \end{bmatrix}$$

#### 2.4 3D Tensor (Shape: (l,m,n))

Shape: (2,2,3)

Example: $$T = \begin{bmatrix} \begin{bmatrix} 1 & 2 & 3 \\ 4 & 5 & 6 \end{bmatrix}, \begin{bmatrix} 7 & 8 & 9 \\ 10 & 11 & 12 \end{bmatrix} \end{bmatrix}$$

RGB Image Example (Shape: (height, width,3)):

Shape: (2,3,3)

$$T = \begin{bmatrix} \begin{bmatrix} 255 & 0 & 0 \\ 0 & 255 & 0 \\ 0 & 0 & 255 \end{bmatrix}, \begin{bmatrix} 128 & 0 & 0 \\ 0 & 128 & 0 \\ 0 & 0 & 128 \end{bmatrix} \end{bmatrix}$$

### 3. Mathematical Operations with Shapes

#### 3.1 Reshaping Operations

Original shape: (4,) → vector $$\begin{bmatrix} 1 \\ 2 \\ 3 \\ 4  \end{bmatrix}$$

Reshaped to (2,3) → matrix $$\begin{bmatrix} 1 & 2 & 3 \\ 4 & 5 & 6 \end{bmatrix}$$

Reshaped to (3,2) → matrix $$\begin{bmatrix} 1 & 2 \\ 3 & 4 \\ 5 & 6 \end{bmatrix}$$

#### 3.2 Shape Compatibility in Operations

Matrix Multiplication ($$A×B$$):

A shape: $$(m,n)$$

B shape: $$(n,p)$$

Result shape: $$(m,p)$$

Example: $$(2,3) \times (3,2) \rightarrow (2,2)$$&#x20;

$$\begin{bmatrix} 1 & 2 & 3 \\ 4 & 5 & 6 \end{bmatrix} \times \begin{bmatrix} 7 & 8 \\ 9 & 10 \\ 11 & 12 \end{bmatrix}  \begin{bmatrix} 58 & 64 \\ 139 & 154 \end{bmatrix}$$

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

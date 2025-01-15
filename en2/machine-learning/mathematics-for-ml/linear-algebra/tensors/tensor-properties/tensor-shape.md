# Tensor Shape

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

Vector + Scalar: Shape (3,) + () → (3,)

$$\begin{bmatrix} 1 \\ 2 \\ 3 \end{bmatrix} + 4 = \begin{bmatrix} 4 \\ 6 \\ 7 \end{bmatrix}$$

Matrix + Vector: Shape (2,3) + (3,) → (2,3)&#x20;

$$\begin{bmatrix} 1 & 2 & 3 \\ 4 & 5 & 6 \end{bmatrix} + \begin{bmatrix} 1 \\ 1 \\ 1 \end{bmatrix} = \begin{bmatrix} 2 & 3 & 4 \\ 5 & 6 & 7 \end{bmatrix}$$

### 4. Special Tensor Shapes in Mathematics

#### 4.1 Square Matrices (n,n)

Shape: (2,2)

$$\begin{bmatrix} a & b \\ c & d \end{bmatrix}$$

Shape: (3,3)

$$\begin{bmatrix} a & b & c \\ d & e & f \\ g & h & i \end{bmatrix}$$

#### 4.2 Diagonal Matrices

Shape: (3,3)

$$\begin{bmatrix} \lambda_1 & 0 & 0 \\ 0 & \lambda_2 & 0 \\ 0 & 0 & \lambda_3 \end{bmatrix}$$

#### 4.3 Symmetric Tensors

This shows a symmetric matrix where the entries satisfy $$b_{ij} = b_{ji}$$ .

Shape: (3,3)

$$\begin{bmatrix} a & b & c \\ b & d & e \\ c & e & f \end{bmatrix}$$

### 5. Applications with Specific Shapes

#### 5.1 Physics Examples

Stress Tensor (Shape: (3,3)):

$$\begin{bmatrix} \sigma_{xx} & \sigma_{xy} & \sigma_{xz} \\ \sigma_{yx} & \sigma_{yy} & \sigma_{yz} \\ \sigma_{zx} & \sigma_{zy} & \sigma_{zz} \end{bmatrix}$$

Electromagnetic Field Tensor (Shape: (4,4)):

$$\begin{bmatrix} 0 & -E_x & -E_y & -E_z \\ E_x & 0 & -B_z & B_y \\ E_y & B_z & 0 & -B_x \\ E_z & -B_y & B_x & 0 \end{bmatrix}$$

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

Original Shape: (2,3)

$$\begin{bmatrix} 1 & 2 & 3 \\ 4 & 5 & 6 \end{bmatrix}$$

Transposed Shape: (3,2)

$$\begin{bmatrix} 1 & 4 \\ 2 & 5 \\ 3 & 6 \end{bmatrix}$$

#### 6.2 Flatten Operation

Original Shape: (2,2,2)

$$\begin{bmatrix} \begin{bmatrix} 1 & 2 \\ 3 & 4 \end{bmatrix}, \begin{bmatrix} 5 & 6 \\ 7 & 8 \end{bmatrix} \end{bmatrix}$$

Flattened Shape: (8,)&#x20;

$$\begin{bmatrix} 1 & 2 & 3 & 4 & 5 & 6 & 7 & 8 \end{bmatrix}$$

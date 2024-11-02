# Tensor Rank ?..

### 1. Tensor Rank (Order)

#### **Formal Definition**:&#x20;

The rank (or order) of a tensor is the number of independent directional components required to specify the tensor completely. Alternatively, it's the number of indices needed in the component representation.

* **Mathematical Form**: $$T ∈ ℝ$$
* **Examples**:

```mathematica
Temperature: T = 298K
Mass: m = 5kg
Pressure: P = 101.3 kPa
Energy: E = 10J
```

### 2. Classification by Rank

#### Rank 0 (Scalars)

* **Definition**: Single numbers, no directional components
* **Mathematical Form**: $$T ∈ ℝ$$
* **Examples**:

```nb
Temperature: T = 298K
Mass: m = 5kg
Pressure: P = 101.3 kPa
Energy: E = 10J
```

#### Rank 1 (Vectors)

* **Definition**: One directional component
* **Mathematical Form**: $$v ∈ ℝⁿ$$
* **Examples**:

```mathematica
Position vector: r = [x, y, z]
Velocity: v = [vx, vy, vz]
Force: F = [Fx, Fy, Fz]
Electric field: E = [Ex, Ey, Ez]
```

#### Rank 2 (Matrices)

* **Definition**: Two directional components
* **Mathematical Form**: $$M ∈ ℝⁿˣᵐ$$
* **Examples**:

```mathematica
1. Stress tensor:
σ = [σxx σxy σxz]
    [σyx σyy σyz]
    [σzx σzy σzz]

2. Inertia tensor:
I = [Ixx Ixy Ixz]
    [Iyx Iyy Iyz]
    [Izx Izy Izz]

3. Metric tensor:
g = [gtt gtx gty gtz]
    [gxt gxx gxy gxz]
    [gyt gyx gyy gyz]
    [gzt gzx gzy gzz]
```

#### Rank 3

* **Definition**: Three directional components
* **Mathematical Form**: $$T ∈ ℝⁿˣᵐˣᵏ$$
* **Examples**:

```mathematica
1. Piezoelectric tensor:
dijk = [d111 d112 d113]
       [d121 d122 d123]
       [d131 d132 d133]

2. Levi-Civita symbol:
εijk = +1 if (i,j,k) is (1,2,3), (2,3,1), or (3,1,2)
     = -1 if (i,j,k) is (3,2,1), (1,3,2), or (2,1,3)
     =  0 if any index is repeated
```

### 3. Mathematical Operations by Rank

#### 3.1 Rank Addition

Rank addition for tensors refers to the process of adding tensors with different ranks, resulting in a higher-rank tensor. In the context of tensors, _rank_ (or _order_) refers to the number of dimensions (or axes) in the tensor.

When adding tensors of different ranks, one of two main techniques is typically used:

**1. Broadcasting**:

* Broadcasting is a technique where lower-rank tensors are expanded to match the dimensions of higher-rank tensors during an operation like addition.
* For example, if we have a rank-2 tensor (a matrix) and add it to a rank-1 tensor (a vector), the rank-1 tensor can be “broadcast” to match the rank-2 shape.
* Suppose a rank-2 tensor has a shape of 3 x 3, and a rank-1 tensor has a shape of 3. Broadcasting replicates the rank-1 tensor across each row or column, allowing element-wise addition to occur.

**2. Tensor Stacking:**

* Stacking is a method where tensors of the same shape are combined along a new axis, increasing the rank by one.
* For instance, stacking two matrices (rank-2 tensors) of shape 3 x 3 results in a rank-3 tensor of shape 2 x 3 x 3. This is useful in cases where we want to preserve each tensor independently within a higher-rank structure, rather than perform element-wise addition.

**Key Points**

* Broadcasting efficiently aligns shapes, enabling element-wise operations between tensors of different ranks without needing explicit expansion of the lower-rank tensor.
* Stacking maintains individual tensor elements but increases rank, as it adds a new dimension.

Same rank tensors can be added:

```mathematica
Vectors (Rank 1):
[a1] + [b1] = [a1 + b1]
[a2]   [b2]   [a2 + b2]
[a3]   [b3]   [a3 + b3]

Matrices (Rank 2):
[A11 A12] + [B11 B12] = [A11+B11 A12+B12]
[A21 A22]   [B21 B22]   [A21+B21 A22+B22]
```

Rank Addition with Broadcasting

Consider a rank-1 tensor (vector) $$v = [1, 2, 3]$$\
and a rank-2 tensor (matrix) M: \
\
$$M = \begin{bmatrix} 4 & 5 & 6 \\ 7 & 8 & 9 \\ 10 & 11 & 12 \end{bmatrix}$$

Using broadcasting, $$v$$ can be added to each row of $$M$$ as follows:&#x20;

$$M + v = \begin{bmatrix} 4+1 & 5+2 & 6+3 \\ 7+1 & 8+2 & 9+3 \\ 10+1 & 11+2 & 12+3 \end{bmatrix} = \begin{bmatrix} 5 & 7 & 9 \\ 8 & 10 & 12 \\ 11 & 13 & 15 \end{bmatrix}$$

#### 3.2 Rank Multiplication

1. Vector × Vector → Scalar (Rank reduction):\
   $$a·b = Σᵢ aᵢbᵢ$$
2. Vector × Vector → Matrix (Rank increase):\
   $$(a⊗b)ᵢⱼ = aᵢbⱼ$$
3. Matrix × Vector → Vector:\
   $$(Av)ᵢ = Σⱼ Aᵢⱼvⱼ$$

### 4. Rank Change Operations

Rank multiplication for tensors involves performing multiplication operations between tensors of varying ranks, often resulting in a higher-rank tensor. The type of multiplication can vary depending on the application and can include operations such as element-wise multiplication, outer products, or tensor contractions.\


**1. Element-wise Multiplication:**

* Also known as the Hadamard product, element-wise multiplication occurs when tensors of the same shape (and thus the same rank) are multiplied. Each element in one tensor is multiplied by the corresponding element in the other tensor.
* Example: For two rank-2 tensors (matrices) A and B of shape 2 x 2 ,\
  \
  $$A = \begin{bmatrix} 1 & 2 \\ 3 & 4 \end{bmatrix}, \quad B = \begin{bmatrix} 5 & 6 \\ 7 & 8 \end{bmatrix}$$\
  \
  the element-wise multiplication results in\
  \
  $$A \circ B = \begin{bmatrix} 1 \cdot 5 & 2 \cdot 6 \\ 3 \cdot 7 & 4 \cdot 8 \end{bmatrix} = \begin{bmatrix} 5 & 12 \\ 21 & 32 \end{bmatrix}$$\


**2. Outer Product:**

* The outer product of two tensors of ranks m and n produces a tensor of rank m + n. This is often used to create higher-dimensional tensors by combining lower-dimensional ones.
* Example: For a rank-1 tensor $$a = [1, 2, 3]$$ and another rank-1 tensor $$b = [4, 5]$$,
* Here, the result is a rank-2 tensor (matrix) with shape 3 x 2.\
  \
  $$a \otimes b = \begin{bmatrix} 1 \cdot 4 & 1 \cdot 5 \\ 2 \cdot 4 & 2 \cdot 5 \\ 3 \cdot 4 & 3 \cdot 5 \end{bmatrix} = \begin{bmatrix} 4 & 5 \\ 8 & 10 \\ 12 & 15 \end{bmatrix}$$\


**3. Tensor Contraction (Generalized Inner Product):**

* Tensor contraction is similar to matrix multiplication but extends to tensors of higher ranks. It involves summing over specific indices in a pair of tensors, effectively reducing the total rank.
* For example, contracting a rank-3 tensor A of shape 2 x 3 x 4 with a rank-2 tensor B of shape 4 x 5 over the last index of A and the first index of B results in a rank-3 tensor of shape 2 x 3 x 5.

**4. Matrix Multiplication:**

* Matrix multiplication is a specific type of rank multiplication between two rank-2 tensors (matrices). In this operation, the number of columns in the first matrix must equal the number of rows in the second. The result is a new rank-2 tensor (matrix).
* Example: For matrices C and D where C is 2 x 3 and D is 3 x 2,\
  \
  $$C \cdot D = \text{resulting } 2 \times 2 \text{ matrix}$$

**Key Points**

* Rank Addition and Rank Multiplication often lead to different dimensional results. Rank addition typically preserves the rank, while rank multiplication can increase or decrease it.
* These operations are integral in fields like deep learning, where tensors of varying ranks are manipulated in complex models like neural networks.

### 5. Properties Based on Rank

#### 5.1 Component Count

In 3D space:

```mathematica
Rank 0: 1 component
Rank 1: 3 components
Rank 2: 9 components
Rank 3: 27 components
Rank 4: 81 components
```

#### 5.2 Symmetry Properties

```mathematica
Symmetric Rank 2: Aᵢⱼ = Aⱼᵢ
Antisymmetric Rank 2: Aᵢⱼ = -Aⱼᵢ
Cyclic Rank 3: Tᵢⱼₖ = Tⱼₖᵢ = Tₖᵢⱼ
```




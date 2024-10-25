# Definition and Properties ?..

### Visual Representation

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

### Formal Definitions

#### 1. Multilinear Map Definition

A tensor is formally defined as a multilinear map from a product of vector spaces to a field F (typically real numbers ℝ):

T: V₁\* × V₂\* × ... × Vₙ\* × W₁ × W₂ × ... × Wₘ → F

where:

* V₁\*, V₂\*, etc. are dual vector spaces
* W₁, W₂, etc. are vector spaces
* The tensor is contravariant in the W arguments
* The tensor is covariant in the V\* arguments

#### 2. Component-Based Definition

A tensor of rank (m,n) has components that transform according to:

T'ᵢ₁...ᵢₘⱼ₁...ⱼₙ = ∑ₖ₁...ₖₘₗ₁...ₗₙ (∂x'ᵢ₁/∂xₖ₁)...(∂x'ᵢₘ/∂xₖₘ)(∂xₗ₁/∂x'ⱼ₁)...(∂xₗₙ/∂x'ⱼₙ) Tₖ₁...ₖₘₗ₁...ₗₙ

### Key Properties with Examples

#### 1. Multilinearity

Definition: Linear in each argument when others are held constant.

Example:

```python
# For a 2nd order tensor T operating on vectors u and v
T(au + bv, w) = aT(u,w) + bT(v,w)
T(u, aw + bz) = aT(u,w) + bT(u,z)

# Concrete numerical example
T = [[1, 2],
     [3, 4]]
u = [1, 0]
v = [0, 1]
w = [1, 1]

# Demonstrating multilinearity:
T(2u + 3v, w) = 2T(u,w) + 3T(v,w)
```

#### 2. Transformation Rules

**Coordinate Transformation Example:**

Consider a 2nd order tensor T under rotation matrix R:

```python
# Original tensor
T = [[1, 0],
     [0, 1]]

# Rotation matrix (45 degrees)
R = [[cos(π/4), -sin(π/4)],
     [sin(π/4),  cos(π/4)]]

# Transformed tensor
T' = RTRᵀ
```

#### 3. Rank and Shape Properties

#### Shape

The shape of a tensor describes its dimensions. For example:

* Vector: (n,)
* Matrix: (m, n)
* 3D Tensor: (l, m, n)
* Batch of Images: (batch\_size, height, width, channels)

A tensor's rank determines its transformation behavior:

```python
# Examples of different ranks:

# Rank 0 (Scalar): Invariant under transformation
s = 5

# Rank 1 (Vector): Transforms once
v = [1, 2, 3]

# Rank 2 (Matrix): Transforms twice
M = [[1, 2],
     [3, 4]]

# Rank 3: Transforms three times
T = [[[1, 2], [3, 4]],
     [[5, 6], [7, 8]]]
```

#### 4. Tensor Operations

**1. Tensor Product (⊗)**

```python
# Vector tensor product example
u = [1, 2]
v = [3, 4]

u ⊗ v = [[1*3, 1*4],
         [2*3, 2*4]]
     = [[3, 4],
        [6, 8]]
```

**2. Contraction**

```python
# Contracting a rank-2 tensor (trace)
T = [[1, 2],
     [3, 4]]
Trace = T₁₁ + T₂₂ = 1 + 4 = 5
```

#### 5. Special Tensor Types

**1. Symmetric Tensor**

A tensor that remains unchanged under index permutation:

```python
# Symmetric 2nd order tensor
S = [[1, 2],
     [2, 1]]

# Property: Sᵢⱼ = Sⱼᵢ
```

**2. Antisymmetric Tensor**

Changes sign under odd permutations:

```python
# Antisymmetric 2nd order tensor
A = [[ 0,  1],
     [-1,  0]]

# Property: Aᵢⱼ = -Aⱼᵢ
```

### Practical Applications

#### 1. Stress Tensor in Physics

```python
# 3D stress tensor
σ = [[σxx, σxy, σxz],
     [σyx, σyy, σyz],
     [σzx, σzy, σzz]]

# Symmetric property: σᵢⱼ = σⱼᵢ
```

#### 2. Moment of Inertia Tensor

```python
# Moment of inertia tensor for a rigid body
I = [[Ixx, -Ixy, -Ixz],
     [-Ixy, Iyy, -Iyz],
     [-Ixz, -Iyz, Izz]]
```

#### 3. Machine Learning Example

```python
# 4D tensor for batch of images
# Shape: [batch_size, height, width, channels]
images_tensor = np.zeros((32, 256, 256, 3))

# Convolutional kernel (4D tensor)
# Shape: [kernel_height, kernel_width, in_channels, out_channels]
kernel = np.zeros((3, 3, 3, 64))
```

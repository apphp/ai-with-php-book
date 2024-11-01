# Tensor Rank

### 1. Tensor Rank (Order)

#### **Formal Definition**:&#x20;

The rank (or order) of a tensor is the number of independent directional components required to specify the tensor completely. Alternatively, it's the number of indices needed in the component representation.

* **Mathematical Form**: T ∈ ℝ
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
* **Mathematical Form**: T ∈ ℝ
* **Examples**:

```mathematica
Temperature: T = 298K
Mass: m = 5kg
Pressure: P = 101.3 kPa
Energy: E = 10J
```

#### Rank 1 (Vectors)

* **Definition**: One directional component
* **Mathematical Form**: v ∈ ℝⁿ
* **Examples**:

```mathematica
Position vector: r = [x, y, z]
Velocity: v = [vx, vy, vz]
Force: F = [Fx, Fy, Fz]
Electric field: E = [Ex, Ey, Ez]
```

#### Rank 2 (Matrices)

* **Definition**: Two directional components
* **Mathematical Form**: M ∈ ℝⁿˣᵐ
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
* **Mathematical Form**: T ∈ ℝⁿˣᵐˣᵏ
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

```mathematica
Same rank tensors can be added:

Vectors (Rank 1):
[a1] + [b1] = [a1 + b1]
[a2]   [b2]   [a2 + b2]
[a3]   [b3]   [a3 + b3]

Matrices (Rank 2):
[A11 A12] + [B11 B12] = [A11+B11 A12+B12]
[A21 A22]   [B21 B22]   [A21+B21 A22+B22]
```

#### 3.2 Rank Multiplication

```mathematica
1. Vector × Vector → Scalar (Rank reduction):
a·b = Σᵢ aᵢbᵢ

2. Vector × Vector → Matrix (Rank increase):
(a⊗b)ᵢⱼ = aᵢbⱼ

3. Matrix × Vector → Vector:
(Av)ᵢ = Σⱼ Aᵢⱼvⱼ
```

### 4. Rank Change Operations

#### 4.1 Tensor Product (Rank Increase)

```mathematica
v ⊗ w = vᵢwⱼ (Rank 1 + Rank 1 → Rank 2)

Example:
[1] ⊗ [3 4] = [3 4]
[2]          [6 8]
```

#### 4.2 Contraction (Rank Reduction)

```mathematica
Matrix trace: Tr(A) = Σᵢ Aᵢᵢ (Rank 2 → Rank 0)
Double contraction: A:B = Σᵢⱼ AᵢⱼBᵢⱼ (Rank 2 + Rank 2 → Rank 0)
```

### 5. Physical Applications and Examples

#### 5.1 Electromagnetic Theory

```mathematica
Rank 1: Electric field E = [Ex, Ey, Ez]
Rank 2: Maxwell stress tensor:
T = [Txx Txy Txz]
    [Tyx Tyy Tyz]
    [Tzx Tzy Tzz]
```

#### 5.2 Continuum Mechanics

```mathematica
Rank 2: Strain tensor εᵢⱼ
Rank 4: Elasticity tensor Cᵢⱼₖₗ
```

#### 5.3 General Relativity

```mathematica
Rank 2: Metric tensor gμν
Rank 4: Riemann curvature tensor Rᵢⱼₖₗ
```

### 6. Properties Based on Rank

#### 6.1 Component Count

In 3D space:

```mathematica
Rank 0: 1 component
Rank 1: 3 components
Rank 2: 9 components
Rank 3: 27 components
Rank 4: 81 components
```

#### 6.2 Symmetry Properties

```mathematica
Symmetric Rank 2: Aᵢⱼ = Aⱼᵢ
Antisymmetric Rank 2: Aᵢⱼ = -Aⱼᵢ
Cyclic Rank 3: Tᵢⱼₖ = Tⱼₖᵢ = Tₖᵢⱼ
```




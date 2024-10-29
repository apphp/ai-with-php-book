# Tensor Operations

#### Tensor Operations

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

####

#### Operations

**1. Element-wise Operations**

* Addition: C\[i,j,k] = A\[i,j,k] + B\[i,j,k]
* Multiplication: C\[i,j,k] = A\[i,j,k] \* B\[i,j,k]
* Activation Functions: Applied element-wise

**2. Reduction Operations**

* Sum across dimensions
* Mean across dimensions
* Maximum/minimum values

**3. Linear Transformations**

* Matrix multiplication
* Tensor contraction
* Convolution operations

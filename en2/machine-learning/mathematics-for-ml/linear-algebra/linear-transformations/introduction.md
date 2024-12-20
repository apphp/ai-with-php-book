# Introduction

### **1. Understanding Linear Mappings Between Vector Spaces**

#### **Definition**

A **linear transformation** is a mapping between two vector spaces that preserves vector addition and scalar multiplication. In simple terms, linear transformations ensure that the structure of a vector space is maintained during the mapping.

Mathematically, a function $$T:V→W$$ between two vector spaces $$V$$ and $$W$$ (over the same field, such as real numbers $$\mathbb{R}$$) that satisfies two main properties::&#x20;

1. **Additivity** (preserves vector addition):\
   \
   $$T(\mathbf{u} + \mathbf{v}) = T(\mathbf{u}) + T(\mathbf{v}), \quad \forall \mathbf{u}, \mathbf{v} \in V$$.\

2. **Homogeneity** (preserves scalar multiplication):\
   \
   $$T(c \mathbf{u}) = c T(\mathbf{u}), \quad \forall c \in \mathbb{R}, \mathbf{u} \in V.$$

These two properties ensure that a linear transformation maintains the "linear structure" of a vector space, such as straight lines, scalar multiples, and sums.

#### **Matrix Representation of Linear Transformations**

Any linear transformation $$T: \mathbb{R}^n \to \mathbb{R}^m$$ can be represented as a matrix $$A \in \mathbb{R}^{m \times n}$$:&#x20;

$$T(\mathbf{x}) = A\mathbf{x},$$

where $$\mathbf{x} \in \mathbb{R}^n$$ n is the input vector, and $$𝐴$$ is the transformation matrix.

***

#### **Example 1:**

If $$T: \mathbb{R}^2 \to \mathbb{R}^2$$ is defined as $$T\left( \begin{bmatrix} x \\ y \end{bmatrix} \right) = \begin{bmatrix} 2x \\ 3y \end{bmatrix}$$

&#x20;it is a linear transformation because it satisfies both vector addition and scalar multiplication.

#### **Example 2:**&#x20;

(Simple Scaling Transformation)

If  $$T: \mathbb{R}^2 \to \mathbb{R}^2$$  scales a vector $$\mathbf{x} = \begin{bmatrix} x \\ y \end{bmatrix}$$, then:  $$T\left( \begin{bmatrix} x \\ y \end{bmatrix} \right) = \begin{bmatrix} 2x \\ 3y \end{bmatrix}$$

For  $$\mathbf{x} = \begin{bmatrix} 1 \\ 2 \end{bmatrix}$$, the output is:  $$T\left( \begin{bmatrix} 1 \\ 2 \end{bmatrix} \right) = \begin{bmatrix} 2 \cdot 1 \\ 3 \cdot 2 \end{bmatrix} = \begin{bmatrix} 2 \\ 6 \end{bmatrix}$$

**Visualization of a Scaling Transformation**

Scaling transforms a square grid, stretching it vertically and horizontally:

<table><thead><tr><th width="383" align="center">Original Grid</th><th align="center">Scaled Grid (2x, 3y)</th></tr></thead><tbody><tr><td align="center"><img src="../../../../.gitbook/assets/image (1).png" alt="" data-size="original"></td><td align="center"><img src="../../../../.gitbook/assets/image (92).png" alt="" data-size="original"></td></tr></tbody></table>

<details>

<summary>Step by step explanation</summary>

1. Original Grid:
   * A standard coordinate system with equal spacing
   * The point (1,2) marked in red
   * Grid lines for reference

2) Scaled Grid:
   * The same coordinate system after applying the transformation
   * The transformed point (2,6) marked in blue
   * Grid lines showing the scaling effect (2x horizontal, 3y vertical)
   * Reference axes remaining in original position

You can clearly see how the transformation stretches the grid, with:

* Horizontal spacing doubled (2x scaling in x-direction)
* Vertical spacing tripled (3y scaling in y-direction)

The example point moves from (1,2) to (2,6), demonstrating how the transformation affects individual points in the space.

</details>

\
**Example 3:**&#x20;

Let $$T: \mathbb{R}^2 \to \mathbb{R}^2$$ be defined as: $$T\left( \begin{bmatrix} x \\ y \end{bmatrix} \right) = \begin{bmatrix} 3x + 2y \\ -x + 4y \end{bmatrix}.$$

This transformation can be expressed using a matrix $$A$$: $$A = \begin{bmatrix} 3 & 2 \\ -1 & 4 \end{bmatrix}.$$

Given $$\mathbf{x} = \begin{bmatrix} 1 \\ 2 \end{bmatrix}$$, the transformation is:

$$T(\mathbf{x}) = A\mathbf{x} = \begin{bmatrix} 3 & 2 \\ -1 & 4 \end{bmatrix} \begin{bmatrix} 1 \\ 2 \end{bmatrix}$$

Perform the multiplication:

$$T(\mathbf{x}) = \begin{bmatrix} 3(1) + 2(2) \\ -1(1) + 4(2) \end{bmatrix} = \begin{bmatrix} 7 \\ 7 \end{bmatrix}.$$

#### **Visualization of the Transformation**

<table><thead><tr><th width="383" align="center">Original Vector</th><th align="center">Transformed Vector</th></tr></thead><tbody><tr><td align="center"><img src="../../../../.gitbook/assets/image (94).png" alt="" data-size="original"></td><td align="center"><img src="../../../../.gitbook/assets/image (92).png" alt="" data-size="original"></td></tr></tbody></table>



The original grid is distorted based on the transformation matrix AAA, stretching and rotating the space.





***

### **2. Mathematical Properties of Linear Transformations**

A linear transformation T:Rn→RmT: \mathbb{R}^n \to \mathbb{R}^mT:Rn→Rm has the following properties:

1.  **Zero Vector Mapping**:\
    The zero vector in VVV always maps to the zero vector in WWW:

    T(0)=0.T(\mathbf{0}) = \mathbf{0}.T(0)=0.
2.  **Preservation of Linear Combinations**:\
    For vectors u,v∈V\mathbf{u}, \mathbf{v} \in Vu,v∈V and scalars a,b∈Ra, b \in \mathbb{R}a,b∈R:

    T(au+bv)=aT(u)+bT(v).T(a\mathbf{u} + b\mathbf{v}) = aT(\mathbf{u}) + bT(\mathbf{v}).T(au+bv)=aT(u)+bT(v).
3.  **Kernel (Null Space)**:\
    The set of all vectors that map to the zero vector:

    Ker(T)={x∈V:T(x)=0}.\text{Ker}(T) = \\{ \mathbf{x} \in V : T(\mathbf{x}) = \mathbf{0} \\}.Ker(T)={x∈V:T(x)=0}.
4.  **Image (Range)**:\
    The set of all vectors in WWW that are outputs of TTT:

    Im(T)={T(x):x∈V}.\text{Im}(T) = \\{ T(\mathbf{x}) : \mathbf{x} \in V \\}.Im(T)={T(x):x∈V}.



### **3. Geometric Interpretation**

* **Scaling** stretches or compresses vectors.
* **Rotation** changes the direction of vectors.
* **Reflection** mirrors vectors across an axis.

#### **Visualization**

Below are visualizations of common transformations:

| **Scaling Transformation** | **Rotation Transformation** |
| -------------------------- | --------------------------- |
|                            |                             |

| **Reflection Transformation** |
| ----------------------------- |
|                               |





### **4. Application of Matrices in Transforming Data**

Matrices are powerful tools to represent linear transformations. A linear transformation T:Rn→RmT: \mathbb{R}^n \to \mathbb{R}^m can always be expressed as matrix multiplication. Given a vector x∈Rn\mathbf{x} \in \mathbb{R}^n and a matrix A∈Rm×nA \in \mathbb{R}^{m \times n}, the transformation is:

T(x)=Ax.T(\mathbf{x}) = A\mathbf{x}.

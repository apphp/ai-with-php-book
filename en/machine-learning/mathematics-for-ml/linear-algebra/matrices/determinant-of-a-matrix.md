# Determinant of a Matrix

### Definition of Determinant

A determinant is a scalar value that can be computed from the elements of a square matrix. It provides important information about the matrix's properties.

#### 2x2 Matrices

For a 2x2 matrix

$$A = \begin{bmatrix} a & b \\ c & d \end{bmatrix}$$, the determinant is defined as: $$det(A) = |A| = ad - bc$$

**Example**:

Using formula:

$$A = \begin{bmatrix} 3 & 2 \\ 1 & 4 \end{bmatrix}$$, then $$\det(A) = (3 \cdot 4) - (2 \cdot 1) = 12 - 2 = 10$$

<details>

<summary>Step-by-step explanation</summary>

* Identify the elements: $$a = 3, b = 2, c = 1, d = 4$$
* Apply the formula: $$det(A) = ad - bc$$
* Substitute the values: $$\det(A) = (3 \cdot 4) - (2 \cdot 1)$$
* Multiply: $$det(A) = 12 - 2$$
* Subtract: $$det(A) = 10$$

</details>

#### 3x3 Matrices

For a 3x3 matrix $$A = \begin{bmatrix} a & b & c \\ d & e & f \\ g & h & i \end{bmatrix}$$, the determinant can be calculated using Sarrus' rule or cofactor\
expansion.

**Sarrus' rule**

{% hint style="info" %}
Sarrus' rule is a useful tool for quickly calculating 3x3 determinants, especially when working by hand. However, for larger matrices or when using computer algorithms, other methods like cofactor expansion or LU decomposition are typically preferred.
{% endhint %}

Given: $$A = \begin{bmatrix} a & b & c \\ d & e & f \\ g & h & i \end{bmatrix}$$

The determinant of A , denoted $$\det(A)$$, is calculated as:

$$\det(A) = (a \cdot e \cdot i + b \cdot f \cdot g + c \cdot d \cdot h) - (c \cdot e \cdot g + b \cdot d \cdot i + a \cdot f \cdot h)$$

<details>

<summary>Step-by-step explanation</summary>

* Start with your 3x3 matrix:\
  \
  $$\begin{bmatrix} a & b & c \\ d & e & f \\ g & h & i \end{bmatrix}$$

- Extend the matrix by copying the first two columns to the right:\
  \
  $$\begin{bmatrix} a & b & c \mid a & b \\ d & e & f \mid d & e \\ g & h & i \mid g & h \end{bmatrix}$$

*   Calculate the products along the diagonals: a. Positive diagonals (left to right):

    * $$aei$$ (main diagonal)
    * $$bfg$$ (starts from the second column)
    * $$cdh$$ (starts from the third column)

    b. Negative diagonals (right to left):

    * $$ceg$$ (starts from the third column)
    * $$bdi$$ (starts from the second column)
    * $$afh$$ (starts from the first column of the extension)
* Sum the positive products and subtract the sum of the negative products:\
  $$det(A) = (aei + bfg + cdh) - (ceg + bdi + afh)$$

</details>

**Example**: Let $$A = \begin{bmatrix} 2 & -1 & 3 \\ 4 & 5 & -2 \\ 1 & -3 & 2 \end{bmatrix}$$

Using Sarrus' rule:

Given: $$A = \begin{bmatrix} 2 & -1 & 3 \\ 4 & 5 & -2 \\ 1 & -3 & 2 \end{bmatrix}$$

For the diagonal method, we extend matrix A by repeating the first two columns:

$$\begin{bmatrix} 2 & -1 & 3 & | & 2 & -1 \\ 4 & 5 & -2 & | & 4 & 5 \\ 1 & -3 & 2 & | & 1 & -3 \end{bmatrix}$$

Positive diagonals: 20 2 -36

Negative diagonals: 15 -8 12

$$det(A) = (20 + 2 - 36) - (15 + (-8) + 12) = -14 - 19 = -33$$

<details>

<summary>Step-by-step explanation</summary>

Step 1: Write out the matrix $$A = \begin{bmatrix} 2 & -1 & 3 \\ 4 & 5 & -2 \\ 1 & -3 & 2 \end{bmatrix}$$

Step 2: Extend the matrix $$A = \begin{bmatrix} 2 & -1 & 3 & | & 2 & -1 \\ 4 & 5 & -2 & | & 4 & 5 \\ 1 & -3 & 2 & | & 1 & -3 \end{bmatrix}$$

Step 3: Calculate the products a. Positive diagonals:

* $$2 * 5 * 2 = 20$$
* $$(-1) * (-2) * 1 = 2$$
* $$3 * 4 * (-3) = -36$$

b. Negative diagonals:

* $$3 * 5 * 1 = 15$$
* $$(-1) * 4 * 2 = -8$$
* $$2 * (-2) * (-3) = 12$$

Step 4: Sum and subtract\\

$$det(A) = (20 + 2 + (-36)) - (15 + (-8) + 12) = -14 - 19 = -33$$

Therefore, the determinant of $$A$$ is $$-33$$.

</details>

**Cofactor expansion**

{% hint style="info" %}
Cofactor expansion, also known as Laplace expansion, is a general method for calculating determinants of matrices of any size. For 3x3 matrices, it provides an alternative to Sarrus' rule and helps build understanding for larger matrices.
{% endhint %}

#### Key Concepts

1. Minor: The minor of an element $$a_{ij}$$ is the determinant of the 2x2 matrix formed by deleting the $$i$$-th row and $$j$$-th column of the original matrix.
2. Cofactor: The cofactor $$C_{ij}$$ of an element $$a_{ij}$$ is defined as $$C_{ij} = (-1)^{i+j} * M_{ij}$$.
3. Expansion: The determinant is the sum of the products of the elements of any row (or column) with their cofactors.

#### The Method

For a 3x3 matrix $$A = \begin{bmatrix} a & b & c \\ d & e & f \\ g & h & i \end{bmatrix}$$, the cofactor expansion along the first row is:

$$det(A) = a * C_{11} + b * C_{12} + c * C_{13}$$

where $$C_{11}$$, $$C_{12}$$, and $$C_{13}$$ are the cofactors of $$a$$, , and $$c$$ respectively.

For example, expanding along the first row: $$det(A) = a(ei-fh) - b(di-fg) + c(dh-eg)$$

<details>

<summary>Step-by-step explanation</summary>

* Choose a row or column for expansion (typically the one with the most zeros).
* For each element in the chosen row/column:
  * a. Find its minor by calculating the determinant of the 2x2 matrix formed by deleting its row and column.
  * b. Calculate its cofactor by multiplying the minor by .
  * c. Multiply the element by its cofactor.
* Sum all these products.

</details>

**Example**: Let $$A = \begin{bmatrix} 2 & -1 & 3 \\ 4 & 5 & -2 \\ 1 & -3 & 2 \end{bmatrix}$$

Using Cofactor expansion:

$$\text{det}(A) = 2 \cdot C_{11} + (-1) \cdot C_{12} + 3 \cdot C_{13}$$\
$$= 2 \cdot (1 \cdot 4) + (-1) \cdot (-1 \cdot 10) + 3 \cdot (-12 - 5)$$\
$$= 2 \cdot 4 + (-1) \cdot (-10) + 3 \cdot (-17)$$\
$$= 2 \cdot 4 + (-1) \cdot (-10) + 3 \cdot (-17)$$

<details>

<summary>Step-by-step explanation</summary>

Let's use cofactor expansion on the first row of matrix $$A = \begin{bmatrix} 2 & -1 & 3 \\ 4 & 5 & -2 \\ 1 & -3 & 2 \end{bmatrix}$$

Step 1: Expand along the first row $$det(A) = 2 * C_{11} + (-1) * C_{12} + 3 * C_{13}$$

Step 2: Calculate each cofactor

* For $$a = 2$$ (position $$1,1$$):
  * Minor $$M_{11} = | 5 -2 | = 5(2) - (-2)(-3) = 10 - 6 = 4$$
  * $$|-3 2 |$$
  * Cofactor $$C_{11} = (-1)^{1+1} * M_{11} = 1 * 4 = 4$$
* For $$b = -1$$ (position $$1,2$$):
  * Minor $$M_{12} = | 4 -2 | = 4(2) - (-2)(1) = 8 + 2 = 10$$
  * $$| 1 2 |$$
  * Cofactor $$C_{12} = (-1)^{1+2} * M_{12} = -1 * 10 = -10$$
* For (position $$1,3$$):
  * Minor $$M_{13} = | 4 5 | = 4(-3) - (5)(1) = -12 - 5 = -17$$
  * $$| 1 -3 |$$
  * Cofactor $$C_{13} = (-1)^{1+3} * M_{13} = 1 * (-17) = -17$$

Step 3: Sum the products \\

$$det(A) = 2 * 4 + (-1) * (-10) + 3 * (-17) = 8 + 10 - 51 = -33$$

Therefore, the determinant of $$A$$ is $$-33$$.

</details>

### Properties of Determinants

1. Determinant of identity matrix: $$det(I) = 1$$
2. Effect of row/column operations:
   * Swapping two rows/columns changes the sign of the determinant
   * Multiplying a row/column by a scalar k multiplies the determinant by $$k$$
   * Adding a multiple of one row/column to another doesn't change the determinant
3. Multiplicative property:
4. Transpose property: $$det(A) = det(A^T)$$
5. Zero determinant and singular matrices: If $$det(A) = 0$$, then $$A$$ is singular (non-invertible)

**Example** demonstrating properties:

Let $$A = \begin{bmatrix} 2 & 1 \\ 3 & 4 \end{bmatrix}$$ and $$B = \begin{bmatrix} 1 & 2 \\ 0 & 3 \end{bmatrix}$$

$$det(A) = 2(4) - 1(3) = 8 - 3 = 5$$

$$det(B) = 1(3) - 2(0) = 3$$

$$det(AB) = det([2 7; 3 18]) = 2(18) - 7(3) = 36 - 21 = 15$$

Verifying multiplicative property: $$det(A) * det(B) = 5 * 3 = 15 = det(AB)$$

### Applications of Determinants

1. Solving systems of linear equations (Cramer's rule):\
   For a system $$Ax = b$$, where A is a square matrix, the solution is: $$x_i = det(A_i) / det(A)$$,\
   where $$A_i$$ is A with its $$i$$-th column replaced by $$b$$.
2. Finding inverse matrices:\
   For a 2x2 matrix $$A = \begin{bmatrix} a & b \\ c & d \end{bmatrix}$$, its inverse is: $$A^{-1} = \frac{1}{\text{det}(A)} \begin{bmatrix} d & -b \\ -c & a \end{bmatrix}$$
3. Calculating area and volume:
   1. The absolute value of the determinant of a 2x2 matrix represents the area of the parallelogram formed by its column vectors.
   2. The absolute value of the determinant of a 3x3 matrix represents the volume of the parallelepiped formed by its column vectors.

**Example** area calculation:

Let $$A = \begin{bmatrix} 3 & 1 \\ 1 & 2 \end{bmatrix}$$ represent two vectors. $$det(A) = 3(2) - 1(1) = 5$$\
The area of the parallelogram formed by these vectors is $$|5| = 5$$ square units.

<details>

<summary>Step-by-step explanation</summary>

Step 1: Understand the given vectors We have two vectors represented by the matrix

$$A$$: $$A = \begin{bmatrix} 3 & 1 \\ 1 & 2 \end{bmatrix}$$\
These vectors are: $$v1 = (3, 1)\ v2 = (1, 2)$$

Step 2: Set up the determinant calculation The formula for the area of a parallelogram formed by two vectors is the absolute value of the determinant of the matrix formed by these vectors.

$$Area = |det(A)|$$

For a 2x2 matrix $$\begin{bmatrix} a & b \\ c & d \end{bmatrix}$$, the determinant is calculated as: $$ad - bc$$

Step 3: Calculate the determinant $$det(A) = (3 × 2) - (1 × 1) = 6 - 1 = 5$$

Step 4: Take the absolute value Since the area is always positive, we take the absolute value of the determinant: $$Area = |det(A)| = |5| = 5$$

Step 5: Interpret the result\
The area of the parallelogram formed by vectors $$v1$$ and $$v2$$ is $$5$$ square units.

Additional explanation:

* This method works because the determinant of a 2x2 matrix represents the signed area of the parallelogram formed by the two column vectors.
* The absolute value is used because area is always positive, while determinants can be positive or negative.
* This calculation also represents the magnitude of the cross product of the two vectors in 3D space, with the third component being zero.

</details>

Understanding these matrix operations with concrete examples is essential for implementing efficient ML algorithms and grasping the underlying mathematical principles of many machine learning techniques.

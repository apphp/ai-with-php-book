# Matrix Operations

### Compatibility Conditions and Properties

Understanding the compatibility conditions and properties of matrix operations is crucial in machine learning, especially when dealing with neural networks and other complex models.

#### Compatibility Conditions

Matrix operations have specific requirements for the dimensions of the matrices involved. This is particularly important for matrix multiplication.

### Matrix Operations

Matrix operations are fundamental to many machine learning algorithms and techniques. Understanding these operations is crucial for implementing and optimizing ML models efficiently.

#### Properties of Matrix Operations

Understanding these properties helps in optimizing computations and designing efficient algorithms.

**1. Non-commutativity of Matrix Multiplication**

Unlike scalar multiplication, matrix multiplication is not commutative. In general, $$AB ≠ BA$$.

**Example**: $$A = \begin{bmatrix} 1 & 2 \end{bmatrix}$$, $$B = \begin{bmatrix} 5 & 6 \\ 3 & 4 \\ 7 & 8 \end{bmatrix}$$

$$AB = \begin{bmatrix} 19 & 22 \end{bmatrix} \neq BA = \begin{bmatrix} 23 & 34 \\ 43 & 50 \\ 31 & 46 \end{bmatrix}$$\


ML Application: The order of operations matters in neural network computations. For instance, applying activation functions before or after matrix multiplication can lead to different results.

**2. Associativity of Matrix Multiplication**

$$(AB)C = A(BC)$$ for matrices with compatible dimensions.

**Example**: $$A (2×2) * (B (2×2) * C (2×1)) = (A (2×2) * B (2×2)) * C (2×1)$$

ML Application: This property allows for optimizing computations in deep neural networks by grouping operations efficiently.

**3. Distributivity of Matrix Multiplication over Addition**

$$A(B + C) = AB + AC$$ and $$(A + B)C = AC + BC$$ for matrices with compatible dimensions.

**Example**: $$A * (B + C) = (A * B) + (A * C)$$

ML Application: This property is useful in backpropagation when computing gradients with respect to multiple parameters.

### **Addition and Subtraction**

Matrix addition and subtraction are performed element-wise between matrices of the same dimensions.

**Examples** (2 x 2 matrices):

Given: $$A = \begin{bmatrix} 1 & 2 \\ 3 & 4 \end{bmatrix}, \quad B = \begin{bmatrix} 5 & 6 \\ 7 & 8 \end{bmatrix}$$  then  $$A + B = \begin{bmatrix} 1 + 5 & 2 + 6 \\ 3 + 7 & 4 + 8 \end{bmatrix} = \begin{bmatrix} 6 & 8 \\ 10 & 12 \end{bmatrix}$$

<details>

<summary>Step by step explanation</summary>

Step 1: Add corresponding elements

* $$(1,1): 1 + 5 = 6$$
* $$(1,2): 2 + 6 = 8$$
* $$(2,1): 3 + 7 = 10$$
* $$(2,2): 4 + 8 = 12$$

Step 2: Write the result $$A + B = \begin{bmatrix} 6 & 8 \\ 10 & 12 \end{bmatrix}$$

</details>

Given: $$A = \begin{bmatrix} 1 & 2 \\ 7 & 8 \end{bmatrix}, \quad B = \begin{bmatrix} 5 & 6 \\ 3 & 4 \end{bmatrix}$$ then $$A - B = \begin{bmatrix} 1 - 5 & 2 - 6 \\ 7 - 3 & 8 - 4 \end{bmatrix} = \begin{bmatrix} -4 & -2 \\ 4 & 4 \end{bmatrix}$$

<details>

<summary>Step by step explanation</summary>

Step 1: Add corresponding elements

* $$(1,1): 1 - 5 = -4$$
* $$(1,2): 2 - 6 = -2$$
* $$(2,1): 7 - 3 = 4$$
* $$(2,2): 8 -4 = 4$$

Step 2: Write the result $$A - B = \begin{bmatrix} -4 & -2 \\ 4 & 4 \end{bmatrix}$$

</details>

**Example in ML**: Updating weights in neural networks. In gradient descent, we update parameters by subtracting the gradient multiplied by the learning rate:

$$W = \begin{bmatrix} 0.1 & 0.2 \\ 0.3 & 0.4 \end{bmatrix}, \quad \text{gradient} = \begin{bmatrix} 0.01 & 0.02 \\ 0.03 & 0.04 \end{bmatrix}, \quad \text{learning rate} = 0.1$$&#x20;

$$W_{\text{new}} = W - (\text{learning rate} \times \text{gradient})$$&#x20;

$$W_{\text{new}} = \begin{bmatrix} 0.1 & 0.2 \\ 0.3 & 0.4 \end{bmatrix} - 0.1 \cdot \begin{bmatrix} 0.01 & 0.02 \\ 0.03 & 0.04 \end{bmatrix} = \begin{bmatrix} 0.099 & 0.198 \\ 0.297 & 0.396 \end{bmatrix}$$

<details>

<summary>Step by step explanation</summary>

Step 1: Multiply gradient by learning rate

$$0.1 \cdot \begin{bmatrix} 0.01 & 0.02 \\ 0.03 & 0.04 \end{bmatrix} = \begin{bmatrix} 0.001 & 0.002 \\ 0.003 & 0.004 \end{bmatrix}$$

Step 2: Subtract from&#x20;

$$\begin{bmatrix} 0.1 & 0.2 \\ 0.3 & 0.4 \end{bmatrix} - \begin{bmatrix} 0.001 & 0.002 \\ 0.003 & 0.004 \end{bmatrix} = \begin{bmatrix} 0.099 & 0.198 \\ 0.297 & 0.396 \end{bmatrix}$$

</details>

### Scalar Multiplication

Scalar multiplication involves multiplying each element of a matrix by a scalar value.

**Example** (3x3 matrix):

Let's multiply a 3x3 matrix by a scalar:

Given:

$$k = 2, \quad A = \begin{bmatrix} 1 & 2 & 3 \\ 4 & 5 & 6 \\ 7 & 8 & 9 \end{bmatrix}$$

Then,

$$k \cdot A = 2 \cdot \begin{bmatrix} 1 & 2 & 3 \\ 4 & 5 & 6 \\ 7 & 8 & 9 \end{bmatrix} = \begin{bmatrix} 2 & 4 & 6 \\ 8 & 10 & 12 \\ 14 & 16 & 18 \end{bmatrix}$$

<details>

<summary>Step by step explanation</summary>

Step 1: Multiply each element by $$k$$

* $$(1,1): 2 * 1 = 2$$
* $$(1,2): 2 * 2 = 4$$
* $$(1,3): 2 * 3 = 6$$
* $$(2,1): 2 * 4 = 8$$
* $$(2,2): 2 * 5 = 10$$
* $$(2,3): 2 * 6 = 12$$
* $$(3,1): 2 * 7 = 14$$
* $$(3,2): 2 * 8 = 16$$
* $$(3,3): 2 * 9 = 18$$

Step 2: Write the result $$A = \begin{bmatrix} 2 & 4 & 6 \\ 8 & 10 & 12 \\ 14 & 16 & 18 \end{bmatrix}$$

</details>

### Matrix Multiplication

Matrix multiplication is a crucial operation in many ML computations, including neural network layers and linear transformations.

**Matrix Multiplication Compatibility**

For two matrices A and B to be multiplied:

* The number of columns in matrix  must equal the number of rows in matrix $$B$$.
* If A is an $$m × n$$ matrix and B is a $$p × q$$ matrix, then n must equal p.
* The resulting matrix will have dimensions $$m × q$$.

**Example** (2x2 matrices):&#x20;

Given:&#x20;

$$A = \begin{bmatrix} 1 & 2 \\ 3 & 4 \end{bmatrix}, \quad B = \begin{bmatrix} 5 & 6 \\ 7 & 8 \end{bmatrix}$$

Then,&#x20;

$$A \cdot B = \begin{bmatrix} 1 & 2 \\ 3 & 4 \end{bmatrix} \cdot \begin{bmatrix} 5 & 6 \\ 7 & 8 \end{bmatrix} = \begin{bmatrix} 19 & 22 \\ 43 & 50 \end{bmatrix}$$

<details>

<summary>Step by step explanation</summary>

Step 1: Multiply row 1 of A with columns of B

* $$(1,1): (15) + (27) = 5 + 14 = 19$$
* $$(1,2): (16) + (28) = 6 + 16 = 22$$

Step 2: Multiply row 2 of A with columns of B

* $$(2,1): (35) + (47) = 15 + 28 = 43$$
* $$(2,2): (36) + (48) = 18 + 32 = 50$$

Step 3: Write the result $$AB = \begin{bmatrix} 19 & 22 \\ 43 & 50 \end{bmatrix}$$

</details>

**Example in ML.** Forward pass in a neural network layer:

Given:

$$W = \begin{bmatrix} 0.1 & 0.2 \\ 0.3 & 0.4 \end{bmatrix}, \quad X = \begin{bmatrix} 2 \\ 3 \end{bmatrix}, \quad b = \begin{bmatrix} 0.1 \\ 0.2 \end{bmatrix}$$

Then,

$$Z = W \cdot X + b = \begin{bmatrix} (0.1 \cdot 2 + 0.2 \cdot 3) \\ (0.3 \cdot 2 + 0.4 \cdot 3) \end{bmatrix} + \begin{bmatrix} 0.1 \\ 0.2 \end{bmatrix} = \begin{bmatrix} 0.8 \\ 2.0 \end{bmatrix} + \begin{bmatrix} 0.1 \\ 0.2 \end{bmatrix} = \begin{bmatrix} 0.9 \\ 2.2 \end{bmatrix}$$

<details>

<summary>Step by step explanation</summary>

Step 1: Multiply $$W$$ and $$X$$&#x20;

$$Z = W \cdot X = \begin{bmatrix} 0.1 & 0.2 \\ 0.3 & 0.4 \end{bmatrix} \cdot \begin{bmatrix} 2 \\ 3 \end{bmatrix} = \begin{bmatrix} (0.1 \cdot 2 + 0.2 \cdot 3) \\ (0.3 \cdot 2 + 0.4 \cdot 3) \end{bmatrix}$$$$Z = W \cdot X = \begin{bmatrix} 0.1 & 0.2 \\ 0.3 & 0.4 \end{bmatrix} \cdot \begin{bmatrix} 2 \\ 3 \end{bmatrix} = \begin{bmatrix} (0.1 \cdot 2 + 0.2 \cdot 3) \\ (0.3 \cdot 2 + 0.4 \cdot 3) \end{bmatrix}$$

Step 2: Add bias b

$$Z = \begin{bmatrix} 0.8 \\ 1.8 \end{bmatrix} + \begin{bmatrix} 0.1 \\ 0.2 \end{bmatrix} = \begin{bmatrix} 0.8 + 0.1 \\ 1.8 + 0.2 \end{bmatrix} = \begin{bmatrix} 0.9 \\ 2.0 \end{bmatrix}$$$$[0.8] + [0.1] = [0.9] [1.8] [0.2] [2.0]$$

</details>

### Transposition

Transposition is the operation of flipping a matrix over its diagonal, switching its rows with its columns.

#### **Transpose Properties**

$$(A^T)^T = A (AB)^T = B^T A^T (A + B)^T = A^T + B^T$$

**Example:**

Given:\
\
&#x20;$$A = \begin{bmatrix} 1 & 2 \\ 3 & 4 \end{bmatrix}$$

Taking the transpose of $$A$$:&#x20;

$$A^T = \begin{bmatrix} 1 & 3 \\ 2 & 4 \end{bmatrix}$$

Then, taking the transpose again:

$$(A^T)^T = \begin{bmatrix} 1 & 2 \\ 3 & 4 \end{bmatrix} = A$$

ML Application: These properties are often used in deriving gradient descent algorithms and in simplifying complex matrix expressions in various ML models.

Understanding these compatibility conditions and properties is essential for:

1. Correctly implementing machine learning algorithms
2. Optimizing computations for better performance
3. Debugging issues related to matrix dimensions in neural networks
4. Deriving new algorithms or simplifying existing ones

In practice, many machine learning libraries handle these compatibility checks automatically, but understanding the underlying principles helps in designing and troubleshooting models effectively.

**Example** (3x3 matrix)**:**&#x20;

Given:

$$A = \begin{bmatrix} 1 & 2 & 3 \\ 4 & 5 & 6 \\ 7 & 8 & 9 \end{bmatrix}$$

Then, the transpose of $$A$$ , denoted $$A^T$$, is:

$$A^T = \begin{bmatrix} 1 & 4 & 7 \\ 2 & 5 & 8 \\ 3 & 6 & 9 \end{bmatrix}$$

<details>

<summary>Step by step explanation</summary>

Step 1: Swap rows and columns

* $$New (1,1) = Old (1,1): 1$$
* $$New (1,2) = Old (2,1): 4$$
* $$New (1,3) = Old (3,1): 7$$
* $$New (2,1) = Old (1,2): 2$$
* $$New (2,2) = Old (2,2): 5$$
* $$New (2,3) = Old (3,2): 8$$
* $$New (3,1) = Old (1,3): 3$$
* $$New (3,2) = Old (2,3): 6$$
* $$New (3,3) = Old (3,3): 9$$

Step 2: Write the result

$$A^T = \begin{bmatrix} 1 & 4 & 7 \\ 2 & 5 & 8 \\ 3 & 6 & 9 \end{bmatrix}$$

</details>

**Example in ML.** Computing the gradient in linear regression:

( $$gradient = X^T * (y_{pred} - y) / n_{samples}$$ )

Given:

$$X = \begin{bmatrix} 1 & 2 \\ 3 & 4 \\ 5 & 6 \end{bmatrix}, \quad y = \begin{bmatrix} 5 \\ 11 \\ 17 \end{bmatrix}, \quad w = \begin{bmatrix} 0.5 \\ 1.5 \end{bmatrix}$$

Transpose of $$X$$:&#x20;

$$X^T = \begin{bmatrix} 1 & 3 & 5 \\ 2 & 4 & 6 \end{bmatrix}$$

Predicted values $$y_{\text{pred}}$$:&#x20;

$$y_{\text{pred}} = X \cdot w = \begin{bmatrix} 1 \cdot 0.5 + 2 \cdot 1.5 \\ 3 \cdot 0.5 + 4 \cdot 1.5 \\ 5 \cdot 0.5 + 6 \cdot 1.5 \end{bmatrix} = \begin{bmatrix} 3.5 \\ 7.5 \\ 11.5 \end{bmatrix}$$

Error calculation:&#x20;

$$\text{error} = y_{\text{pred}} - y = \begin{bmatrix} 3.5 - 5 \\ 7.5 - 11 \\ 11.5 - 17 \end{bmatrix} = \begin{bmatrix} -1.5 \\ -3.5 \\ -5.5 \end{bmatrix}$$

Gradient calculation:&#x20;

$$\text{gradient} = X^T \cdot \text{error} = \begin{bmatrix} 1 \cdot (-1.5) + 3 \cdot (-3.5) + 5 \cdot (-5.5) \\ 2 \cdot (-1.5) + 4 \cdot (-3.5) + 6 \cdot (-5.5) \end{bmatrix} = \begin{bmatrix} -40.5 \\ -58.5 \end{bmatrix}$$\


<details>

<summary>Step by step explanation</summary>

Step 1: Calculate error:

$$\text{error} = y_{\text{pred}} - y = \begin{bmatrix} 3.5 - 5 \\ 7.5 - 11 \\ 11.5 - 17 \end{bmatrix} = \begin{bmatrix} -1.5 \\ -3.5 \\ -5.5 \end{bmatrix}$$

Step 2: Transpose $$X$$:&#x20;

$$X^T = \begin{bmatrix} 1 & 3 & 5 \\ 2 & 4 & 6 \end{bmatrix}$$

Step 3: Multiply $$X^T$$ by error:&#x20;

$$\text{gradient} = X^T \cdot \text{error} = \begin{bmatrix} 1 \cdot (-1.5) + 3 \cdot (-3.5) + 5 \cdot (-5.5) \\ 2 \cdot (-1.5) + 4 \cdot (-3.5) + 6 \cdot (-5.5) \end{bmatrix} = \begin{bmatrix} -40.5 \\ -58.5 \end{bmatrix}$$

Step 4: Divide by $$n_{samples}$$ (3 in this case):

$$\frac{\text{gradient}}{3} = \begin{bmatrix} \frac{-40.5}{3} \\ \frac{-58.5}{3} \end{bmatrix} = \begin{bmatrix} -13.5 \\ -19.5 \end{bmatrix}$$

This step-by-step breakdown illustrates how each matrix operation is performed and how it applies in machine learning contexts.

</details>

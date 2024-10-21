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

**Example**: $$A = [1\ 2]$$, $$B = [5\ 6] [3\ 4] [7\ 8]$$

$$AB = [19\ 22] ≠ BA = [23\ 34] [43\ 50] [31\ 46]$$

ML Application: The order of operations matters in neural network computations. For instance, applying activation functions before or after matrix multiplication can lead to different results.

**2. Associativity of Matrix Multiplication**

$$(AB)C = A(BC)$$ for matrices with compatible dimensions.

**Example**: $$A (2×2) * (B (2×2) * C (2×1)) = (A (2×2) * B (2×2)) * C (2×1)$$

ML Application: This property allows for optimizing computations in deep neural networks by grouping operations efficiently.

**3. Distributivity of Matrix Multiplication over Addition**

$$A(B + C) = AB + AC and (A + B)C = AC + BC$$ for matrices with compatible dimensions.

**Example**: $$A * (B + C) = (A * B) + (A * C)$$

ML Application: This property is useful in backpropagation when computing gradients with respect to multiple parameters.

### **Addition and Subtraction**

Matrix addition and subtraction are performed element-wise between matrices of the same dimensions.

**Examples** (2x2 matrices):

```
A = [1 2]    B = [5 6]     A + B = [6 8]
    [3 4]        [7 8]             [10 12]
```

<details>

<summary>Step by step explanation</summary>

Step 1: Add corresponding elements

* $$(1,1): 1 + 5 = 6$$
* $$(1,2): 2 + 6 = 8$$
* $$(2,1): 3 + 7 = 10$$
* $$(2,2): 4 + 8 = 12$$

Step 2: Write the result $$A + B = [6\ 8] [10\ 12]$$

</details>

```
A = [1 2]    B = [5 6]     A - B = [-4 -2]
    [7 8]        [3 4]             [4 4]
```

<details>

<summary>Step by step explanation</summary>

Step 1: Add corresponding elements

* $$(1,1): 1 - 5 = -4$$
* $$(1,2): 2 - 6 = -2$$
* $$(2,1): 7 - 3 = 4$$
* $$(2,2): 8 -4 = 4$$

Step 2: Write the result $$A - B = [-4\ -2] [4\ 4]$$

</details>

**Example in ML**: Updating weights in neural networks. In gradient descent, we update parameters by subtracting the gradient multiplied by the learning rate:

```
W = [0.1 0.2]  gradient = [0.01 0.02]  learning_rate = 0.1
    [0.3 0.4]             [0.03 0.04]

W_new = W - learning_rate * gradient

W_new = [0.1 0.2] - 0.1 * [0.01 0.02] = [0.099 0.198]
        [0.3 0.4]         [0.03 0.04]   [0.297 0.396]
```

<details>

<summary>Step by step explanation</summary>

Step 1: Multiply gradient by learning rate $$0.1 * [0.01\ 0.02] = [0.001\ 0.002] [0.03\ 0.04] [0.003\ 0.004]$$

Step 2: Subtract from $$W [0.1\ 0.2 ] - [0.001\ 0.002] = [0.099\ 0.198] [0.3\ 0.4 ] [0.003\ 0.004] [0.297\ 0.396]$$

</details>

### Scalar Multiplication

Scalar multiplication involves multiplying each element of a matrix by a scalar value.

Example (3x3 matrix):

Let's multiply a 3x3 matrix by a scalar:

```
k = 2
A = [1 2 3]    k * A = [2 4 6]
    [4 5 6]            [8 10 12]
    [7 8 9]            [14 16 18]
```

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

Step 2: Write the result $$A = [2\ 4\ 6] [8\ 10\ 12] [14\ 16\ 18]$$

</details>

### Matrix Multiplication

Matrix multiplication is a crucial operation in many ML computations, including neural network layers and linear transformations.

**Matrix Multiplication Compatibility**

For two matrices A and B to be multiplied:

* The number of columns in matrix  must equal the number of rows in matrix $$B$$.
* If A is an $$m × n$$ matrix and B is a $$p × q$$ matrix, then n must equal p.
* The resulting matrix will have dimensions $$m × q$$.

Example (2x2 matrices):

$$A = [1\ 2], B = [5\ 6]$$&#x20;

```
A = [1 2]      B = [5 6]        A * B = [19 22]
    [3 4]          [7 8]                [43 50]
```

<details>

<summary>Step by step explanation</summary>

Step 1: Multiply row 1 of A with columns of B

* $$(1,1): (15) + (27) = 5 + 14 = 19$$
* $$(1,2): (16) + (28) = 6 + 16 = 22$$

Step 2: Multiply row 2 of A with columns of B

* $$(2,1): (35) + (47) = 15 + 28 = 43$$
* $$(2,2): (36) + (48) = 18 + 32 = 50$$

Step 3: Write the result $$AB = [19\ 22] [43\ 50]$$

</details>

**Example in ML**: Forward pass in a neural network layer:

```
W = [0.1 0.2]  X = [2]  b = [0.1]
    [0.3 0.4]      [3]      [0.2]

Z = W * X + b = [(0.1*2 + 0.2*3)] + [0.1] = [0.8] + [0.1] = [0.9]
                [(0.3*2 + 0.4*3)]   [0.2]   [1.8]   [0.2]   [2.0]
```

<details>

<summary>Step by step explanation</summary>

Step 1: Multiply $$W$$ and $$X [0.1 0.2] * [2] = [(0.12 + 0.23)] [0.3 0.4] [3] [(0.32 + 0.43)] = [0.2 + 0.6] = [0.8] [0.6 + 1.2] [1.8]$$

Step 2: Add bias b $$[0.8] + [0.1] = [0.9] [1.8] [0.2] [2.0]$$

</details>

### Transposition

Transposition is the operation of flipping a matrix over its diagonal, switching its rows with its columns.

#### **Transpose Properties**

$$(A^T)^T = A (AB)^T = B^T A^T (A + B)^T = A^T + B^T$$

Example: $$If A = [1\ 2], then (A{T})^T = [1\ 2] = A [3\ 4] [3\ 4]$$

ML Application: These properties are often used in deriving gradient descent algorithms and in simplifying complex matrix expressions in various ML models.

Understanding these compatibility conditions and properties is essential for:

1. Correctly implementing machine learning algorithms
2. Optimizing computations for better performance
3. Debugging issues related to matrix dimensions in neural networks
4. Deriving new algorithms or simplifying existing ones

In practice, many machine learning libraries handle these compatibility checks automatically, but understanding the underlying principles helps in designing and troubleshooting models effectively.

**Example (3x3 matrix):**

```
A = [1 2 3] A^T = [1 4 7] 
    [4 5 6]       [2 5 8] 
    [7 8 9]       [3 6 9]
```

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

Step 2: Write the result $$A^T = [1\ 4\ 7] [2\ 5\ 8] [3\ 6\ 9]$$

</details>

**Example in ML**: Computing the gradient in linear regression:

( $$gradient = X^T * (y_pred - y) / n_samples$$ )

```
X = [1 2]  y = [5]  
    [3 4]      [11]
    [5 6]      [17]

X^T = [1 3 5]
      [2 4 6]

w = [0.5]
    [1.5]

y_pred = X * w = [1*0.5 + 2*1.5]   = [3.5]
                 [3*0.5 + 4*1.5]     [7.5]
                 [5*0.5 + 6*1.5]     [11.5]

error = y_pred - y = [3.5 - 5]    = [-1.5]
                     [7.5 - 11]     [-3.5]
                     [11.5 - 17]    [-5.5]

gradient = X^T * error = [(1*-1.5 + 3*-3.5 + 5*-5.5)]   = [-40.5]
                         [(2*-1.5 + 4*-3.5 + 6*-5.5)]     [-58.5]
```

<details>

<summary>Step by step explanation</summary>

Step 1: Calculate error: $$(y_{pred} - y) [3.5] - [5] = [-1.5] [7.5] [11] [-3.5] [11.5] [17] [-5.5]$$

Step 2: Transpose $$X$$: $$X^T = [1\ 3\ 5] [2\ 4\ 6]$$

Step 3: Multiply $$X^T$$ by error: $$[1\ 3\ 5] * [-1.5] = [(1*-1.5 + 3*-3.5 + 5*-5.5)] [2\ 4\ 6] [-3.5] [(2*-1.5 + 4*-3.5 + 6*-5.5)] [-5.5] = [-40.5] [-58.5]$$

Step 4: Divide by $$n_{samples}$$ (3 in this case): $$[-40.5 / 3] = [-13.5] [-58.5 / 3] [-19.5]$$

This step-by-step breakdown illustrates how each matrix operation is performed and how it applies in machine learning contexts.

</details>

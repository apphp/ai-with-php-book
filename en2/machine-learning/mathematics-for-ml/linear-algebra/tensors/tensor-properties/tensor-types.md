# Tensor Types

In mathematics and machine learning, tensors are fundamental structures that hold data in multi-dimensional arrays. Each element within a tensor follows a specific **tensor type** — a data format that determines the range, memory allocation, precision, and permissible operations on that data. Here’s a guide to the main tensor types, with definitions and examples.

### What is a Tensor Type?

A **tensor type** defines the numeric format for each element in a tensor. It specifies:

* **Range of values** that the tensor can represent
* **Memory allocation** requirements
* **Precision** for calculations
* **Permissible operations** for mathematical computation

Primary Tensor Types and Examples

#### **1. Integer Tensors**

* **Definition:** Elements are whole numbers, represented by the set of integers $$\mathbb{Z}$$
* Example:
  * Rank-1\
    &#x20;$$T_1 = \begin{bmatrix} 1 & 2 & 3 \\ -4 & 0 & 5 \end{bmatrix}$$
  * Rank-3\
    $$T_2 = \left[ \begin{bmatrix} 1 & 2 \\ 3 & 4 \end{bmatrix}, \begin{bmatrix} 5 & 6 \\ 7 & 8 \end{bmatrix} \right]$$

#### **2. Floating-Point Tensors**

* **Definition:** Elements are real numbers $$\mathbb{R}$$ with decimal precision, allowing for more precise calculations.
* Example:
  * Matrix (Rank 2) Tensor:\
    $$F_1 = \begin{bmatrix} 1.5 & 2.7 & 3.14 \\ -4.0 & 0.01 & 5.67 \end{bmatrix}$$
  * Scalar (Rank 0) Tensor:\
    $$F_2 = 3.14159$$

#### **3. Complex Tensors**

* Definition: Elements are complex numbers, represented $$\mathbb{C}$$ by , and contain both real and imaginary parts.
* Example:
  * Complex Matrix (Rank 2) Tensor: \
    $$C_1 = \begin{bmatrix} 1 + 2i & 2 - 3i \\ 4 + 0i & 5 - 1i \end{bmatrix}$$
  * Complex Scalar (Rank 0) Tensor:\
    $$C_2 = 2 + 3i$$

#### **4. Boolean Tensors**

* Definition: Elements are binary values {True, False}
* Example:
  * Boolean Tensor (Rank 2):\
    $$B_1 = \begin{bmatrix} \text{True} & \text{False} & \text{True} \\ \text{False} & \text{True} & \text{False} \end{bmatrix}$$
  * Mask Example (Conditions):\
    $$\text{Mask} = \begin{bmatrix} 1 > 0 & 2 < 1 & 3 == 3 \end{bmatrix} = \begin{bmatrix} \text{True} & \text{False} & \text{True} \end{bmatrix}$$

#### **5. Mixed-Type Operations**:&#x20;

When performing operations between tensors of different types, type conversion rules apply:

* Integer + Float → Float
* Real + Complex → Complex
* Example:
  * Integer Tensor:\
    $$A = \begin{bmatrix} 1 & 2 \\ 3 & 4 \end{bmatrix}$$
  * Float Tensor:\
    $$B = \begin{bmatrix} 1.5 & 2.5 \\ 3.5 & 4.5 \end{bmatrix}$$
  * Sum Result (Float Tensor):\
    $$A + B = \begin{bmatrix} 1 + 1.5 & 2 + 2.5 \\ 3 + 3.5 & 4 + 4.5 \end{bmatrix} = \begin{bmatrix} 2.5 & 4.5 \\ 6.5 & 8.5 \end{bmatrix}$$

#### **6. Special Cases**:

a) **Rational Tensors**

* **Definition:** Elements are rational numbers, $$\mathbb{Q}$$, represented as fractions.
* Example:\
  $$R = \begin{bmatrix} \frac{1}{2} & \frac{2}{3} & \frac{3}{4} \\ \frac{4}{5} & \frac{5}{6} & \frac{6}{7} \end{bmatrix}$$

b) **Categorical Tensors**

* Definition: Elements represent categories or labels, often used in machine learning for classification.
* Example:\
  $$C = \begin{bmatrix} 1 & 0 & 0 \\ 0 & 1 & 0 \\ 0 & 0 & 1 \end{bmatrix}$$

#### Important Properties of Tensor Types

1. **Type Consistency:** All elements within a tensor are of the same type.
2. **Type Preservation:** Operations between tensors of the same type maintain that type.
3. **Type Coercion:** Mixed operations follow standard mathematical type promotion rules.

#### Practical Applications of Tensor Types

Tensor types play essential roles across various domains in machine learning:

* **Neural Networks:**
  * **Weights/Biases:** Generally stored as `float32` or `float64` for precision.
  * **Input Data:** Data type varies based on the application domain.
* **Image Processing:**
  * **Pixel Values:** Often use `uint8` for values between 0 and 255 or `float32` for normalized values between 0 and 1.
  * **Masks:** Boolean tensors are used for masking regions.
* **Natural Language Processing (NLP):**
  * **Word Embeddings:** Stored as `float32` for efficiency.
  * **Token IDs:** Represented as `int32` for indexing words.

Understanding tensor types enables efficient data handling and accurate computation, ensuring optimal performance in mathematical and machine learning applications.

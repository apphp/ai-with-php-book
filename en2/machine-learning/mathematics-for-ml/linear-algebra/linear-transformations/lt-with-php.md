# LT with PHP

Here some examples of linear transformation, implemented with PHP.

### 1. **Application of Matrices in Transforming Data**

#### Example:

Here, the matrix $$A = \begin{bmatrix} 2 & 0 \\ 0 & 3 \end{bmatrix}$$ scales x-coordinate by 2 and y-coordinate by 3.

```php
// Matrix A representing the transformation
$A = [
    [2, 0],
    [0, 3]
];

// Vector x
$x = [1, 2];

// Function for matrix-vector multiplication
function transform($matrix, $vector) {
    $result = [];
    foreach ($matrix as $row) {
        $result[] = $row[0] * $vector[0] + $row[1] * $vector[1];
    }
    return $result;
}

$result = transform($A, $x);

echo "Transformed Vector: [" . implode(", ", $result) . "]";
```

**Output:**&#x20;

`Transformed Vector: [2, 6]`

**Result Visualization**:

<div align="left"><figure><img src="../../../../.gitbook/assets/image (4) (1).png" alt="" width="375"><figcaption></figcaption></figure></div>

<div align="left"><figure><img src="../../../../.gitbook/assets/image (2) (1).png" alt="" width="375"><figcaption></figcaption></figure></div>

### 2. Linear Transformations in  Neural Networks

In neural networks, linear transformations are represented as:  𝑦

𝑊 𝑥 + 𝑏 . y=Wx+b. Here, 𝑊 W is a weight matrix, 𝑥 x is the input, and 𝑏 b is the bias vector.

#### Example: Simple Linear Layer in PHP

```php
<?php
// Linear transformation: y = Wx + b
function linearLayer($weights, $bias, $input) {
    $output = [];
    foreach ($weights as $i => $weight) {
        $sum = $weight[0] * $input[0] + $weight[1] * $input[1];
        $output[] = $sum + $bias[$i];
    }
    return $output;
}

$W = [[2, -1], [1, 3]]; // Weights
$b = [1, 0];           // Biases
$x = [1, 2];           // Input

$result = linearLayer($W, $b, $x);

echo "Output after Linear Layer: [" . implode(", ", $result) . "]";
?>
```

**Output:**

`Output after Linear Layer: [1, 7]`

**Result Visualization**:

<div align="left"><figure><img src="../../../../.gitbook/assets/image (2).png" alt="" width="375"><figcaption></figcaption></figure></div>

<div align="left"><figure><img src="../../../../.gitbook/assets/image (3).png" alt="" width="375"><figcaption></figcaption></figure></div>

### **3. Linear Transformations in Neural Networks**

In a neural network, each layer applies a linear transformation followed by an activation function:

y=Wx+b.\mathbf{y} = W\mathbf{x} + \mathbf{b}.y=Wx+b.

Here:

* WWW is the weight matrix, representing the linear transformation.
* x\mathbf{x}x is the input vector.
* b\mathbf{b}b is the bias vector.

#### **Example: Transformation in a Fully Connected Layer**

**PHP Code: Linear Transformation with Bias**

```php
<?php
function linearTransform($weights, $bias, $input) {
    $output = [];
    foreach ($weights as $i => $row) {
        $sum = 0;
        foreach ($row as $j => $w) {
            $sum += $w * $input[$j];
        }
        $output[] = $sum + $bias[$i];
    }
    return $output;
}

$weights = [[3, 2], [-1, 4]];  // Weight matrix W
$bias = [1, -2];               // Bias vector b
$input = [1, 2];               // Input vector x

$result = linearTransform($weights, $bias, $input);

echo "Output: [" . implode(", ", $result) . "]";
?>
```

**Mathematical Representation**:

y=\[32−14]\[12]+\[1−2].\mathbf{y} = \begin{bmatrix} 3 & 2 \\\ -1 & 4 \end{bmatrix} \begin{bmatrix} 1 \\\ 2 \end{bmatrix} + \begin{bmatrix} 1 \\\ -2 \end{bmatrix}.y=\[3−1​24​]\[12​]+\[1−2​].

Performing the calculation:

y=\[77]+\[1−2]=\[85].\mathbf{y} = \begin{bmatrix} 7 \\\ 7 \end{bmatrix} + \begin{bmatrix} 1 \\\ -2 \end{bmatrix} = \begin{bmatrix} 8 \\\ 5 \end{bmatrix}.y=\[77​]+\[1−2​]=\[85​].

**Output:**\
`Output: [8, 5]`



### **4. Activation Functions and the Importance of Nonlinearities**

Linear transformations alone cannot solve complex, nonlinear problems. **Activation functions** like ReLU or Sigmoid introduce nonlinearity to the network.

#### **Example 4: ReLU Activation**

The ReLU function is defined as:

ReLU(x)=max⁡(0,x).\text{ReLU}(x) = \max(0, x).ReLU(x)=max(0,x).

**PHP Code:**

```php
phpCopy code<?php
function relu($vector) {
    return array_map(function($v) { return max(0, $v); }, $vector);
}

$output = [1, -3, 7, -2];
$result = relu($output);

echo "ReLU Output: [" . implode(", ", $result) . "]";
?>
```

**Output:**\
`ReLU Output: [1, 0, 7, 0]`

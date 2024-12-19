# LT with PHP

### 1. **Application of Matrices in Transforming Data**

#### Example in PHP:

```php
<?php
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
?>
```

**Output:**\
`Transformed Vector: [2, 6]`

Here, the matrix A=\[2003]A = \begin{bmatrix} 2 & 0 \\\ 0 & 3 \end{bmatrix} scales xx-coordinate by 2 and yy-coordinate by 3.



### 2. Linear Transformations in  Neural Networks

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

**Output:**\
`Output after Linear Layer: [1, 7]`



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
phpCopy code<?php
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

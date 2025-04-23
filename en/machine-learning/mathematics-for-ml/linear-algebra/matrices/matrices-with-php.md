# Matrices with PHP

### Matrix Operations with Rubix

Rubix ML provides convenient tools for working with matrices, making it an excellent choice for data processing in machine learning with PHP. The library includes classes for creating, manipulating, and performing operations on matrices, such as transposition, multiplication, normalization, and decomposition. By using the Matrix class from Rubix ML, developers can efficiently handle numerical data, preprocess it before training models, and execute complex mathematical computations without writing low-level code.

**Example of Use:**

<details>

<summary>Example with Rubix</summary>

```php
// Import the Matrix class from Tensor library
use Tensor\Matrix;
use Tensor\Vector;
use Tensor\ColumnVector;

// -------------------------------------------------------------------------
// 1. Creating Matrices
// -------------------------------------------------------------------------
echo "=== CREATING MATRICES\n--------------------------------------\n";

// Create a matrix from a 2D array
$data = [
    [1, 2, 3],
    [4, 5, 6],
    [7, 8, 9],
];
$matrixA = Matrix::quick($data);
displayMatrix($matrixA, "Matrix A (created from array)");

// Create special matrices
$identity = Matrix::identity(3);
displayMatrix($identity, "3x3 Identity Matrix");

$zeros = Matrix::zeros(2, 3);
displayMatrix($zeros, "2x3 Zero Matrix");

$ones = Matrix::ones(2, 2);
displayMatrix($ones, "2x2 Ones Matrix");

// -------------------------------------------------------------------------
// 2. Matrix Properties
// -------------------------------------------------------------------------
echo "=== MATRIX PROPERTIES\n--------------------------------------\n";

echo "Matrix A dimensions: " . $matrixA->m() . " rows x " . $matrixA->n() . " columns\n";
echo "Matrix A shape: [" . $matrixA->shape()[0] . ", " . $matrixA->shape()[1] . "]\n";
echo "Matrix A size (total elements): " . $matrixA->size() . "\n";
echo "Is Matrix A square? " . ($matrixA->isSquare() ? 'Yes' : 'No') . "\n\n";

// -------------------------------------------------------------------------
// 3. Accessing Matrix Elements
// -------------------------------------------------------------------------
echo "=== ACCESSING MATRIX ELEMENTS\n--------------------------------------\n";

// Get a single element using array access
$array = $matrixA->asArray();
echo "Element at row 1, column 2: " . $array[1][2] . "\n\n";

// -------------------------------------------------------------------------
// 4. Basic Matrix Operations
// -------------------------------------------------------------------------
echo "=== BASIC MATRIX OPERATIONS\n--------------------------------------\n";

$matrixB = Matrix::quick([
    [9, 8, 7],
    [6, 5, 4],
    [3, 2, 1],
]);
displayMatrix($matrixB, "Matrix B");

// Matrix addition
$sum = $matrixA->add($matrixB);
displayMatrix($sum, "A + B");

// Matrix subtraction
$difference = $matrixA->subtract($matrixB);
displayMatrix($difference, "A - B");

// Matrix multiplication
$product = $matrixA->matmul($matrixB);
displayMatrix($product, "A * B (matrix multiplication)");

// Element-wise multiplication
$hadamard = $matrixA->multiply($matrixB);
displayMatrix($hadamard, "A ∘ B (element-wise multiplication)");

// Element-wise division
$division = $matrixA->divide($matrixB);
displayMatrix($division, "A / B (element-wise division)");

// Scalar addition
$scalarAdd = $matrixA->add(5);
displayMatrix($scalarAdd, "A + 5");

// Scalar multiplication
$scalarMul = $matrixA->multiply(2);
displayMatrix($scalarMul, "A * 2");

// -------------------------------------------------------------------------
// 5. Matrix Transformations
// -------------------------------------------------------------------------
echo "=== MATRIX TRANSFORMATIONS\n--------------------------------------\n";

// Transpose
$transpose = $matrixA->transpose();
displayMatrix($transpose, "Transpose of A");

// Matrix inverse (for a well-conditioned matrix)
$invertibleMatrix = Matrix::quick([
    [4, 7],
    [2, 6]
]);
displayMatrix($invertibleMatrix, "Invertible Matrix");

try {
    $inverse = $invertibleMatrix->inverse();
    displayMatrix($inverse, "Inverse Matrix");
} catch (Exception $e) {
    echo "Matrix inversion failed: " . $e->getMessage() . "\n\n";
}

// Absolute value (element-wise)
$absolute = $matrixA->abs();
displayMatrix($absolute, "Absolute value of A");

// -------------------------------------------------------------------------
// 6. Matrix Reductions
// -------------------------------------------------------------------------
echo "=== MATRIX REDUCTIONS\n--------------------------------------\n";

// Sum of all elements
echo "Sum of all elements in A: " . displayColumnVector($matrixA->sum());

// Product of all elements
echo "Product of all elements in A: " . displayColumnVector($matrixA->product());

// Minimum value
echo "Minimum value in A: " . displayColumnVector($matrixA->min());

// Maximum value
echo "Maximum value in A: " . displayColumnVector($matrixA->max());

// Mean of all elements
echo "Mean of all elements in A: " . displayColumnVector($matrixA->mean());

// -------------------------------------------------------------------------
// 7. Row and Column Operations
// -------------------------------------------------------------------------
echo "=== ROW AND COLUMN OPERATIONS\n--------------------------------------\n";

// Safe row sums
try {
    // Attempt to get row sums as a vector
    $rowSums = $matrixA->sum(1); // Axis 1 for rows
    displayVector($rowSums, "Row sums");
} catch (Exception $e) {
    // Manual calculation if method fails
    echo "Row sums (calculated manually):\n";
    $array = $matrixA->asArray();
    foreach ($array as $i => $row) {
        echo "Row $i: " . array_sum($row) . "\n";
    }
    echo "\n";
}

// Safe column sums
try {
    // Attempt to get column sums as a vector
    $colSums = $matrixA->sum(0); // Axis 0 for columns
    displayVector($colSums, "Column sums");
} catch (Exception $e) {
    // Manual calculation if method fails
    echo "Column sums (calculated manually): [";
    $array = $matrixA->asArray();
    for ($j = 0; $j < $matrixA->n(); $j++) {
        $sum = 0;
        for ($i = 0; $i < $matrixA->m(); $i++) {
            $sum += $array[$i][$j];
        }
        echo sprintf("%.0f", $sum);
        if ($j < $matrixA->n() - 1) {
            echo ", ";
        }
    }
    echo "]\n\n";
}

// -------------------------------------------------------------------------
// 8. Determinant and Trace
// -------------------------------------------------------------------------
echo "=== DETERMINANT AND TRACE\n--------------------------------------\n";

// Determinant
try {
    $det = $matrixA->det();
    echo "Determinant of A: " . $det . "\n";
} catch (Exception $e) {
    echo "Could not calculate determinant: " . $e->getMessage() . "\n";
}

// Trace - calculated manually
// Extract diagonal elements manually - avoid using potentially problematic methods
echo "Diagonal elements of A (calculated manually): [";
$array = $matrixA->asArray();
$min = min($matrixA->m(), $matrixA->n());
for ($i = 0; $i < $min; $i++) {
    echo sprintf("%.0f", $array[$i][$i]);
    if ($i < $min - 1) {
        echo ", ";
    }
}
echo "]\n\n";

// If you need to create a diagonal matrix from array:
// This is what diagonal() actually does in Tensor - it creates a new diagonal matrix
try {
    $diagonalElements = [1, 2, 3];
    $diagonalMatrix = Matrix::diagonal($diagonalElements);
    echo "Diagonal matrix created from [1, 2, 3]:\n";
    $arrayDiag = $diagonalMatrix->asArray();
    foreach ($arrayDiag as $row) {
        echo "[";
        $firstCol = true;
        foreach ($row as $value) {
            if (!$firstCol) echo ", ";
            echo sprintf("%.0f", $value);
            $firstCol = false;
        }
        echo "]\n";
    }
    echo "\n";
} catch (Exception $e) {
    echo "Could not create diagonal matrix: " . $e->getMessage() . "\n\n";
}

/**
 * Safely convert any Tensor object to string
 * This function checks the object type and handles it appropriately
 *
 * @param mixed $object Any object or value to safely convert
 * @return string String representation of the object
 */
function tensorToString($object) {
    // If it's already a string, return it
    if (is_string($object)) {
        return $object;
    }

    // If it's a scalar value, convert directly
    if (is_scalar($object)) {
        return (string)$object;
    }

    // If it's an array, process it
    if (is_array($object)) {
        $result = "[";
        $first = true;
        foreach ($object as $item) {
            if (!$first) $result .= ", ";
            $result .= tensorToString($item);
            $first = false;
        }
        $result .= "]";
        return $result;
    }

    // Handle Tensor Matrix
    if ($object instanceof Matrix) {
        $rows = $object->asArray();
        $result = "";
        foreach ($rows as $i => $row) {
            if ($i > 0) $result .= "\n";
            $result .= "[";
            $first = true;
            foreach ($row as $value) {
                if (!$first) $result .= ", ";
                $result .= sprintf("%.0f", $value);
                $first = false;
            }
            $result .= "]";
        }
        return $result;
    }

    // Handle Tensor Vector
    if ($object instanceof Vector) {
        $values = $object->asArray();
        $result = "[";
        $first = true;
        foreach ($values as $value) {
            if (!$first) $result .= ", ";
            $result .= sprintf("%.0f", $value);
            $first = false;
        }
        $result .= "]";
        return $result;
    }

    // Handle Tensor ColumnVector
    if ($object instanceof ColumnVector) {
        $values = $object->asArray();
        $result = "[";
        $first = true;
        foreach ($values as $value) {
            if (!$first) $result .= ", ";
            // Handle nested arrays that might come from column vectors
            if (is_array($value) && count($value) == 1) {
                $result .= sprintf("%.0f", $value[0]);
            } else {
                $result .= sprintf("%.0f", $value);
            }
            $first = false;
        }
        $result .= "]";
        return $result;
    }

    // For any other object, return the class name to avoid string conversion errors
    if (is_object($object)) {
        return "Object of class " . get_class($object);
    }

    // Default fallback
    return "Unknown type";
}

/**
 * Display a matrix with a title
 */
function displayMatrix($matrix, $title) {
    echo "$title:\n" . tensorToString($matrix) . "\n\n";
}

/**
 * Display a vector with a title
 */
function displayVector($vector, $title) {
    echo "$title: " . tensorToString($vector) . "\n\n";
}

function displayColumnVector($vector) {
    return tensorToString($vector) . "\n\n";
}
```

</details>

### Matrix Operations with Rubix/Tensor

The RubixML/Tensor library provides high-performance matrix operations for numerical computing in PHP. It offers a Matrix class with support for element-wise arithmetic, transposition, multiplication, decomposition, and advanced transformations. These capabilities are essential for data preprocessing, feature engineering, and machine learning model optimization. With its efficient implementation, Tensor enables seamless matrix computations without external dependencies, making it a powerful tool for AI and data science applications in PHP.

**Example of Use:**

<details>

<summary>Example with Rubix/Tensor</summary>

```php
// Import the necessary classes from Tensor library
use Tensor\Matrix;
use Tensor\Vector;
use Tensor\ColumnVector;
use Tensor\Tensor;

// -------------------------------------------------------------------------
// 1. Creating Matrices
// -------------------------------------------------------------------------
echo "=== CREATING MATRICES\n--------------------------------------\n";

// Create a matrix from a 2D array
$data = [
    [1, 2, 3],
    [4, 5, 6],
    [7, 8, 9],
];
$matrixA = Matrix::quick($data);
displayMatrix($matrixA, "Matrix A (created from array)");

// Alternative way to create a matrix
$matrixAlternative = Matrix::build([
    [1, 2, 3],
    [4, 5, 6],
    [7, 8, 9],
]);
displayMatrix($matrixAlternative, "Matrix A (created using build)");

// Create special matrices
$identity = Matrix::identity(3);
displayMatrix($identity, "3x3 Identity Matrix");

$zeros = Matrix::zeros(2, 3);
displayMatrix($zeros, "2x3 Zero Matrix");

$ones = Matrix::ones(2, 2);
displayMatrix($ones, "2x2 Ones Matrix");

$random = Matrix::rand(3, 3);
displayMatrix($random, "3x3 Random Matrix");

$uniform = Matrix::uniform(3, 3);
displayMatrix($uniform, "3x3 Uniform Matrix");

$diagonal = Matrix::diagonal([1, 2, 3]);
displayMatrix($diagonal, "3x3 Diagonal Matrix");

// Create from a range (manual implementation since range() might not be available)
$rangeFlatArray = range(1, 9);
$rangeData = [
    array_slice($rangeFlatArray, 0, 3),
    array_slice($rangeFlatArray, 3, 3),
    array_slice($rangeFlatArray, 6, 3)
];
$range = Matrix::quick($rangeData);
displayMatrix($range, "3x3 Range Matrix (1-9) (manually created)");

// -------------------------------------------------------------------------
// 2. Matrix Properties
// -------------------------------------------------------------------------
echo "\n=== MATRIX PROPERTIES\n--------------------------------------\n";

echo "Matrix A dimensions: " . $matrixA->m() . " rows x " . $matrixA->n() . " columns\n";
echo "Matrix A shape: [" . implode(", ", $matrixA->shape()) . "]\n";
echo "Matrix A size (total elements): " . $matrixA->size() . "\n";
echo "Is Matrix A square? " . ($matrixA->isSquare() ? 'Yes' : 'No') . "\n";

// Manual check for symmetry
function isSymmetric($matrix) {
    if (!$matrix->isSquare()) {
        return false;
    }

    $array = $matrix->asArray();
    $n = $matrix->n();

    for ($i = 0; $i < $n; $i++) {
        for ($j = 0; $j < $i; $j++) {
            if ($array[$i][$j] != $array[$j][$i]) {
                return false;
            }
        }
    }

    return true;
}

echo "Is Identity Matrix symmetric? " . (isSymmetric($identity) ? 'Yes' : 'No') . "\n";
echo "Is Matrix A symmetric? " . (isSymmetric($matrixA) ? 'Yes' : 'No') . "\n";

// Check for positive definiteness (manual implementation)
// Note: A matrix is positive definite if all eigenvalues are positive
function isPositiveDefinite($matrix) {
    if (!$matrix->isSquare()) {
        return false;
    }

    try {
        $eigenArray = $matrix->eig();

        foreach ($eigenArray as $value) {
            if ($value <= 0) {
                return false;
            }
        }

        return true;
    } catch (Exception $e) {
        return false;
    }
}

echo "Is Matrix A positive definite? " . (isPositiveDefinite($matrixA) ? 'Yes' : 'No') . "\n\n";


// -------------------------------------------------------------------------
// 3. Accessing Matrix Elements
// -------------------------------------------------------------------------
echo "\n=== ACCESSING MATRIX ELEMENTS\n--------------------------------------\n";

// Get a single element using array access
$array = $matrixA->asArray();
echo "Element at row 1, column 2: " . $array[1][2] . "\n";

// Get a row as a vector
$row = $matrixA->rowAsVector(1);
displayVector($row, "Second row as a vector");

// Get a column as a vector
$column = $matrixA->columnAsVector(1);
displayColumnVector($column, "Second column as a column vector");

// -------------------------------------------------------------------------
// 4. Basic Matrix Operations
// -------------------------------------------------------------------------
echo "\n=== BASIC MATRIX OPERATIONS\n--------------------------------------\n";

$matrixB = Matrix::quick([
    [9, 8, 7],
    [6, 5, 4],
    [3, 2, 1],
]);
displayMatrix($matrixB, "Matrix B");

// Matrix addition
$sum = $matrixA->add($matrixB);
displayMatrix($sum, "A + B");

// Matrix subtraction
$difference = $matrixA->subtract($matrixB);
displayMatrix($difference, "A - B");

// Matrix multiplication
$product = $matrixA->matmul($matrixB);
displayMatrix($product, "A * B (matrix multiplication)");

// Element-wise multiplication
$hadamard = $matrixA->multiply($matrixB);
displayMatrix($hadamard, "A ∘ B (element-wise multiplication)");

// Element-wise division
$division = $matrixA->divide($matrixB);
displayMatrix($division, "A / B (element-wise division)");

// Scalar addition
$scalarAdd = $matrixA->add(5);
displayMatrix($scalarAdd, "A + 5");

// Scalar multiplication
$scalarMul = $matrixA->multiply(2);
displayMatrix($scalarMul, "A * 2");

// Power operation
$power = $matrixA->pow(2);
displayMatrix($power, "A ^ 2 (element-wise)");

// Matrix power (using matmul)
$matrixPower = $matrixA->matmul($matrixA);
displayMatrix($matrixPower, "A ^ 2 (matrix multiplication)");

// -------------------------------------------------------------------------
// 5. Matrix Transformations
// -------------------------------------------------------------------------
echo "\n=== MATRIX TRANSFORMATIONS\n--------------------------------------\n";

// Transpose
$transpose = $matrixA->transpose();
displayMatrix($transpose, "Transpose of A");

// Matrix inverse (for a well-conditioned matrix)
$invertibleMatrix = Matrix::quick([
    [4, 7],
    [2, 6]
]);
displayMatrix($invertibleMatrix, "Invertible Matrix");

try {
    $inverse = $invertibleMatrix->inverse();
    displayMatrix($inverse, "Inverse Matrix");

    // Verify inverse by multiplying with original
    $identityCheck = $invertibleMatrix->matmul($inverse);
    displayMatrix($identityCheck, "Original * Inverse (should be identity)");
} catch (Exception $e) {
    echo "Matrix inversion failed: " . $e->getMessage() . "\n\n";
}

// Absolute value (element-wise)
$absolute = $matrixA->abs();
displayMatrix($absolute, "Absolute value of A");

// Square root (element-wise)
$sqrt = $matrixA->sqrt();
displayMatrix($sqrt, "Square root of A (element-wise)");

// Exponential (element-wise)
$exp = $matrixA->exp();
displayMatrix($exp, "Exponential of A (element-wise)");

// Log (element-wise)
$log = $matrixA->log();
displayMatrix($log, "Natural log of A (element-wise)");

// -------------------------------------------------------------------------
// 6. Matrix Decompositions
// -------------------------------------------------------------------------
echo "\n=== MATRIX DECOMPOSITIONS\n--------------------------------------\n";

$decomposableMatrix = Matrix::quick([
    [4, 7],
    [2, 6]
]);
displayMatrix($decomposableMatrix, "Decomposable Matrix");

// LU Decomposition
try {
    $lu = $decomposableMatrix->lu();
    displayMatrix($lu->l(), "L (Lower triangular matrix)");
    displayMatrix($lu->u(), "U (Upper triangular matrix)");

    // Verify L*U = A
    $reconstructed = $lu->l()->matmul($lu->u());
    displayMatrix($reconstructed, "L * U (should equal original matrix)");
} catch (Exception $e) {
    echo "LU decomposition failed: " . $e->getMessage() . "\n\n";
}

// Cholesky Decomposition (for positive definite matrices)
$symmetricPD = Matrix::quick([
    [2, -1, 0],
    [-1, 2, -1],
    [0, -1, 2]
]);
displayMatrix($symmetricPD, "Symmetric Positive Definite Matrix");

// Eigendecomposition
try {
    $eig = $decomposableMatrix->eig();
    echo array_to_vector($eig->eigenvalues());
    echo array_to_matrix($eig->eigenvectors()->asArray());
} catch (Exception $e) {
    echo "Eigendecomposition failed: " . $e->getMessage() . "\n\n";
}

// SVD Decomposition
try {
    $svd = $decomposableMatrix->svd();
    displayMatrix($svd->u(), "U (left singular vectors)");
    displayMatrix($svd->s(), "S (singular values as diagonal matrix)");
    displayMatrix($svd->vT(), "V^T (right singular vectors transposed)");

    // Verify U*S*V^T = A
    $reconstructed = $svd->u()->matmul($svd->s())->matmul($svd->vT());
    displayMatrix($reconstructed, "U * S * V^T (should equal original matrix)");
} catch (Exception $e) {
    echo "SVD decomposition failed: " . $e->getMessage() . "\n\n";
}

// -------------------------------------------------------------------------
// 7. Matrix Reductions
// -------------------------------------------------------------------------
echo "\n=== MATRIX REDUCTIONS\n--------------------------------------\n";

// Sum of all elements
echo "Sum of all elements in A: " . displayColumnVector($matrixA->sum(), return: true);

// Product of all elements
echo "Product of all elements in A: " . displayColumnVector($matrixA->product(), return: true);

// Minimum value
echo "Minimum value in A: " . displayColumnVector($matrixA->min(), return: true);

// Maximum value
echo "Maximum value in A: " . displayColumnVector($matrixA->max(), return: true);

// Mean of all elements
echo "Mean of all elements in A: " . displayColumnVector($matrixA->mean(), return: true);

// Variance of all elements
echo "Variance of all elements in A: " . displayColumnVector($matrixA->variance(), return: true);

// Median of all elements
echo "Median of all elements in A: " . displayColumnVector($matrixA->median(), return: true) . "\n";


// -------------------------------------------------------------------------
// 8. Determinant and Trace
// -------------------------------------------------------------------------
echo "\n=== DETERMINANT AND TRACE\n--------------------------------------\n";

// Determinant
$det = $matrixA->det();
echo "Determinant of Matrix A: " . $det . "\n";

// Trace (sum of diagonal elements)
$trace = 0;
$array = $matrixA->asArray();
$min = min($matrixA->m(), $matrixA->n());
for ($i = 0; $i < $min; $i++) {
    $trace += $array[$i][$i];
}
echo "Trace of Matrix A: " . $trace . "\n\n";

// -------------------------------------------------------------------------
// Helper functions to display matrix and vector objects
// -------------------------------------------------------------------------

/**
 * Display a matrix with a title
 */
function displayMatrix($matrix, $title) {
    echo "$title:\n";
    $array = $matrix->asArray();
    foreach ($array as $row) {
        echo "[";
        echo implode(", ", array_map(function($val) {
            return is_float($val) ? number_format($val, 0) : $val;
        }, $row));
        echo "]\n";
    }
    echo "\n";
}

/**
 * Display a vector with a title
 */
function displayVector($vector, $title) {
    echo "$title: [";
    echo implode(", ", array_map(function($val) {
        return is_float($val) ? number_format($val, 0) : $val;
    }, $vector->asArray()));
    echo "]\n\n";
}

/**
 * Display a column vector with a title
 */
function displayColumnVector($vector, $title = '', $return = false) {
    $result = '';

    if ($title) {
        $result .= "$title:\n";
    }
    $array = $vector->asArray();
    foreach ($array as $val) {
        $result .= "[" . (is_float($val) ? number_format($val, 4) : $val) . "]\n";
    }
    $result .= "\n";

    if ($return) {
        return $result;
    }

    echo $result;
}


```

</details>

### Matrix Operations with MathPHP

MathPHP is a PHP library designed for advanced mathematical operations, including matrix operations. Below is an overview of how you can use MathPHP to perform matrix operations such as addition, subtraction, multiplication, transposition, inverting and much more.

<details>

<summary>Example with MathPHP</summary>

```php
use MathPHP\LinearAlgebra\Matrix;
use MathPHP\LinearAlgebra\MatrixFactory;
use MathPHP\LinearAlgebra\Vector;
use MathPHP\Exception\MatrixException;

// Section 1: Matrix Creation and Basic Operations
echo "=== MATRIX CREATION AND BASIC OPERATIONS\n--------------------------------------\n\n";

// Create matrices from arrays
$matrixA = MatrixFactory::create([
    [4, 3, 2],
    [1, 5, 7],
    [8, 6, 9]
]);

$matrixB = MatrixFactory::create([
    [1, 2, 3],
    [4, 5, 6],
    [7, 8, 9]
]);

$vector = new Vector([1, 2, 3]);

// Display basic matrix information
echo "Matrix A:\n" . $matrixA . "\n";
echo "Number of rows: " . $matrixA->getM() . "\n";
echo "Number of columns: " . $matrixA->getN() . "\n\n";

// Get individual elements and sub-components
echo "Element at position (1,2): " . $matrixA->get(1, 2) . "\n";
echo "Row 1: " . implode(", ", $matrixA->getRow(1)) . "\n";
echo "Column 2: " . implode(", ", $matrixA->getColumn(2)) . "\n\n";

// Matrix arithmetic operations
echo "=== MATRIX ARITHMETIC\n--------------------------------------\n\n";
echo "A + B:\n" . $matrixA->add($matrixB) . "\n\n";
echo "A - B:\n" . $matrixA->subtract($matrixB) . "\n\n";
echo "A * B:\n" . $matrixA->multiply($matrixB) . "\n\n";
echo "2 * A:\n" . $matrixA->scalarMultiply(2) . "\n\n";
echo "A / 2:\n" . $matrixA->scalarDivide(2) . "\n\n";
echo "A ∘ B (Hadamard product):\n" . $matrixA->hadamardProduct($matrixB) . "\n\n";

// Section 2: Row and Column Operations
echo "=== ROW AND COLUMN OPERATIONS\n--------------------------------------\n\n";

// Row operations
echo "Original Matrix A:\n" . $matrixA . "\n\n";
echo "Row interchange (0 and 2):\n" . $matrixA->rowInterchange(0, 2) . "\n\n";
echo "Row exclude (1):\n" . $matrixA->rowExclude(1) . "\n\n";
echo "Row multiply (row 1 * 3):\n" . $matrixA->rowMultiply(1, 3) . "\n\n";
echo "Row add (2 * row 0 to row 2):\n" . $matrixA->rowAdd(0, 2, 2) . "\n\n";

// Column operations
echo "Column interchange (0 and 2):\n" . $matrixA->columnInterchange(0, 2) . "\n\n";
echo "Column exclude (1):\n" . $matrixA->columnExclude(1) . "\n\n";
echo "Column multiply (column 1 * 3):\n" . $matrixA->columnMultiply(1, 3) . "\n\n";

// Section 3: Matrix Augmentations
echo "=== MATRIX AUGMENTATIONS\n--------------------------------------\n\n";

// Create identity matrix for augmentation
$identity = MatrixFactory::identity(3);
echo "Identity matrix:\n" . $identity . "\n\n";

// Augment matrices
echo "Augment A with Identity (A|I):\n" . $matrixA->augmentIdentity() . "\n\n";
echo "Augment A with B (A|B):\n" . $matrixA->augment($matrixB) . "\n\n";
echo "Augment A below B:\n" . $matrixA->augmentBelow($matrixB) . "\n\n";

// Section 4: Matrix Properties and Values
echo "=== MATRIX PROPERTIES AND VALUES\n--------------------------------------\n\n";
echo "Trace of A: " . $matrixA->trace() . "\n";
echo "Determinant of A: " . $matrixA->det() . "\n";
echo "Rank of A: " . $matrixA->rank() . "\n\n";

// Check matrix properties
echo "Is A square? " . ($matrixA->isSquare() ? "Yes" : "No") . "\n";
echo "Is A symmetric? " . ($matrixA->isSymmetric() ? "Yes" : "No") . "\n";
echo "Is A invertible? " . ($matrixA->isInvertible() ? "Yes" : "No") . "\n";
echo "Is A diagonal? " . ($matrixA->isDiagonal() ? "Yes" : "No") . "\n\n";

// Section 5: Matrix Operations
echo "=== MATRIX OPERATIONS\n--------------------------------------\n\n";
echo "Transpose of A:\n" . $matrixA->transpose() . "\n\n";

try {
    echo "Inverse of A:\n" . $matrixA->inverse() . "\n\n";
} catch (MatrixException $e) {
    echo "Matrix is not invertible: " . $e->getMessage() . "\n\n";
}

echo "Cofactor matrix of A:\n" . $matrixA->cofactorMatrix() . "\n\n";
echo "Adjugate of A:\n" . $matrixA->adjugate() . "\n\n";

// Section 6: Matrix Norms
echo "=== MATRIX NORMS\n--------------------------------------\n\n";
echo "One norm of A: " . $matrixA->oneNorm() . "\n";
echo "Infinity norm of A: " . $matrixA->infinityNorm() . "\n";
echo "Frobenius norm of A: " . $matrixA->frobeniusNorm() . "\n\n";

// Section 7: Matrix Reductions
echo "=== MATRIX REDUCTIONS\n--------------------------------------\n\n";
echo "Row echelon form (REF) of A:\n" . $matrixA->ref() . "\n\n";
echo "Reduced row echelon form (RREF) of A:\n" . $matrixA->rref() . "\n\n";

// Section 8: Matrix Decompositions
echo "=== MATRIX DECOMPOSITIONS\n--------------------------------------\n\n";

// LU Decomposition - Handle as object with properties
try {
    $lu = $matrixA->luDecomposition();
    echo "LU Decomposition:\n";

    // Check how to access components (as properties, not array keys)
    if (property_exists($lu, 'L') && property_exists($lu, 'U')) {
        echo "L matrix:\n" . $lu->L . "\n\n";
        echo "U matrix:\n" . $lu->U . "\n\n";
        if (property_exists($lu, 'P')) {
            echo "P matrix:\n" . $lu->P . "\n\n";
        }
    } else {
        // Fallback: try to access as array for compatibility
        echo "L matrix:\n" . (isset($lu['L']) ? $lu['L'] : "Not available") . "\n";
        echo "U matrix:\n" . (isset($lu['U']) ? $lu['U'] : "Not available") . "\n";
        echo "P matrix:\n" . (isset($lu['P']) ? $lu['P'] : "Not available") . "\n";
    }
    echo "\n";
} catch (Exception $e) {
    echo "LU Decomposition failed: " . $e->getMessage() . "\n\n";
}

// QR Decomposition - similar pattern
try {
    $qr = $matrixA->qrDecomposition();
    echo "QR Decomposition:\n";

    // Check how to access components
    if (property_exists($qr, 'Q') && property_exists($qr, 'R')) {
        echo "Q matrix:\n" . $qr->Q . "\n\n";
        echo "R matrix:\n" . $qr->R . "\n\n";
    } else {
        echo "Q matrix:\n" . (isset($qr['Q']) ? $qr['Q'] : "Not available") . "\n\n";
        echo "R matrix:\n" . (isset($qr['R']) ? $qr['R'] : "Not available") . "\n\n";
    }
    echo "\n";
} catch (Exception $e) {
    echo "QR Decomposition failed: " . $e->getMessage() . "\n\n";
}

// Eigenvalues and Eigenvectors
echo "=== EIGENVALUES AND EIGENVECTORS\n--------------------------------------\n\n";

// Create a symmetric matrix for eigenvalue demonstration
$symmetricMatrix = MatrixFactory::create([
    [4, 1, 1],
    [1, 3, 1],
    [1, 1, 5]
]);

echo "Symmetric Matrix:\n" . $symmetricMatrix . "\n\n";

try {
    $eigenvalues = $symmetricMatrix->eigenvalues();
    echo "Eigenvalues:\n";
    print_r($eigenvalues);

    $eigenvectors = $symmetricMatrix->eigenvectors();
    echo "\nEigenvectors:\n";
    foreach ($eigenvectors as $index => $eigenvector) {
        // Handle Vector objects properly
        if ($eigenvector instanceof Vector) {
            echo "Eigenvector " . ($index + 1) . ": [" . implode(", ", $eigenvector->getVector()) . "]\n";
        } else {
            echo "Eigenvector " . ($index + 1) . ": " . print_r($eigenvector, true) . "\n";
        }
    }
    echo "\n";
} catch (Exception $e) {
    echo "Eigenvalue calculation failed: " . $e->getMessage() . "\n\n";
}

// Section 9: Solving Linear Systems
echo "=== SOLVING LINEAR SYSTEMS\n--------------------------------------\n\n";

// Create a system Ax = b
$A = MatrixFactory::create([
    [3, 2, -1],
    [2, -2, 4],
    [-1, 0.5, -1]
]);

$b = new Vector([1, -2, 0]);

echo "System Ax = b:\n";
echo "A:\n" . $A . "\n\n";
echo "b: [" . implode(", ", $b->getVector()) . "]\n";

try {
    $x = $A->solve($b);
    // Handle Vector result
    if ($x instanceof Vector) {
        echo "Solution x: [" . implode(", ", $x->getVector()) . "]\n\n";
    } else {
        echo "Solution x: " . print_r($x, true) . "\n\n";
    }
} catch (Exception $e) {
    echo "Couldn't solve the system: " . $e->getMessage() . "\n\n";
}

// Section 10: Special Matrices
echo "=== SPECIAL MATRICES\n--------------------------------------\n\n";

// Create special matrices
echo "3x3 Identity Matrix:\n" . MatrixFactory::identity(3) . "\n\n";
echo "2x3 Zero Matrix:\n" . MatrixFactory::zero(2, 3) . "\n\n";

// Diagonal matrix
$diagonalMatrix = MatrixFactory::diagonal([1, 2, 3]);
echo "Diagonal Matrix from [1,2,3]:\n" . $diagonalMatrix . "\n\n";

// Vandermonde matrix
try {
    $vandermonde = MatrixFactory::vandermonde([1, 2, 3], 3);
    echo "Vandermonde Matrix:\n" . $vandermonde . "\n\n";
} catch (Exception $e) {
    echo "Vandermonde Matrix creation failed: " . $e->getMessage() . "\n\n";
}

// Hilbert matrix
try {
    $hilbert = MatrixFactory::hilbert(3);
    echo "3x3 Hilbert Matrix:\n" . $hilbert . "\n\n";
} catch (Exception $e) {
    echo "Hilbert Matrix creation failed: " . $e->getMessage() . "\n\n";
}

// Section 11: Matrix Mapping Functions
echo "=== MATRIX FUNCTION MAPPING\n--------------------------------------\n\n";

// Map a function over the elements
$squareFunc = function($x) {
    return $x * $x;
};

echo "Original Matrix A:\n" . $matrixA . "\n\n";
echo "A with each element squared:\n" . $matrixA->map($squareFunc) . "\n\n";
echo "Absolute value of elements in A:\n" . $matrixA->map('abs') . "\n\n";

// Map operations on rows
$rowSums = $matrixA->mapRows('array_sum');
echo "Sum of each row in A: ";
print_r($rowSums);
echo "\n";

// Section 12: Matrix Vector Operations
echo "=== MATRIX VECTOR OPERATIONS\n--------------------------------------\n\n";

echo "Matrix A:\n" . $matrixA . "\n\n";
echo "Vector v: [" . implode(", ", $vector->getVector()) . "]\n\n";

try {
    $result = $matrixA->vectorMultiply($vector);
    // Handle Vector result properly
    if ($result instanceof Vector) {
        echo "A * v: [" . implode(", ", $result->getVector()) . "]\n";
    } else {
        echo "A * v: " . print_r($result, true) . "\n";
    }
} catch (Exception $e) {
    echo "Matrix-vector multiplication failed: " . $e->getMessage() . "\n";
}

// Row and column statistics
// Make sure we're handling array or Vector results properly
try {
    $rowSums = $matrixA->rowSums();
    if (is_array($rowSums)) {
        echo "Row sums: " . implode(", ", $rowSums) . "\n";
    } else if ($rowSums instanceof Vector) {
        echo "Row sums: [" . implode(", ", $rowSums->getVector()) . "]\n";
    }
} catch (Exception $e) {
    echo "Row sums calculation failed: " . $e->getMessage() . "\n";
}

try {
    $colSums = $matrixA->columnSums();
    if (is_array($colSums)) {
        echo "Column sums: " . implode(", ", $colSums) . "\n";
    } else if ($colSums instanceof Vector) {
        echo "Column sums: [" . implode(", ", $colSums->getVector()) . "]\n";
    }
} catch (Exception $e) {
    echo "Column sums calculation failed: " . $e->getMessage() . "\n";
}

try {
    $rowMeans = $matrixA->rowMeans();
    if (is_array($rowMeans)) {
        echo "Row means: " . implode(", ", $rowMeans) . "\n";
    } else if ($rowMeans instanceof Vector) {
        echo "Row means: [" . implode(", ", $rowMeans->getVector()) . "]\n";
    }
} catch (Exception $e) {
    echo "Row means calculation failed: " . $e->getMessage() . "\n";
}

try {
    $colMeans = $matrixA->columnMeans();
    if (is_array($colMeans)) {
        echo "Column means: " . implode(", ", $colMeans) . "\n\n";
    } else if ($colMeans instanceof Vector) {
        echo "Column means: [" . implode(", ", $colMeans->getVector()) . "]\n\n";
    }
} catch (Exception $e) {
    echo "Column means calculation failed: " . $e->getMessage() . "\n\n";
}

// Section 13: Matrix Submatrices and Elements
echo "=== MATRIX SUBMATRICES AND ELEMENTS\n--------------------------------------\n\n";

// Get diagonal elements
try {
    $diagonalElements = $matrixA->getDiagonalElements();
    if (is_array($diagonalElements)) {
        echo "Diagonal elements of A: " . implode(", ", $diagonalElements) . "\n\n";
    } else if ($diagonalElements instanceof Vector) {
        echo "Diagonal elements of A: [" . implode(", ", $diagonalElements->getVector()) . "]\n\n";
    }
} catch (Exception $e) {
    echo "Failed to get diagonal elements: " . $e->getMessage() . "\n";
}

// Submatrix
try {
    $submatrix = $matrixA->submatrix(0, 0, 1, 1);
    echo "Submatrix of A (0,0 to 1,1):\n" . $submatrix . "\n\n";
} catch (Exception $e) {
    echo "Submatrix extraction failed: " . $e->getMessage() . "\n\n";
}

// Minor matrix
try {
    $minorMatrix = $matrixA->minorMatrix(0, 0);
    echo "Minor matrix of A (exclude row 0, col 0):\n" . $minorMatrix . "\n\n";
} catch (Exception $e) {
    echo "Minor matrix extraction failed: " . $e->getMessage() . "\n\n";
}

// Minor value
try {
    $minorValue = $matrixA->minor(0, 0);
    echo "Minor value of A at (0,0): " . $minorValue . "\n\n";
} catch (Exception $e) {
    echo "Minor value calculation failed: " . $e->getMessage() . "\n\n";
}

// Section 14: Matrix as Column Vectors
echo "=== MATRIX AS COLUMN VECTORS\n--------------------------------------\n\n";

try {
    $vectors = $matrixA->asVectors();
    echo "A as column vectors:\n";
    foreach ($vectors as $index => $columnVector) {
        // Handle Vector objects properly
        if ($columnVector instanceof Vector) {
            echo "Column " . $index . ": [" . implode(", ", $columnVector->getVector()) . "]\n";
        } else {
            echo "Column " . $index . ": " . print_r($columnVector, true) . "\n";
        }
    }
} catch (Exception $e) {
    echo "Failed to get columns as vectors: " . $e->getMessage() . "\n";
}

```

</details>

### Matrix Operations with Pure PHP

In PHP it can be written as a class **Matrix** with implementation of a set of matrix operations.

This class is a PHP implementation of matrix operations commonly used in linear algebra and, by extension, in various AI and machine learning algorithms. It provides a robust set of methods for performing matrix calculations, making it a valuable tool for developers working on AI projects in PHP.

<details>

<summary>Example of Class Matrix</summary>

```php
namespace Apphp\MLKit\Math\Linear;

use Exception;

/**
 * Class Matrix
 *
 * Provides basic matrix operations for linear algebra: addition, subtraction, multiplication, determinant, inverse, etc.
 *
 * @package Apphp\MLKit\Math\Linear
 */
class Matrix {
    /**
     * @var array<int, array<int, float>> The matrix data.
     */
    private array $matrix;

    /**
     * @var int Number of rows.
     */
    private int $rows;

    /**
     * @var int Number of columns.
     */
    private int $cols;

    /**
     * Matrix constructor.
     *
     * @param array<int, array<int, float>> $matrix
     * @throws Exception
     */
    public function __construct(array $matrix) {
        if (empty($matrix) || !is_array($matrix[0])) {
            throw new Exception('Matrix must be a non-empty 2D array.');
        }
        $this->matrix = $matrix;
        $this->rows = count($matrix);
        $this->cols = count($matrix[0]);
    }

    /**
     * Get the rank of the matrix.
     *
     * @return int
     */
    public function rank(): int {
        $epsilon = 1e-10; // Threshold for considering a value as zero
        $ref = $this->reducedEchelonForm();
        $rank = 0;
        for ($i = 0; $i < $this->rows; $i++) {
            for ($j = 0; $j < $this->cols; $j++) {
                if (abs($ref->matrix[$i][$j]) > $epsilon) {
                    $rank++;
                    break;
                }
            }
        }

        return $rank;
    }

    /**
     * Add another matrix to this matrix.
     *
     * @param Matrix $other
     * @return Matrix
     * @throws Exception
     */
    public function add(Matrix $other): Matrix {
        if ($this->rows !== $other->rows || $this->cols !== $other->cols) {
            throw new Exception('Matrices must have the same dimensions for addition.');
        }

        $result = [];
        for ($i = 0; $i < $this->rows; $i++) {
            for ($j = 0; $j < $this->cols; $j++) {
                $result[$i][$j] = $this->matrix[$i][$j] + $other->matrix[$i][$j];
            }
        }

        return new Matrix($result);
    }

    /**
     * Subtract another matrix from this matrix.
     *
     * @param Matrix $other
     * @return Matrix
     * @throws Exception
     */
    public function subtract(Matrix $other): Matrix {
        if ($this->rows !== $other->rows || $this->cols !== $other->cols) {
            throw new Exception('Matrices must have the same dimensions for subtraction.');
        }

        $result = [];
        for ($i = 0; $i < $this->rows; $i++) {
            for ($j = 0; $j < $this->cols; $j++) {
                $result[$i][$j] = $this->matrix[$i][$j] - $other->matrix[$i][$j];
            }
        }

        return new Matrix($result);
    }

    /**
     * Multiply this matrix by a scalar.
     *
     * @param float|int $scalar
     * @return Matrix
     */
    public function scalarMultiply(float|int $scalar): Matrix {
        $result = [];
        for ($i = 0; $i < $this->rows; $i++) {
            for ($j = 0; $j < $this->cols; $j++) {
                $result[$i][$j] = $this->matrix[$i][$j] * $scalar;
            }
        }

        return new Matrix($result);
    }

    /**
     * Multiply this matrix by another matrix.
     *
     * @param Matrix $other
     * @return Matrix
     * @throws Exception
     */
    public function multiply(Matrix $other): Matrix {
        if ($this->cols !== $other->rows) {
            throw new Exception('Number of columns in the first matrix must equal the number of rows in the second matrix.');
        }

        $result = [];
        for ($i = 0; $i < $this->rows; $i++) {
            for ($j = 0; $j < $other->cols; $j++) {
                $result[$i][$j] = 0;
                for ($k = 0; $k < $this->cols; $k++) {
                    $result[$i][$j] += $this->matrix[$i][$k] * $other->matrix[$k][$j];
                }
            }
        }

        return new Matrix($result);
    }

    /**
     * Transpose the matrix.
     *
     * @return Matrix
     */
    public function transpose(): Matrix {
        $result = [];
        for ($i = 0; $i < $this->cols; $i++) {
            for ($j = 0; $j < $this->rows; $j++) {
                $result[$i][$j] = $this->matrix[$j][$i];
            }
        }
        return new Matrix($result);
    }

    /**
     * Compute the determinant of the matrix.
     *
     * @return float
     * @throws Exception
     */
    public function determinant(): float {
        if ($this->rows !== $this->cols) {
            throw new Exception('Determinant can only be calculated for square matrices.');
        }

        if ($this->rows === 1) {
            return $this->matrix[0][0];
        }

        if ($this->rows === 2) {
            return $this->matrix[0][0] * $this->matrix[1][1] - $this->matrix[0][1] * $this->matrix[1][0];
        }

        $det = 0;
        for ($j = 0; $j < $this->cols; $j++) {
            $det += (($j % 2 == 0) ? 1 : -1) * $this->matrix[0][$j] * $this->cofactor(0, $j)->determinant();
        }

        return $det;
    }

    /**
     * Get the cofactor matrix after removing a row and column.
     *
     * @param int $row
     * @param int $col
     * @return Matrix
     */
    private function cofactor(int $row, int $col): Matrix {
        $result = [];
        $r = 0;
        for ($i = 0; $i < $this->rows; $i++) {
            if ($i == $row) {
                continue;
            }
            $c = 0;
            for ($j = 0; $j < $this->cols; $j++) {
                if ($j == $col) {
                    continue;
                }
                $result[$r][$c] = $this->matrix[$i][$j];
                $c++;
            }
            $r++;
        }

        return new Matrix($result);
    }

    /**
     * Get the inverse of the matrix.
     *
     * @return Matrix
     * @throws Exception
     */
    public function inverse(): Matrix {
        $det = $this->determinant();
        if ($det == 0) {
            throw new Exception('Matrix is not invertible.');
        }

        $adjoint = $this->adjoint();
        return $adjoint->scalarMultiply(1 / $det);
    }

    /**
     * Get the adjoint of the matrix.
     *
     * @return Matrix
     */
    private function adjoint(): Matrix {
        $result = [];
        for ($i = 0; $i < $this->rows; $i++) {
            for ($j = 0; $j < $this->cols; $j++) {
                $result[$j][$i] = (($i + $j) % 2 == 0 ? 1 : -1) * $this->cofactor($i, $j)->determinant();
            }
        }

        return new Matrix($result);
    }

    /**
     * Get the reduced row echelon form of the matrix.
     *
     * @return Matrix
     */
    private function reducedEchelonForm(): Matrix {
        $ref = $this->matrix;
        $lead = 0;
        for ($r = 0; $r < $this->rows; $r++) {
            if ($lead >= $this->cols) {
                return new Matrix($ref);
            }
            $i = $r;
            while ($ref[$i][$lead] == 0) {
                $i++;
                if ($i == $this->rows) {
                    $i = $r;
                    $lead++;
                    if ($this->cols == $lead) {
                        return new Matrix($ref);
                    }
                }
            }
            $temp = $ref[$i];
            $ref[$i] = $ref[$r];
            $ref[$r] = $temp;
            $lv = $ref[$r][$lead];
            for ($j = 0; $j < $this->cols; $j++) {
                $ref[$r][$j] /= $lv;
            }
            for ($i = 0; $i < $this->rows; $i++) {
                if ($i != $r) {
                    $lv = $ref[$i][$lead];
                    for ($j = 0; $j < $this->cols; $j++) {
                        $ref[$i][$j] -= $lv * $ref[$r][$j];
                    }
                }
            }
            $lead++;
        }
        return new Matrix($ref);
    }

    /**
     * Get the cofactor matrix of the matrix.
     *
     * @return Matrix
     */
    public function cofactorMatrix(): Matrix {
        $result = [];
        for ($i = 0; $i < $this->rows; $i++) {
            for ($j = 0; $j < $this->cols; $j++) {
                $result[$i][$j] = (($i + $j) % 2 == 0 ? 1 : -1) * $this->cofactor($i, $j)->determinant();
            }
        }
        return new Matrix($result);
    }

    /**
     * Get the adjugate matrix (transpose of cofactor matrix).
     *
     * @return Matrix
     */
    public function adjugateMatrix(): Matrix {
        return $this->cofactorMatrix()->transpose();
    }

    /**
     * Get a string representation of the matrix.
     *
     * @return string
     */
    public function toString(): string {
        $result = '';
        for ($i = 0; $i < $this->rows; $i++) {
            $result .= '[' . implode(', ', $this->matrix[$i]) . "]\n";
        }
        return $result;
    }

    /**
     * Check if the matrix is square (rows == cols).
     *
     * @return bool
     */
    public function isSquare(): bool {
        return $this->rows === $this->cols;
    }

    /**
     * Check if the matrix is symmetric (equal to its transpose).
     *
     * @return bool
     */
    public function isSymmetric(): bool {
        if (!$this->isSquare()) {
            return false;
        }
        for ($i = 0; $i < $this->rows; $i++) {
            for ($j = 0; $j < $this->cols; $j++) {
                if ($this->matrix[$i][$j] !== $this->matrix[$j][$i]) {
                    return false;
                }
            }
        }
        return true;
    }

    /**
     * Compute the trace (sum of diagonal elements) of a square matrix.
     *
     * @return float
     * @throws Exception
     */
    public function trace(): float {
        if (!$this->isSquare()) {
            throw new Exception('Trace can only be computed for square matrices.');
        }
        $trace = 0.0;
        for ($i = 0; $i < $this->rows; $i++) {
            $trace += $this->matrix[$i][$i];
        }
        return $trace;
    }

    /**
     * Get a specific row as an array.
     *
     * @param int $i
     * @return array<int, float>
     * @throws Exception
     */
    public function getRow(int $i): array {
        if ($i < 0 || $i >= $this->rows) {
            throw new Exception('Row index out of bounds.');
        }
        return $this->matrix[$i];
    }

    /**
     * Get a specific column as an array.
     *
     * @param int $j
     * @return array<int, float>
     * @throws Exception
     */
    public function getColumn(int $j): array {
        if ($j < 0 || $j >= $this->cols) {
            throw new Exception('Column index out of bounds.');
        }
        $col = [];
        for ($i = 0; $i < $this->rows; $i++) {
            $col[] = $this->matrix[$i][$j];
        }
        return $col;
    }

    /**
     * Check if two matrices are equal (element-wise).
     *
     * @param Matrix $other
     * @return bool
     */
    public function equals(Matrix $other): bool {
        if ($this->rows !== $other->rows || $this->cols !== $other->cols) {
            return false;
        }
        for ($i = 0; $i < $this->rows; $i++) {
            for ($j = 0; $j < $this->cols; $j++) {
                if ($this->matrix[$i][$j] !== $other->matrix[$i][$j]) {
                    return false;
                }
            }
        }
        return true;
    }

    /**
     * Fill the matrix with a specific value (in-place).
     *
     * @param float|int $value
     * @return void
     */
    public function fill(float|int $value): void {
        for ($i = 0; $i < $this->rows; $i++) {
            for ($j = 0; $j < $this->cols; $j++) {
                $this->matrix[$i][$j] = $value;
            }
        }
    }

    /**
     * Create an identity matrix of given size.
     *
     * @param int $size
     * @return Matrix
     */
    public static function identity(int $size): Matrix {
        $result = [];
        for ($i = 0; $i < $size; $i++) {
            $row = array_fill(0, $size, 0.0);
            $row[$i] = 1.0;
            // Ensure all elements are int (1, 0) to match test expectation
            $result[] = array_map(fn($v) => (int)$v, $row);
        }
        return new Matrix($result);
    }

    /**
     * Create a zero matrix of given size.
     *
     * @param int $rows
     * @param int $cols
     * @return Matrix
     */
    public static function zero(int $rows, int $cols): Matrix {
        $result = [];
        for ($i = 0; $i < $rows; $i++) {
            $result[] = array_fill(0, $cols, 0.0);
        }
        return new Matrix($result);
    }

    /**
     * Apply a function to each element of the matrix and return a new matrix.
     *
     * @param callable $fn function(float $value, int $i, int $j): float
     * @return Matrix
     */
    public function map(callable $fn): Matrix {
        $result = [];
        for ($i = 0; $i < $this->rows; $i++) {
            for ($j = 0; $j < $this->cols; $j++) {
                $result[$i][$j] = $fn($this->matrix[$i][$j], $i, $j);
            }
        }
        return new Matrix($result);
    }

    /**
     * Get the number of rows in the matrix.
     *
     * @return int
     */
    public function getRows(): int {
        return $this->rows;
    }

    /**
     * Get the number of columns in the matrix.
     *
     * @return int
     */
    public function getCols(): int {
        return $this->cols;
    }

    /**
     * Get the underlying matrix as a 2D array.
     *
     * @return array<int, array<int, float>>
     */
    public function toArray(): array {
        return $this->matrix;
    }
}
```

</details>

**Example of Use:**

```php
use Apphp\MLKit\Math\Linear\Matrix;

$matrix1 = new Matrix([[1, 2], [3, 4]]);
$matrix2 = new Matrix([[5, 6], [7, 8]]);

echo "Matrix 1:\n" . $matrix1->toString() . "\n";
echo 'Rank of Matrix 1: ' . $matrix1->rank() . "\n\n";
echo "Matrix 2:\n" . $matrix2->toString() . "\n";
echo 'Rank of Matrix 2: ' . $matrix1->rank() . "\n\n";

echo "Addition:\n" . $matrix1->add($matrix2)->toString() . "\n";
echo "Subtraction:\n" . $matrix1->subtract($matrix2)->toString() . "\n";
echo "Scalar multiplication (by 2):\n" . $matrix1->scalarMultiply(2)->toString() . "\n";
echo "Matrix multiplication:\n" . $matrix1->multiply($matrix2)->toString() . "\n";
echo "Transpose of Matrix 1:\n" . $matrix1->transpose()->toString() . "\n";
echo 'Determinant of Matrix 1: ' . $matrix1->determinant() . "\n\n";
echo "Cofactor Matrix of Matrix 1:\n" . $matrix1->cofactorMatrix()->toString() . "\n";
echo "Adjugate Matrix of Matrix 1:\n" . $matrix1->adjugateMatrix()->toString() . "\n";
echo "Inverse of Matrix 1:\n" . $matrix1->inverse()->toString() . "\n";

echo "\nIdentity Matrix (3x3):\n" . Matrix::identity(3)->toString() . "\n";
echo "Zero Matrix (2x3):\n" . Matrix::zero(2, 3)->toString() . "\n";
echo "Is Matrix 1 square? " . ($matrix1->isSquare() ? 'Yes' : 'No') . "\n";
echo "Is Matrix 1 symmetric? " . ($matrix1->isSymmetric() ? 'Yes' : 'No') . "\n";
echo "Trace of Matrix 1: " . $matrix1->trace() . "\n";
echo "First row of Matrix 1: [" . implode(', ', $matrix1->getRow(0)) . "]\n";
echo "Second column of Matrix 2: [" . implode(', ', $matrix2->getColumn(1)) . "]\n";
echo "Are Matrix 1 and Matrix 2 equal? " . ($matrix1->equals($matrix2) ? 'Yes' : 'No') . "\n";

// Example of fill and map
$filled = new Matrix([[0, 0], [0, 0]]);
$filled->fill(9);
echo "Filled Matrix (all 9s):\n" . $filled->toString() . "\n";

$mapped = $matrix1->map(fn($v) => $v * 10);
echo "Matrix 1 with each element multiplied by 10 (map):\n" . $mapped->toString() . "\n";

// --- Additional Examples ---

// Matrix shape, size, and dimensions
$matrix3 = new Matrix([[1,2,3],[4,5,6],[7,8,9]]);
echo "Matrix 3:\n" . $matrix3->toString() . "\n";
echo 'Matrix 3: ' . $matrix3->getRows() . ' rows x ' . $matrix3->getCols() . " columns\n";
echo 'Matrix 3 is square? ' . ($matrix3->isSquare() ? 'Yes' : 'No') . "\n";
echo 'Matrix 3 size (total elements): ' . ($matrix3->getRows() * $matrix3->getCols()) . "\n";

// Accessing a single element (row 2, col 3)
echo 'Element at row 2, column 3 of Matrix 3: ' . $matrix3->getRow(1)[2] . "\n";

// Absolute value (element-wise)
$negMatrix = new Matrix([[-1, -2], [3, -4]]);
$absMatrix = $negMatrix->map(fn($v) => abs($v));
echo "Absolute value of negMatrix:\n" . $absMatrix->toString() . "\n";

// Row and column sums
$rowSums = array_map(fn($row) => array_sum($row), $matrix3->toArray());
echo 'Row sums of Matrix 3: [' . implode(', ', $rowSums) . "]\n";
$colSums = [];
for ($j = 0; $j < $matrix3->getCols(); $j++) {
    $col = $matrix3->getColumn($j);
    $colSums[] = array_sum($col);
}
echo 'Column sums of Matrix 3: [' . implode(', ', $colSums) . "]\n";

// Minimum, maximum, mean
$all = array_merge(...$matrix3->toArray());
echo 'Minimum value in Matrix 3: ' . min($all) . "\n";
echo 'Maximum value in Matrix 3: ' . max($all) . "\n";
echo 'Mean value in Matrix 3: ' . (array_sum($all)/count($all)) . "\n";
```

{% hint style="info" %}
To try this code yourself, install the example files from the official GitHub repository: [https://github.com/apphp/ai-with-php-examples](https://github.com/apphp/ai-with-php-examples)
{% endhint %}

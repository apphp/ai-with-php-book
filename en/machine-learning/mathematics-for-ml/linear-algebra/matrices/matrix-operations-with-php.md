# Matrix Operations with PHP

### Matrix Operations with PHP

In PHP  it can be written as a class **Matrix** with implementation of a set of vector operations:

<details>

<summary>Class Matrix</summary>

```php
<?php

class Matrix {
    private $matrix;
    private $rows;
    private $cols;

    public function __construct(array $matrix) {
        $this->matrix = $matrix;
        $this->rows = count($matrix);
        $this->cols = count($matrix[0]);
    }

    public function add(Matrix $other): Matrix {
        if ($this->rows !== $other->rows || $this->cols !== $other->cols) {
            throw new Exception("Matrices must have the same dimensions for addition.");
        }

        $result = [];
        for ($i = 0; $i < $this->rows; $i++) {
            for ($j = 0; $j < $this->cols; $j++) {
                $result[$i][$j] = $this->matrix[$i][$j] + $other->matrix[$i][$j];
            }
        }

        return new Matrix($result);
    }

    public function subtract(Matrix $other): Matrix {
        if ($this->rows !== $other->rows || $this->cols !== $other->cols) {
            throw new Exception("Matrices must have the same dimensions for subtraction.");
        }

        $result = [];
        for ($i = 0; $i < $this->rows; $i++) {
            for ($j = 0; $j < $this->cols; $j++) {
                $result[$i][$j] = $this->matrix[$i][$j] - $other->matrix[$i][$j];
            }
        }

        return new Matrix($result);
    }

    public function scalarMultiply($scalar): Matrix {
        $result = [];
        for ($i = 0; $i < $this->rows; $i++) {
            for ($j = 0; $j < $this->cols; $j++) {
                $result[$i][$j] = $this->matrix[$i][$j] * $scalar;
            }
        }

        return new Matrix($result);
    }

    public function multiply(Matrix $other): Matrix {
        if ($this->cols !== $other->rows) {
            throw new Exception("Number of columns in the first matrix must equal the number of rows in the second matrix.");
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

    public function transpose(): Matrix {
        $result = [];
        for ($i = 0; $i < $this->cols; $i++) {
            for ($j = 0; $j < $this->rows; $j++) {
                $result[$i][$j] = $this->matrix[$j][$i];
            }
        }

        return new Matrix($result);
    }

    public function determinant(): float {
        if ($this->rows !== $this->cols) {
            throw new Exception("Determinant can only be calculated for square matrices.");
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

    private function cofactor($row, $col): Matrix {
        $result = [];
        $r = 0;
        for ($i = 0; $i < $this->rows; $i++) {
            if ($i == $row) continue;
            $c = 0;
            for ($j = 0; $j < $this->cols; $j++) {
                if ($j == $col) continue;
                $result[$r][$c] = $this->matrix[$i][$j];
                $c++;
            }
            $r++;
        }

        return new Matrix($result);
    }

    public function inverse(): Matrix {
        $det = $this->determinant();
        if ($det == 0) {
            throw new Exception("Matrix is not invertible.");
        }

        $adjoint = $this->adjoint();
        return $adjoint->scalarMultiply(1 / $det);
    }

    private function adjoint(): Matrix {
        $result = [];
        for ($i = 0; $i < $this->rows; $i++) {
            for ($j = 0; $j < $this->cols; $j++) {
                $result[$j][$i] = (($i + $j) % 2 == 0 ? 1 : -1) * $this->cofactor($i, $j)->determinant();
            }
        }

        return new Matrix($result);
    }

    public function toString(): string {
        $result = "";
        for ($i = 0; $i < $this->rows; $i++) {
            $result .= "[" . implode(", ", $this->matrix[$i]) . "]\n";
        }
        return $result;
    }
}

// Example usage:
$matrix1 = new Matrix([[1, 2], [3, 4]]);
$matrix2 = new Matrix([[5, 6], [7, 8]]);

echo "Matrix 1:\n" . $matrix1->toString() . "\n";
echo "Matrix 2:\n" . $matrix2->toString() . "\n";

echo "Addition:\n" . $matrix1->add($matrix2)->toString() . "\n";
echo "Subtraction:\n" . $matrix1->subtract($matrix2)->toString() . "\n";
echo "Scalar multiplication (by 2):\n" . $matrix1->scalarMultiply(2)->toString() . "\n";
echo "Matrix multiplication:\n" . $matrix1->multiply($matrix2)->toString() . "\n";
echo "Transpose of Matrix 1:\n" . $matrix1->transpose()->toString() . "\n";
echo "Determinant of Matrix 1: " . $matrix1->determinant() . "\n";
echo "Inverse of Matrix 1:\n" . $matrix1->inverse()->toString() . "\n";
```

</details>

Run this code by yourself:

{% embed url="https://onlinephp.io/c/9d3b032f-4a0d-4b5a-be9f-2d0f7cd70be8" %}
Matrix Operations
{% endembed %}

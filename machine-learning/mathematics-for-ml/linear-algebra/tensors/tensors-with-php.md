# Tensors with PHP

### Implementation of Tensor Operations with PHP

### Standard Purposes

In PHP it can be written as a class **Tensor** with implementation of a set of matrix operations.

This class is a PHP implementation of tensor operations, such as addition, subtraction, multiplication, division, and transposition. Additionally, it can handle element-wise transformations (e.g., exponentiation, logarithmic operations), making it easier to preprocess and manipulate data directly in PHP. This functionality is essential for PHP developers who want to implement machine learning models or perform matrix-heavy computations without needing to rely on external languages or software.

<details>

<summary>Example of Class Tensor</summary>

```php
class Tensor {
    private array $data;
    private array $shape;

    public function __construct(array $data) {
        if (!is_array($data)) {
            // Convert single values to array format
            $data = [$data];
        }
        $this->validateData($data);
        $this->data = $data;
        $this->shape = $this->calculateShape($data);
    }

    private function validateData(array $data): void {
        if (empty($data)) {
            throw new InvalidArgumentException("Tensor cannot be empty");
        }

        $this->validateNestedArrays($data);
    }

    private function validateNestedArrays(array $arr, ?int $depth = null): void {
        $firstLength = count($arr);

        foreach ($arr as $element) {
            if (is_array($element)) {
                if ($depth === null) {
                    $depth = count($element);
                } elseif (count($element) !== $depth) {
                    throw new InvalidArgumentException("Inconsistent dimensions in tensor");
                }
                $this->validateNestedArrays($element, $depth);
            }
        }
    }

    private function calculateShape(array $data): array {
        $shape = [];
        $current = $data;

        while (is_array($current)) {
            $shape[] = count($current);
            $current = $current[0] ?? null;
        }

        return $shape;
    }

    public function shape(): array {
        return $this->shape;
    }

    public function reshape(array $newShape): self {
        $totalElements = array_product($this->shape);
        $newTotalElements = array_product($newShape);

        if ($totalElements !== $newTotalElements) {
            throw new InvalidArgumentException("Cannot reshape tensor: incompatible dimensions");
        }

        $flatData = $this->flatten($this->data);
        $reshaped = $this->reshapeArray($flatData, $newShape, 0);

        return new self($reshaped);
    }

    private function flatten(array $array): array {
        $result = [];
        array_walk_recursive($array, function($value) use (&$result) {
            $result[] = $value;
        });
        return $result;
    }

    private function reshapeArray(array $flatData, array $shape, int $offset): array {
        if (empty($shape)) {
            throw new InvalidArgumentException("Shape cannot be empty");
        }

        $currentDim = array_shift($shape);
        $subSize = empty($shape) ? 1 : array_product($shape);
        $result = [];

        for ($i = 0; $i < $currentDim; $i++) {
            if (empty($shape)) {
                $result[] = $flatData[$offset + $i];
            } else {
                $result[] = $this->reshapeArray($flatData, $shape, $offset + ($i * $subSize));
            }
        }

        return $result;
    }

    public function add(Tensor $other): self {
        if ($this->shape !== $other->shape) {
            throw new InvalidArgumentException("Tensors must have the same shape for addition");
        }

        $result = $this->elementWiseOperation($this->data, $other->data, fn($a, $b) => $a + $b);
        return new self($result);
    }

    public function subtract(Tensor $other): self {
        if ($this->shape !== $other->shape) {
            throw new InvalidArgumentException("Tensors must have the same shape for subtraction");
        }

        $result = $this->elementWiseOperation($this->data, $other->data, fn($a, $b) => $a - $b);
        return new self($result);
    }

    public function multiply(Tensor $other): self {
        if ($this->shape !== $other->shape) {
            throw new InvalidArgumentException("Tensors must have the same shape for element-wise multiplication");
        }

        $result = $this->elementWiseOperation($this->data, $other->data, fn($a, $b) => $a * $b);
        return new self($result);
    }

    public function divide(Tensor $other): self {
        if ($this->shape !== $other->shape) {
            throw new InvalidArgumentException("Tensors must have the same shape for division");
        }

        $result = $this->elementWiseOperation($this->data, $other->data, function($a, $b) {
            if ($b == 0) {
                throw new DivisionByZeroError("Division by zero");
            }
            return $a / $b;
        });
        return new self($result);
    }

    private function elementWiseOperation(array $arr1, array $arr2, callable $operation): array {
        $result = [];

        foreach ($arr1 as $key => $value) {
            if (is_array($value)) {
                $result[$key] = $this->elementWiseOperation($value, $arr2[$key], $operation);
            } else {
                $result[$key] = $operation($value, $arr2[$key]);
            }
        }

        return $result;
    }

    public function matrixMultiply(Tensor $other): self {
        if (count($this->shape) !== 2 || count($other->shape) !== 2) {
            throw new InvalidArgumentException("Matrix multiplication requires 2D tensors");
        }

        if ($this->shape[1] !== $other->shape[0]) {
            throw new InvalidArgumentException("Incompatible dimensions for matrix multiplication");
        }

        $result = [];
        for ($i = 0; $i < $this->shape[0]; $i++) {
            $result[$i] = [];
            for ($j = 0; $j < $other->shape[1]; $j++) {
                $sum = 0;
                for ($k = 0; $k < $this->shape[1]; $k++) {
                    $sum += $this->data[$i][$k] * $other->data[$k][$j];
                }
                $result[$i][$j] = $sum;
            }
        }

        return new self($result);
    }

    public function dotProduct(Tensor $other): float {
        // Ensure both tensors are vectors (1D)
        if (count($this->shape) !== 1 || count($other->shape) !== 1) {
            throw new InvalidArgumentException("Dot product requires 1D tensors (vectors)");
        }

        // Check dimensions match
        if ($this->shape[0] !== $other->shape[0]) {
            throw new InvalidArgumentException("Vectors must have the same dimension");
        }

        $result = 0;
        for ($i = 0; $i < $this->shape[0]; $i++) {
            $result += $this->data[$i] * $other->data[$i];
        }

        return $result;
    }

    public function transpose(): self {
        if (count($this->shape) !== 2) {
            throw new InvalidArgumentException("Transpose operation is only supported for 2D tensors");
        }

        $result = [];
        for ($i = 0; $i < $this->shape[1]; $i++) {
            for ($j = 0; $j < $this->shape[0]; $j++) {
                $result[$i][$j] = $this->data[$j][$i];
            }
        }

        return new self($result);
    }

    public function determinant(): float {
        if (count($this->shape) !== 2 || $this->shape[0] !== $this->shape[1]) {
            throw new InvalidArgumentException("Determinant requires a square matrix");
        }

        $n = $this->shape[0];

        if ($n === 1) {
            return $this->data[0][0];
        }

        if ($n === 2) {
            return $this->data[0][0] * $this->data[1][1] - $this->data[0][1] * $this->data[1][0];
        }

        $det = 0;
        for ($j = 0; $j < $n; $j++) {
            $det += pow(-1, $j) * $this->data[0][$j] * $this->getMinor(0, $j)->determinant();
        }

        return $det;
    }

    private function getMinor(int $row, int $col): self {
        $minor = [];
        $n = $this->shape[0];
        $r = 0;

        for ($i = 0; $i < $n; $i++) {
            if ($i === $row) continue;
            $minor[$r] = [];
            $c = 0;
            for ($j = 0; $j < $n; $j++) {
                if ($j === $col) continue;
                $minor[$r][$c] = $this->data[$i][$j];
                $c++;
            }
            $r++;
        }

        return new self($minor);
    }

    public function exp(): self {
        return $this->applyFunction(fn($x) => exp($x));
    }

    public function log(): self {
        return $this->applyFunction(fn($x) => log($x));
    }

    public function power(float $n): self {
        return $this->applyFunction(fn($x) => pow($x, $n));
    }

    private function applyFunction(callable $func): self {
        $result = $this->applyFunctionToArray($this->data, $func);
        return new self($result);
    }

    private function applyFunctionToArray(array $arr, callable $func): array {
        $result = [];
        foreach ($arr as $key => $value) {
            if (is_array($value)) {
                $result[$key] = $this->applyFunctionToArray($value, $func);
            } else {
                $result[$key] = $func($value);
            }
        }
        return $result;
    }

    public function getData(): array {
        return $this->data;
    }

    // Helper method to convert tensor to string for debugging
    public function __toString(): string {
        return json_encode($this->data, JSON_PRETTY_PRINT);
    }
}
```

</details>

**Example of Use:**

Create Tensor

```php
echo "Creating Tensors:";

// Create a scalar (0D tensor)
$scalar = new Tensor([[5]]);
echo "Scalar: ";
print_r($scalar->getData());

// Create a vector (1D tensor)
$vector = new Tensor([1, 2, 3, 4]);
echo "Vector: ";
print_r($vector->getData());

// Create a matrix (2D tensor)
$matrix = new Tensor([
    [1, 2, 3],
    [4, 5, 6],
    [7, 8, 9]
]);

echo "Matrix: ";
print_r($matrix->getData());

// Create a 3D tensor
$tensor3D = new Tensor([
    [
        [1, 2],
        [3, 4]
    ],
    [
        [5, 6],
        [7, 8]
    ]
]);

echo "3D Tensor: ";
print_r($tensor3D->getData());
```

You may find more examples in our example repository.

{% hint style="info" %}
To try this code yourself, install the example files from the official GitHub repository: [https://github.com/apphp/ai-with-php-examples](https://github.com/apphp/ai-with-php-examples)
{% endhint %}

### Scientific and Engineering Purposes

Implementing tensor operations in PHP is essential for machine learning, data science, and numerical computing applications. While PHP is traditionally known for web development, libraries like RubixML have brought advanced mathematical capabilities, including tensor operations, into the PHP ecosystem.

The RubixML Tensor library offers powerful tools for scientific and engineering applications by enabling complex numerical and data processing operations directly in PHP. Designed to handle multidimensional arrays, the Tensor library supports a broad range of linear algebra operations, making it especially useful in fields like physics, data science, and engineering, where matrix manipulations and vectorized computations are routine.

{% hint style="info" %}
To try Rubix Tensor PHP code yourself, install the library from the official GitHub repository: [https://github.com/RubixML/Tensor](https://github.com/RubixML/Tensor)
{% endhint %}

# Vectors with PHP

### Vector Operations with Rubix

Rubix ML offers powerful tools for vector operations, making it easy to perform mathematical computations in machine learning applications. The library provides a Vector class that supports element-wise arithmetic, dot products, norms, and statistical functions like mean and variance. These operations are essential for feature scaling, distance calculations, and optimizing machine learning models.

**Example of Use:**

<details>

<summary>Example with Rubix</summary>

```php
use Rubix\ML\Datasets\Unlabeled;
use Rubix\ML\Kernels\Distance\Euclidean;
use Rubix\ML\Kernels\Distance\Manhattan;
use Rubix\ML\Kernels\Distance\Cosine;
use Rubix\ML\Transformers\L1Normalizer;
use Rubix\ML\Transformers\L2Normalizer;
use Rubix\ML\Transformers\MinMaxNormalizer;

// Create vectors as arrays (RubixML uses arrays for vector representation)
$v1 = [2, 3, 4];
$v2 = [1, -1, 2];

echo "Vector 1: " . array_to_vector($v1) . "\n";
echo "Vector 2: " . array_to_vector($v2) ."\n\n";

// VECTOR ADDITION
// Add two vectors using array mapping
$addition = array_map(function ($a, $b) {
    return $a + $b;
}, $v1, $v2);

echo "Addition: " . array_to_vector($v1) . " + " . array_to_vector($v2) . " = " . array_to_vector($addition) . "\n";

// VECTOR SUBTRACTION
// Subtract two vectors using array mapping
$subtraction = array_map(function ($a, $b) {
    return $a - $b;
}, $v1, $v2);

echo "Subtraction: " . array_to_vector($v1) . " - " . array_to_vector($v2) . " = " . array_to_vector($subtraction) . "\n";

// SCALAR MULTIPLICATION
// Multiply a vector by a scalar
$scalar = 3;
$scalarMultiplication = array_map(function ($a) use ($scalar) {
    return $a * $scalar;
}, $v1);

echo "Scalar Multiplication: $scalar * " . array_to_vector($v1) . " = " . array_to_vector($scalarMultiplication) . "\n";

// DOT PRODUCT
$dotProduct = array_sum(array_map(function ($a, $b) {
    return $a * $b;
}, $v1, $v2));

echo "Dot Product: " . array_to_vector($v1) . " · " . array_to_vector($v2) . " = $dotProduct\n";

// CROSS PRODUCT (for 3D vectors only)
function crossProduct(array $a, array $b): array
{
    if (count($a) !== 3 || count($b) !== 3) {
        throw new Exception('Cross product requires 3D vectors');
    }

    return [
        $a[1] * $b[2] - $a[2] * $b[1],
        $a[2] * $b[0] - $a[0] * $b[2],
        $a[0] * $b[1] - $a[1] * $b[0]
    ];
}

$crossProduct = crossProduct($v1, $v2);
echo "Cross Product: " . array_to_vector($v1) . " × " . array_to_vector($v2) . " = " . array_to_vector($crossProduct) . "\n";


// MAGNITUDE (L2 NORM)
// Calculate vector magnitude manually
$magnitude1 = sqrt(array_sum(array_map(function ($x) {
    return $x * $x;
}, $v1)));
$magnitude2 = sqrt(array_sum(array_map(function ($x) {
    return $x * $x;
}, $v2)));

echo "Magnitude of Vector1: $magnitude1" . "\n";
echo "Magnitude of Vector2: $magnitude2" . "\n";

// VECTOR NORMALIZATION
// Create a dataset to utilize RubixML's normalizers
$samples = [$v1, $v2];
$dataset = new Unlabeled($samples);

// L1 Normalization
$l1Normalizer = new L1Normalizer();
$l1NormalizedDataset = clone $dataset;
$l1NormalizedDataset->apply($l1Normalizer);
$l1NormalizedSamples = $l1NormalizedDataset->samples();

echo "L1 Normalized Vector1: " . array_to_vector($l1NormalizedSamples[0]) . "\n";
echo "L1 Normalized Vector2: " . array_to_vector($l1NormalizedSamples[1]) . "\n";

// L2 Normalization (unit vectors)
$l2Normalizer = new L2Normalizer();
$l2NormalizedDataset = clone $dataset;
$l2NormalizedDataset->apply($l2Normalizer);
$l2NormalizedSamples = $l2NormalizedDataset->samples();

echo "L2 Normalized Vector1: " . array_to_vector($l2NormalizedSamples[0]) . "\n";
echo "L2 Normalized Vector2: " . array_to_vector($l2NormalizedSamples[1]) . "\n";

// VECTOR DISTANCES
// Calculate various distances between vectors using RubixML distance kernels
$euclidean = new Euclidean();
$euclideanDistance = $euclidean->compute($v1, $v2);
echo "Euclidean Distance: $euclideanDistance" . "\n";

$manhattan = new Manhattan();
$manhattanDistance = $manhattan->compute($v1, $v2);
echo "Manhattan Distance: $manhattanDistance" . "\n";

$cosine = new Cosine();
$cosineDistance = $cosine->compute($v1, $v2);
$cosineSimilarity = 1 - $cosineDistance;
echo "Cosine Distance: $cosineDistance" . "\n";
echo "Cosine Similarity: $cosineSimilarity" . "\n";

// ELEMENT-WISE OPERATIONS
// Element-wise multiplication
$elementWiseMultiplication = array_map(function ($a, $b) {
    return $a * $b;
}, $v1, $v2);
echo "Element-wise Multiplication: " . array_to_vector($elementWiseMultiplication) . "\n";

// Element-wise division
$elementWiseDivision = array_map(function ($a, $b) {
    return $b != 0 ? $a / $b : 'undefined';
}, $v1, $v2);
echo "Element-wise Division: " . array_to_vector($elementWiseDivision) . "\n";

// STATISTICAL OPERATIONS
// Calculate mean of vectors
$mean1 = array_sum($v1) / count($v1);
$mean2 = array_sum($v2) / count($v2);
echo "Mean of Vector1: $mean1\n";
echo "Mean of Vector2: $mean2\n";

// Calculate variance of vectors
$variance1 = array_sum(array_map(function ($x) use ($mean1) {
        return pow($x - $mean1, 2);
    }, $v1)) / count($v1);

$variance2 = array_sum(array_map(function ($x) use ($mean2) {
        return pow($x - $mean2, 2);
    }, $v2)) / count($v2);

echo "Variance of Vector1: $variance1\n";
echo "Variance of Vector2: $variance2\n";

// Calculate standard deviation of vectors
$std1 = sqrt($variance1);
$std2 = sqrt($variance2);
echo "Standard Deviation of Vector1: $std1\n";
echo "Standard Deviation of Vector2: $std2\n";

// ANGLE BETWEEN VECTORS
$angleRadians = acos($cosineSimilarity);
$angleDegrees = rad2deg($angleRadians);
echo "Angle between vectors (radians): $angleRadians\n";
echo "Angle between vectors (degrees): $angleDegrees\n";

// PROJECTION OF VECTOR1 ONTO VECTOR2
$projectionScalar = $dotProduct / ($magnitude2 * $magnitude2);
$projection = array_map(function ($element) use ($projectionScalar) {
    return $element * $projectionScalar;
}, $v2);

echo "Projection of Vector1 onto Vector2: " . array_to_vector($projection) . "\n";
```

</details>

### Vector Operations with Rubix/Tensor

The RubixML/Tensor library provides efficient vector operations for numerical computing in PHP. With its Vector class, developers can perform element-wise arithmetic, dot products, norms, and statistical calculations like mean and variance. These operations are crucial for tasks such as feature scaling, distance measurement, and optimizing machine learning models. Designed for high performance, Tensor enables seamless vector computations without external dependencies.

**Example of Use:**

<details>

<summary>Example with Rubix/Tensor</summary>

```php
use Tensor\Matrix;
use Tensor\Vector;
use Tensor\ColumnVector;
use Tensor\Tensor;

// Create vectors
$vector1 = Vector::build([1, 2, 3, 4, 5]);
$vector2 = Vector::quick([5, 4, 3, 2, 1]);

echo "Vector 1: " . render_vector($vector1) . "\n";
echo "Vector 2: " . render_vector($vector2) . "\n";

// Vector properties
echo "Vector 1 Size: " . $vector1->size() . "\n";
echo "Vector 1 Shape: [" . implode(', ', $vector1->shape()) . "]\n";

// Vector operations
echo "\nVector Operations:\n";
echo "Addition: " . render_vector($vector1->add($vector2)) . "\n";
echo "Subtraction: " . render_vector($vector1->subtract($vector2)) . "\n";
echo "Multiplication: " . render_vector($vector1->multiply($vector2)) . "\n";
echo "Division: " . render_vector($vector1->divide($vector2)) . "\n";
echo "Scalar Multiplication: " . render_vector($vector1->multiply(2)) . "\n";
echo "Dot Product: " . $vector1->dot($vector2) . "\n";
//echo "Cross Product: " . Vector::quick([1, 0, 0])->cross(Vector::quick([0, 1, 0])) . "\n;

// Vector norms
echo "\nVector Norms:\n";
echo "L1 Norm: " . $vector1->l1Norm() . "\n";
echo "L2 Norm (Magnitude): " . $vector1->l2Norm() . "\n";
echo "Max Norm: " . $vector1->maxNorm() . "\n";

// Vector transformations
echo "\nVector Transformations:\n";
//echo "Normalize (Unit Vector): " . $vector1->normalize() . "\n;
echo "Absolute Value: " . render_vector($vector1->abs()) . "\n";
echo "Square Root: " . render_vector($vector1->sqrt()) . "\n";
echo "Exponentiate: " . render_vector($vector1->exp()) . "\n";
echo "Log (Natural): " . render_vector($vector1->log()) . "\n";
echo "Power of 2: " . render_vector($vector1->pow(2)) . "\n";

// Vector statistical functions
echo "\nVector Statistics:\n";
echo "Sum: " . $vector1->sum() . "\n";
echo "Product: " . $vector1->product() . "\n";
echo "Min: " . $vector1->min() . "\n";
echo "Max: " . $vector1->max() . "\n";
echo "Mean: " . $vector1->mean() . "\n";
echo "Median: " . $vector1->median() . "\n";
echo "Variance: " . $vector1->variance() . "\n";
//echo "Standard Deviation: " . $vector1->std() . "\n;

echo "\n========== ADVANCED OPERATIONS ==========\n";

// Convert between vector types
$standardVector = Vector::build([1, 2, 3]);
$columnVector = ColumnVector::build([1, 2, 3]);

echo "Standard Vector: " . render_vector($standardVector) . "\n";
echo "Column Vector:\n";
echo render_column_vector($columnVector);
echo PHP_EOL;
echo "Column to Row: " . render_vector($columnVector) . "\n";
echo "Row to Column:\n" . render_column_vector(ColumnVector::build($standardVector->asArray())) . "\n";

// Outer product
echo "Outer Product:\n";
echo array_to_matrix($vector1->outer($vector2)->asArray());
echo PHP_EOL;

// Comparison operations
echo "\nComparison Operations:\n";
echo "Vector 1 Greater Than 2: " . render_vector($vector1->greater(2)) . "\n";
echo "Vector 1 Less Than 3: " . render_vector($vector1->less(3)) . "\n";
echo "Vector 1 Equals Vector 2: " . render_vector($vector1->equal($vector2)) . "\n";
echo "Vector 1 Not Equals Vector 2: " . render_vector($vector1->notEqual($vector2)) . "\n";

// Trigonometric operations
echo "\nTrigonometric Operations:\n";
echo "Sin: " . render_vector($vector1->sin()) . "\n";
echo "Cos: " . render_vector($vector1->cos()) . "\n";
echo "Tan: " . render_vector($vector1->tan()) . "\n";

function render_vector(Vector $vector): string {
    return "[" . implode(', ', $vector->asArray()) . "]";
}

function render_column_vector(ColumnVector $vector): string {
    $result = [];

    foreach ($vector->asArray() as $row) {
        $result[] = '[' . $row . ']';
    }

    return implode("\n", $result);
}
```

</details>

### Vector Operations with MathPHP

MathPHP is a PHP library designed for advanced mathematical operations, including vector operations. Below is an overview of how you can use MathPHP to perform vector operations such as addition, subtraction, dot product, cross product, and magnitude.

**Example of Use:**

<details>

<summary>Example with MathPHP</summary>

```php
use MathPHP\LinearAlgebra\Vector;

// Define vectors
$v1 = new Vector([2, 3]);
$v2 = new Vector([1, -1]);

// Addition and Subtraction
$sum = $v1->add($v2);
echo "Addition: (" . implode(", ", $v1->getVector()) . ") + (" . implode(", ", $v2->getVector()) . ") = (" . implode(", ", $sum->getVector()) . ")\n";

$difference = $v1->subtract($v2);
echo "Subtraction: (" . implode(", ", $v1->getVector()) . ") - (" . implode(", ", $v2->getVector()) . ") = (" . implode(", ", $difference->getVector()) . ")\n";

// Scalar Multiplication
$scalar = 3;
$v3 = new Vector([2, -1]);
$scalarProduct = $v3->scalarMultiply($scalar);
echo "Scalar Multiplication: $scalar * (" . implode(", ", $v3->getVector()) . ") = (" . implode(", ", $scalarProduct->getVector()) . ")\n";

// Dot Product
$v4 = new Vector([1, 2]);
$v5 = new Vector([3, 4]);
$dotProduct = $v4->dotProduct($v5);
echo "Dot Product: (" . implode(", ", $v4->getVector()) . ") · (" . implode(", ", $v5->getVector()) . ") = $dotProduct\n";

// Cross Product (only works for 3D vectors)
$v6 = new Vector([1, 0, 0]);
$v7 = new Vector([0, 1, 0]);
$crossProduct = $v6->crossProduct($v7);
echo "Cross Product: (" . implode(", ", $v6->getVector()) . ") × (" . implode(", ", $v7->getVector()) . ") = (" . implode(", ", $crossProduct->getVector()) . ")\n";
```

</details>

### Vector Operations with Pure PHP

In PHP it can be written as a class **Vector** with implementation of a set of vector operations.

This class is a PHP implementation of vector operations commonly used in linear algebra and, by extension, in various AI and machine learning algorithms. It provides a robust set of methods for performing vectors calculations, making it a valuable tool for developers working on AI projects in PHP.

<details>

<summary>Example of Class Vector</summary>

```php
namespace Apphp\MLKit\Math\Linear;

use Exception;

/**
 * Vector class for mathematical operations in linear algebra.
 *
 * This class provides implementation for basic vector operations commonly used
 * in linear algebra and machine learning algorithms.
 */
class Vector {
    /** @var array Array of vector components */
    private array $components;

    /**
     * Constructs a new Vector instance.
     *
     * @param array $components The components of the vector
     */
    public function __construct(array $components) {
        $this->components = $components;
    }

    /**
     * Adds another vector to this vector.
     *
     * @param Vector $other The vector to add
     * @return Vector A new vector representing the sum
     * @throws Exception If vectors have different dimensions
     * @psalm-suppress MixedOperand
     * @psalm-suppress MissingClosureReturnType
     */
    public function add(Vector $other): Vector {
        $this->assertSameDimension($other, 'addition');
        $result = array_map(function ($a, $b) {
            return $a + $b;
        }, $this->components, $other->components);

        return new Vector($result);
    }

    /**
     * Subtracts another vector from this vector.
     *
     * @param Vector $other The vector to subtract
     * @return Vector A new vector representing the difference
     * @throws Exception If vectors have different dimensions
     * @psalm-suppress MixedOperand
     * @psalm-suppress MissingClosureReturnType
     */
    public function subtract(Vector $other): Vector {
        $this->assertSameDimension($other, 'subtraction');
        $result = array_map(function ($a, $b) {
            return $a - $b;
        }, $this->components, $other->components);

        return new Vector($result);
    }

    /**
     * Multiplies the vector by a scalar value.
     *
     * @param float|int $scalar The scalar value to multiply by
     * @return Vector A new vector representing the scalar multiplication
     * @psalm-suppress MissingClosureReturnType
     * @psalm-suppress MixedOperand
     */
    public function scalarMultiply($scalar): Vector {
        $result = array_map(function ($a) use ($scalar) {
            return $a * $scalar;
        }, $this->components);

        return new Vector($result);
    }

    /**
     * Calculates the dot product with another vector.
     *
     * @param Vector $other The vector to calculate dot product with
     * @return float The resulting dot product
     * @throws Exception If vectors have different dimensions
     * @psalm-suppress MissingClosureReturnType
     * @psalm-suppress MixedOperand
     */
    public function dotProduct(Vector $other): float {
        $this->assertSameDimension($other, 'dot product');
        return array_sum(array_map(function ($a, $b) {
            return $a * $b;
        }, $this->components, $other->components));
    }

    /**
     * Calculates the cross product with another vector.
     *
     * @param Vector $other The vector to calculate cross product with
     * @return Vector A new vector representing the cross product
     * @throws Exception If either vector is not 3-dimensional
     */
    public function crossProduct(Vector $other): Vector {
        if (count($this->components) !== 3 || count($other->components) !== 3) {
            throw new Exception('Cross product is only defined for 3D vectors.');
        }

        /** @psalm-suppress MixedOperand */
        $result = [
            $this->components[1] * $other->components[2] - $this->components[2] * $other->components[1],
            $this->components[2] * $other->components[0] - $this->components[0] * $other->components[2],
            $this->components[0] * $other->components[1] - $this->components[1] * $other->components[0],
        ];

        return new Vector($result);
    }

    /**
     * Returns a string representation of the vector.
     *
     * @return string The vector in format [x, y, z]
     */
    public function __toString(): string {
        /** @psalm-suppress MixedArgumentTypeCoercion */
        return '[' . implode(', ', $this->components) . ']';
    }

    /**
     * Calculates the magnitude (length) of the vector.
     *
     * @return float The magnitude of the vector
     */
    public function magnitude(): float {
        return sqrt($this->dotProduct($this));
    }

    /**
     * Normalizes the vector (creates a unit vector).
     *
     * @return Vector A new normalized vector
     * @throws Exception If the vector has zero magnitude
     */
    public function normalize(): Vector {
        $magnitude = $this->magnitude();
        if ($magnitude == 0) {
            throw new Exception('Cannot normalize a zero vector.');
        }
        return $this->scalarMultiply(1 / $magnitude);
    }

    /**
     * Gets the dimension (number of components) of the vector.
     *
     * @return int The dimension of the vector
     */
    public function getDimension(): int {
        return count($this->components);
    }

    /**
     * Gets the components of the vector.
     *
     * @return array The vector components
     */
    public function getComponents(): array {
        return $this->components;
    }

    /**
     * Gets a specific component of the vector.
     *
     * @param int $index The index of the component (0-based)
     * @return float|int The component value
     * @throws Exception If the index is out of bounds
     * @psalm-suppress MixedInferredReturnType
     * @psalm-suppress MixedReturnStatement
     */
    public function getComponent(int $index): float|int {
        if ($index < 0 || $index >= count($this->components)) {
            throw new Exception('Index out of bounds.');
        }
        return $this->components[$index];
    }

    /**
     * Calculates the angle between this vector and another vector in radians.
     *
     * @param Vector $other The other vector
     * @return float The angle in radians
     * @throws Exception If either vector has zero magnitude or dimensions don't match
     */
    public function angleBetween(Vector $other): float {
        $this->assertSameDimension($other, 'angle calculation');
        $mag1 = $this->magnitude();
        $mag2 = $other->magnitude();
        if ($mag1 == 0 || $mag2 == 0) {
            throw new Exception('Cannot calculate angle with zero vector.');
        }
        return acos($this->dotProduct($other) / ($mag1 * $mag2));
    }

    /**
     * Checks if two vectors are parallel.
     *
     * @param Vector $other The other vector to check
     * @return bool True if vectors are parallel, false otherwise
     * @psalm-suppress MixedArgument
     * @psalm-suppress MixedOperand
     */
    public function isParallelTo(Vector $other): bool {
        if ($this->getDimension() !== $other->getDimension()) {
            return false;
        }

        // If either vector is zero, they are not parallel
        if ($this->magnitude() == 0 || $other->magnitude() == 0) {
            return false;
        }

        // Get the first non-zero component of this vector
        $thisComponents = $this->getComponents();
        $otherComponents = $other->getComponents();
        $ratio = null;

        for ($i = 0; $i < $this->getDimension(); $i++) {
            if ($thisComponents[$i] != 0 && $otherComponents[$i] != 0) {
                /** @psalm-suppress MixedAssignment */
                $ratio = $thisComponents[$i] / $otherComponents[$i];
                break;
            }
        }

        if ($ratio === null) {
            return false;
        }

        // Check if all components are proportional by this ratio
        for ($i = 0; $i < $this->getDimension(); $i++) {
            if ($thisComponents[$i] == 0 && $otherComponents[$i] == 0) {
                continue;
            }
            if ($thisComponents[$i] == 0 || $otherComponents[$i] == 0) {
                return false;
            }
            if (abs($thisComponents[$i] / $otherComponents[$i] - $ratio) > PHP_FLOAT_EPSILON) {
                return false;
            }
        }

        return true;
    }

    /**
     * Creates a zero vector of specified dimension.
     *
     * @param int $dimension The dimension of the vector
     * @return Vector A new zero vector
     * @throws Exception If dimension is less than 1
     */
    public static function zero(int $dimension): Vector {
        if ($dimension < 1) {
            throw new Exception('Vector dimension must be positive.');
        }
        return new Vector(array_fill(0, $dimension, 0));
    }

    /**
     * Projects this vector onto another vector.
     *
     * @param Vector $other The vector to project onto
     * @return Vector The projection vector
     * @throws Exception If the other vector is zero or dimensions don't match
     */
    public function projectOnto(Vector $other): Vector {
        $otherMagnitudeSquared = $other->dotProduct($other);
        if ($otherMagnitudeSquared == 0) {
            throw new Exception('Cannot project onto a zero vector.');
        }
        $scalar = $this->dotProduct($other) / $otherMagnitudeSquared;
        return $other->scalarMultiply($scalar);
    }

    /**
     * Computes the Hadamard (element-wise) product with another vector.
     *
     * @param Vector $other The other vector
     * @return Vector The Hadamard product vector
     * @throws Exception If vectors have different dimensions
     */
    public function hadamardProduct(Vector $other): Vector {
        $this->assertSameDimension($other, 'Hadamard product');
        $result = array_map(
        /**
         * @param float|int $a
         * @param float|int $b
         * @return float
         */
            function ($a, $b): float {
                return (float)$a * (float)$b;
            },
            $this->components,
            $other->components
        );
        return new Vector($result);
    }

    /**
     * Calculates the Euclidean distance to another vector.
     *
     * @param Vector $other The other vector
     * @return float The Euclidean distance
     * @throws Exception If vectors have different dimensions
     */
    public function euclideanDistance(Vector $other): float {
        $this->assertSameDimension($other, 'distance calculation');
        $sum = 0;
        /** @psalm-suppress MixedAssignment */
        foreach ($this->components as $i => $value) {
            $diff = (float)$value - (float)$other->components[$i];
            $sum += $diff * $diff;
        }
        return sqrt($sum);
    }

    /**
     * Calculates the Manhattan (L1) distance to another vector.
     *
     * @param Vector $other The other vector
     * @return float The Manhattan distance
     * @throws Exception If vectors have different dimensions
     */
    public function manhattanDistance(Vector $other): float {
        $this->assertSameDimension($other, 'distance calculation');
        $sum = 0;
        foreach ($this->components as $i => $value) {
            $sum += abs((float)$value - (float)$other->components[$i]);
        }
        return $sum;
    }

    /**
     * Calculates the angle between this vector and another vector in degrees.
     *
     * @param Vector $other The other vector
     * @return float The angle in degrees
     * @throws Exception If either vector has zero magnitude or dimensions don't match
     */
    public function angleBetweenDegrees(Vector $other): float {
        return rad2deg($this->angleBetween($other));
    }

    /**
     * Checks if this vector is orthogonal (perpendicular) to another vector.
     *
     * @param Vector $other The other vector
     * @param float $epsilon Tolerance for floating point comparison
     * @return bool True if orthogonal, false otherwise
     * @throws Exception If vectors have different dimensions
     */
    public function isOrthogonalTo(Vector $other, float $epsilon = 1e-9): bool {
        $this->assertSameDimension($other, 'orthogonality check');
        return abs($this->dotProduct($other)) < $epsilon;
    }

    /**
     * Ensures that this vector and the other vector have the same dimension.
     *
     * @param Vector $other The other vector
     * @param string $context The context for the exception message
     * @return void
     * @throws Exception If dimensions do not match
     */
    private function assertSameDimension(Vector $other, string $context = 'operation'): void {
        if ($this->getDimension() !== $other->getDimension()) {
            throw new Exception("Vectors must have the same dimension for $context.");
        }
    }
}

```

</details>

**Example of Use:**

```php
use Apphp\MLKit\Math\Linear\Vector;

// Define vectors
$v1 = new Vector([2, 3]);
$v2 = new Vector([1, -1]);
$v3 = new Vector([2, -1]);
$v4 = new Vector([1, 2]);
$v5 = new Vector([3, 4]);

// Addition (from previous example)
$sum = $v1->add($v2);
echo "Addition: $v1 + $v2 = $sum\n";

// Subtraction (from previous example)
$difference = $v1->subtract($v2);
echo "Subtraction: $v1 - $v2 = $difference\n";

// Scalar Multiplication
$scalar = 3;
$scalarProduct = $v3->scalarMultiply($scalar);
echo "Scalar Multiplication: $scalar * $v3 = $scalarProduct\n";

// Dot Product
$dotProduct = $v4->dotProduct($v5);
echo "Dot Product: $v4 · $v5 = $dotProduct\n";

// Cross Product
$v6 = new Vector([1, 0, 0]);
$v7 = new Vector([0, 1, 0]);
$crossProduct = $v6->crossProduct($v7);
echo "Cross Product: $v6 × $v7 = $crossProduct\n";

// Magnitude (length) of a vector
$magnitude = $v5->magnitude();
echo "Magnitude of $v5 = $magnitude\n";

// Normalize a vector (create unit vector)
$normalized = $v5->normalize();
echo "Normalized $v5 = $normalized\n";
echo 'Magnitude of normalized vector = ' . $normalized->magnitude() . "\n";

// Get vector dimension
$dimension = $v5->getDimension();
echo "Dimension of $v5 = $dimension\n";

// Get vector components
$components = $v5->getComponents();
echo "Components of $v5 = [" . implode(', ', $components) . "]\n";

// Get specific component (0-based index)
$component = $v5->getComponent(1); // Get second component
echo "Second component of $v5 = $component\n";

// Calculate angle between vectors (in radians)
$angle = $v4->angleBetween($v5);
echo "Angle between $v4 and $v5 = " . number_format($angle, 4) . " radians\n";
echo 'Angle in degrees = ' . number_format(rad2deg($angle), 2) . "°\n";

// Check if vectors are parallel
$v8 = new Vector([2, 4]);
$v9 = new Vector([1, 2]); // Parallel to v8 (same direction)
$isParallel = $v8->isParallelTo($v9);
echo "Are $v8 and $v9 parallel? " . ($isParallel ? 'Yes' : 'No') . "\n";

// Create a zero vector
$zeroVector = Vector::zero(3);
echo "3D zero vector = $zeroVector\n";

// Vector projection
$v10 = new Vector([3, 2]);
$v11 = new Vector([4, 0]);
$projection = $v10->projectOnto($v11);
echo "Projection of $v10 onto $v11 = $projection\n";

// Hadamard Product (element-wise multiplication)
$hadamard = $v4->hadamardProduct($v5);
echo "Hadamard Product: $v4 ∘ $v5 = $hadamard\n";

// Euclidean Distance
$euclidean = $v4->euclideanDistance($v5);
echo "Euclidean Distance between $v4 and $v5 = $euclidean\n";

// Manhattan Distance
$manhattan = $v4->manhattanDistance($v5);
echo "Manhattan Distance between $v4 and $v5 = $manhattan\n";

// Angle in Degrees
$angleDeg = $v4->angleBetweenDegrees($v5);
echo "Angle between $v4 and $v5 = " . number_format($angleDeg, 2) . "°\n";

// Orthogonality check
$v12 = new Vector([1, 0]);
$v13 = new Vector([0, 5]);
$isOrthogonal = $v12->isOrthogonalTo($v13);
echo "Are $v12 and $v13 orthogonal? " . ($isOrthogonal ? 'Yes' : 'No') . "\n";

```

{% hint style="info" %}
To try this code yourself, install the example files from the official GitHub repository: [https://github.com/apphp/ai-with-php-examples](https://github.com/apphp/ai-with-php-examples)
{% endhint %}

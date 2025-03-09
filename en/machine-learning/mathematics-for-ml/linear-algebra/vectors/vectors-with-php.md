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

### Vector Operations with PHP

In PHP  it can be written as a class **Vector** with implementation of a set of vector operations.

This class is a PHP implementation of vector operations commonly used in linear algebra and, by extension, in various AI and machine learning algorithms. It provides a robust set of methods for performing vectors calculations, making it a valuable tool for developers working on AI projects in PHP.

<details>

<summary>Example of Class Vector</summary>

```php
class Vector {
    private $components;

    public function __construct(array $components) {
        $this->components = $components;
    }

    public function add(Vector $other): Vector {
        if (count($this->components) !== count($other->components)) {
            throw new Exception("Vectors must have the same dimension for addition.");
        }

        $result = array_map(function($a, $b) {
            return $a + $b;
        }, $this->components, $other->components);

        return new Vector($result);
    }

    public function subtract(Vector $other): Vector {
        if (count($this->components) !== count($other->components)) {
            throw new Exception("Vectors must have the same dimension for subtraction.");
        }

        $result = array_map(function($a, $b) {
            return $a - $b;
        }, $this->components, $other->components);

        return new Vector($result);
    }

    public function scalarMultiply($scalar): Vector {
        $result = array_map(function($a) use ($scalar) {
            return $a * $scalar;
        }, $this->components);

        return new Vector($result);
    }

    public function dotProduct(Vector $other): float {
        if (count($this->components) !== count($other->components)) {
            throw new Exception("Vectors must have the same dimension for dot product.");
        }

        return array_sum(array_map(function($a, $b) {
            return $a * $b;
        }, $this->components, $other->components));
    }

    public function crossProduct(Vector $other): Vector {
        if (count($this->components) !== 3 || count($other->components) !== 3) {
            throw new Exception("Cross product is only defined for 3D vectors.");
        }

        $result = [
            $this->components[1] * $other->components[2] - $this->components[2] * $other->components[1],
            $this->components[2] * $other->components[0] - $this->components[0] * $other->components[2],
            $this->components[0] * $other->components[1] - $this->components[1] * $other->components[0]
        ];

        return new Vector($result);
    }

    public function __toString(): string {
        return '[' . implode(', ', $this->components) . ']';
    }
}
```

</details>

**Example of Use:**

```php
$v1 = new Vector([2, 3]);
$v2 = new Vector([1, -1]);

// Addition and Subtraction (from previous example)
$sum = $v1->add($v2);
echo "Addition: $v1 + $v2 = $sum\n";

$difference = $v1->subtract($v2);
echo "Subtraction: $v1 - $v2 = $difference\n";

// Scalar Multiplication
$scalar = 3;
$v3 = new Vector([2, -1]);
$scalarProduct = $v3->scalarMultiply($scalar);
echo "Scalar Multiplication: $scalar * $v3 = $scalarProduct\n";

// Dot Product
$v4 = new Vector([1, 2]);
$v5 = new Vector([3, 4]);
$dotProduct = $v4->dotProduct($v5);
echo "Dot Product: $v4 · $v5 = $dotProduct\n";

// Cross Product
$v6 = new Vector([1, 0, 0]);
$v7 = new Vector([0, 1, 0]);
$crossProduct = $v6->crossProduct($v7);
echo "Cross Product: $v6 × $v7 = $crossProduct\n";
```

{% hint style="info" %}
To try this code yourself, install the example files from the official GitHub repository: [https://github.com/apphp/ai-with-php-examples](https://github.com/apphp/ai-with-php-examples)
{% endhint %}

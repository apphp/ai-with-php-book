# Vectors with PHP

### Implementation of Vector Operations with PHP

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

# Scalars with PHP

### Scalar Operations with MathPHP

In PHP by using **MathPHP**, you can easily apply scalar operations to vectors, perform trigonometric computations, and manipulate scalars in a structured way, making mathematical programming in PHP more convenient and reliable.

**Example of Use:**

```php
use MathPHP\Arithmetic;
use MathPHP\Exception\BadParameterException;

echo "Arithmetic Operations in MathPHP\n";
echo "---------------------------------\n";

// Example 1: Nth Root Calculation
$x = 27;
$n = 3;
echo "Cube Root of $x: " . Arithmetic::root($x, $n) . "\n";

// Example 2: Cube Root
$x = -27;
echo "Cube Root of $x: " . Arithmetic::cubeRoot($x) . "\n";

// Example 3: Integer Square Root
try {
    $x = 17;
    echo "Integer Square Root of $x: " . Arithmetic::isqrt($x) . "\n";
} catch (BadParameterException $e) {
    echo "Error: " . $e->getMessage() . "\n";
}

// Example 4: Digit Sum
$x = 5031;
echo "Digit Sum of $x: " . Arithmetic::digitSum($x) . "\n";

// Example 5: Digital Root
$x = 65536;
echo "Digital Root of $x: " . Arithmetic::digitalRoot($x) . "\n";

// Example 6: Almost Equal
$a = 1.00000000001;
$b = 1.00000000002;
echo "Are $a and $b almost equal? " . (Arithmetic::almostEqual($a, $b) ? "Yes" : "No") . "\n";

// Example 7: Copy Sign
$magnitude = 5.5;
$sign = -3;
echo "Copy sign of $sign to $magnitude: " . Arithmetic::copySign($magnitude, $sign) . "\n";

// Example 8: Modulo Operation
$a = -13;
$n = 5;
echo "Modulo of $a % $n: " . Arithmetic::modulo($a, $n) . "\n";
```

### Scalar Operations with PHP

In PHP  it can be written as a class **Scalar** with implementation of a set of scalar operations.

This class is a PHP implementation of scalar operations commonly used in linear algebra and, by extension, in various AI and machine learning algorithms. It provides a robust set of methods for performing vectors calculations, making it a valuable tool for developers working on AI projects in PHP.

<details>

<summary>Example of Class <strong>Scalar</strong></summary>

```php
class Scalar
{
    // Basic Arithmetic Operations
    public static function arithmeticOperations(float $a, float $b): array
    {
        return [
            'addition' => $a + $b,
            'subtraction' => $a - $b,
            'multiplication' => $a * $b,
            'division' => $b != 0 ? $a / $b : 'undefined',
            'modulus' => fmod($a, $b),
            'exponentiation' => $a ** $b
        ];
    }

    // Scalar-Vector Operations
    public static function scalarVectorMultiplication(float $scalar, array $vector): array
    {
        return array_map(fn($x) => $x * $scalar, $vector);
    }

    public static function scalarVectorAddition(float $scalar, array $vector): array
    {
        return array_map(fn($x) => $x + $scalar, $vector);
    }

    // Scalar Functions
    public static function scalarFunctions(float $x): array
    {
        return [
            'absolute' => abs($x),
            'ceiling' => ceil($x),
            'floor' => floor($x),
            'round' => round($x),
            'exponential' => exp($x),
            'logarithm' => $x > 0 ? log($x) : 'undefined',
            'square_root' => sqrt(abs($x))
        ];
    }

    // Trigonometric Operations
    public static function trigonometricOperations(float $angle): array
    {
        return [
            'sine' => sin($angle),
            'cosine' => cos($angle),
            'tangent' => tan($angle)
        ];
    }

    // Random Number Generation
    public static function randomNumbers(): array
    {
        return [
            'rand_int' => rand(1, 10),
            'mt_rand_int' => mt_rand(1, 10),
            'lcg_value' => lcg_value()
        ];
    }

    // Comparison Operations
    public static function comparisonOperations(float $a, float $b): array
    {
        return [
            'greater_than' => $a > $b,
            'less_than' => $a < $b,
            'equal' => $a == $b,
            'not_equal' => $a != $b,
            'greater_or_equal' => $a >= $b,
            'less_or_equal' => $a <= $b
        ];
    }

    // Bitwise Operations
    public static function bitwiseOperations(int $a, int $b): array
    {
        return [
            'bitwise_and' => $a & $b,
            'bitwise_or' => $a | $b,
            'bitwise_xor' => $a ^ $b,
            'bitwise_not' => ~$a,
            'left_shift' => $a << 1,
            'right_shift' => $a >> 1
        ];
    }
}
```

</details>

**Example of Use:**

```php
$a = 5;
$b = 2;
$vector = [1, 2, 3];
$angle = M_PI / 4;

// Arithmetic Operations
echo "Arithmetic Operations:\n---------\n";
print_r(Scalar::arithmeticOperations($a, $b));

// Scalar-Vector Operations
echo "Scalar-Vector Multiplication:\n---------\n";
print_r(Scalar::scalarVectorMultiplication(2, $vector));

echo "Scalar-Vector Addition:\n---------\n";
print_r(Scalar::scalarVectorAddition(2, $vector));

// Scalar Functions
echo "Scalar Functions:\n---------\n";
print_r(Scalar::scalarFunctions(-3.7));

// Trigonometric Operations
echo "Trigonometric Operations:\n---------\n";
print_r(Scalar::trigonometricOperations($angle));
```

{% hint style="info" %}
To try this code yourself, install the example files from the official GitHub repository: [https://github.com/apphp/ai-with-php-examples](https://github.com/apphp/ai-with-php-examples)
{% endhint %}

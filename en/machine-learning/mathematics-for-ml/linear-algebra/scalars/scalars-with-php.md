# Scalars with PHP

### Scalar Operations with MathPHP

In PHP by using **MathPHP**, you can easily apply scalar operations to vectors, perform trigonometric computations, and manipulate scalars in a structured way, making mathematical programming in PHP more convenient and reliable.

**Example of Use:**

<details>

<summary>Example with MathPHP</summary>

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

</details>

### Scalar Operations with Pure PHP

In PHP it can be written as a class **Scalar** with implementation of a set of scalar operations.

This class is a PHP implementation of scalar operations commonly used in linear algebra and, by extension, in various AI and machine learning algorithms. It provides a robust set of methods for performing vectors calculations, making it a valuable tool for developers working on AI projects in PHP.

<details>

<summary>Example of Class <strong>Scalar</strong></summary>

```php
namespace Apphp\MLKit\Math\Linear;

use DivisionByZeroError;
use InvalidArgumentException;
use Random\RandomException;

/**
 * Scalar class provides a comprehensive set of mathematical operations for scalar values
 *
 * This class includes basic arithmetic operations, scalar-vector operations,
 * mathematical functions, trigonometric operations, random number generation,
 * comparison operations, and bitwise operations. All methods are static and
 * designed for high precision mathematical computations.
 *
 * @package Apphp\MLKit\Math\Linear
 */
class Scalar {
    /**
     * Default precision settings for different operation types
     * @var array<string, int>
     */
    private static array $defaultPrecisions = [
        'basic_arithmetic' => 5,    // Basic arithmetic operations
        'exponential' => 8,         // Exponential and logarithmic operations
        'trigonometric' => 8,       // Trigonometric functions
        'vector' => 6,              // Vector operations
    ];

    /**
     * Custom precision settings that override defaults
     * @var array<string, int>
     */
    private static array $customPrecisions = [];

    /**
     * Global precision setting that overrides all others when set
     * @var int|null
     */
    private static ?int $globalPrecision = null;

    /**
     * Sets the global precision for all operations
     *
     * @param int $precision Number of decimal places
     * @return void
     * @throws InvalidArgumentException
     */
    public static function setPrecision(int $precision): void {
        if ($precision < 0) {
            throw new InvalidArgumentException("Precision must be non-negative. Got: {$precision}");
        }
        self::$globalPrecision = $precision;
    }

    /**
     * Sets precision for a specific operation type
     *
     * @param string $operation Operation type ('basic_arithmetic', 'trigonometric', 'exponential', 'vector')
     * @param int $precision Number of decimal places
     * @return void
     * @throws InvalidArgumentException
     */
    public static function setOperationPrecision(string $operation, int $precision): void {
        if ($precision < 0) {
            throw new InvalidArgumentException("Precision must be non-negative. Got: {$precision}");
        }
        if (!isset(self::$defaultPrecisions[$operation])) {
            throw new InvalidArgumentException("Invalid operation type: {$operation}");
        }
        self::$customPrecisions[$operation] = $precision;
    }

    /**
     * Resets all precision settings to defaults
     *
     * @return void
     */
    public static function resetPrecision(): void {
        self::$globalPrecision = null;
        self::$customPrecisions = [];
    }

    /**
     * Gets the current global precision setting
     *
     * @return int|null Current global precision or null if not set
     */
    public static function getPrecision(): ?int {
        return self::$globalPrecision;
    }

    /**
     * Gets the current precision for a specific operation type
     *
     * @param string $operation Operation type
     * @return int Current precision for the operation
     * @throws InvalidArgumentException
     */
    public static function getOperationPrecision(string $operation): int {
        return self::getOptimalPrecision($operation);
    }

    /**
     * Gets the optimal precision for a given operation
     *
     * @param string $operation Type of operation ('basic_arithmetic', 'trigonometric', 'exponential', 'vector')
     * @param int|null $precision Optional precision override
     * @return int Optimal precision for the operation
     * @throws InvalidArgumentException
     */
    private static function getOptimalPrecision(string $operation, ?int $precision = null): int {
        // Method-specific precision has highest priority
        if ($precision !== null) {
            if ($precision < 0) {
                throw new InvalidArgumentException("Precision must be non-negative. Got: {$precision}");
            }
            return $precision;
        }

        // Custom operation-specific precision has second priority
        if (isset(self::$customPrecisions[$operation])) {
            return self::$customPrecisions[$operation];
        }

        // Global precision overrides defaults if set
        if (self::$globalPrecision !== null) {
            return self::$globalPrecision;
        }

        // Default precision for operation type
        if (isset(self::$defaultPrecisions[$operation])) {
            return self::$defaultPrecisions[$operation];
        }

        throw new InvalidArgumentException("Invalid operation type: {$operation}");
    }

    /**
     * Adds two numbers
     *
     * @param float|int $a First number
     * @param float|int $b Second number
     * @param int|null $precision Number of decimal places to round to
     * @return float Sum of the two numbers
     */
    public static function add(float|int $a, float|int $b, ?int $precision = null): float {
        $precision = self::getOptimalPrecision('basic_arithmetic', $precision);
        return round($a + $b, $precision);
    }

    /**
     * Subtracts second number from the first
     *
     * @param float|int $a Number to subtract from
     * @param float|int $b Number to subtract
     * @param int|null $precision Number of decimal places to round to
     * @return float Result of subtraction
     */
    public static function subtract(float|int $a, float|int $b, ?int $precision = null): float {
        $precision = self::getOptimalPrecision('basic_arithmetic', $precision);
        return round($a - $b, $precision);
    }

    /**
     * Multiplies two numbers
     *
     * @param float|int $a First number
     * @param float|int $b Second number
     * @param int|null $precision Number of decimal places to round to
     * @return float Product of the two numbers
     */
    public static function multiply(float|int $a, float|int $b, ?int $precision = null): float {
        $precision = self::getOptimalPrecision('basic_arithmetic', $precision);
        return round($a * $b, $precision);
    }

    /**
     * Divides first number by the second
     *
     * @param float|int $a Dividend
     * @param float|int $b Divisor
     * @param int|null $precision Number of decimal places to round to
     * @return float|string Result of division or 'undefined' if divisor is zero
     * @throws DivisionByZeroError
     */
    public static function divide(float|int $a, float|int $b, ?int $precision = null): float|string {
        if ($b === 0) {
            throw new DivisionByZeroError('Division by zero is not allowed');
        }
        $precision = self::getOptimalPrecision('basic_arithmetic', $precision);
        return round($a / $b, $precision);
    }

    /**
     * Calculates the floating-point remainder (modulo)
     *
     * @param float|int $a Dividend
     * @param float|int $b Divisor
     * @param int|null $precision Number of decimal places to round to
     * @return float Remainder of the division
     * @throws DivisionByZeroError
     */
    public static function modulus(float|int $a, float|int $b, ?int $precision = null): float {
        if ($b === 0) {
            throw new DivisionByZeroError('Division by zero is not allowed');
        }
        $precision = self::getOptimalPrecision('basic_arithmetic', $precision);
        return round(fmod($a, $b), $precision);
    }

    /**
     * Raises first number to the power of second number
     *
     * @param float|int $a Base number
     * @param float|int $b Exponent
     * @param int|null $precision Number of decimal places to round to
     * @return float Result of exponentiation
     */
    public static function power(float|int $a, float|int $b, ?int $precision = null): float {
        $precision = self::getOptimalPrecision('exponential', $precision);
        return round($a ** $b, $precision);
    }

    /**
     * Multiplies each element of a vector by a scalar value
     *
     * @param float|int $scalar The scalar value to multiply by
     * @param array<int|float> $vector Array of numbers
     * @param int|null $precision Number of decimal places to round to
     * @return array<int|float> Resulting vector after multiplication
     */
    public static function multiplyVector(float|int $scalar, array $vector, ?int $precision = null): array {
        $precision = self::getOptimalPrecision('vector', $precision);
        return array_map(fn ($x) => round($x * $scalar, $precision), $vector);
    }

    /**
     * Adds a scalar value to each element of a vector
     *
     * @param float|int $scalar The scalar value to add
     * @param array<int|float> $vector Array of numbers
     * @param int|null $precision Number of decimal places to round to
     * @return array<int|float> Resulting vector after addition
     */
    public static function addToVector(float|int $scalar, array $vector, ?int $precision = null): array {
        $precision = self::getOptimalPrecision('vector', $precision);
        return array_map(fn ($x) => round($x + $scalar, $precision), $vector);
    }

    /**
     * Calculates the absolute value of a number
     *
     * @param float|int $x Input number
     * @return float Absolute value
     */
    public static function absolute(float|int $x): float {
        return abs($x);
    }

    /**
     * Rounds a number up to the next highest integer
     *
     * @param float|int $x Input number
     * @return float Ceiling value
     */
    public static function ceiling(float|int $x): float {
        return ceil($x);
    }

    /**
     * Rounds a number down to the next lowest integer
     *
     * @param float|int $x Input number
     * @return float Floor value
     */
    public static function floor(float|int $x): float {
        return floor($x);
    }

    /**
     * Rounds a number to the nearest integer
     *
     * @param float|int $x Input number
     * @return float Rounded value
     */
    public static function round(float|int $x): float {
        return round($x);
    }

    /**
     * Calculates e raised to the power of x
     *
     * @param float|int $x The exponent
     * @param int|null $precision Number of decimal places to round to
     * @return float e^x
     */
    public static function exponential(float|int $x, ?int $precision = null): float {
        $precision = self::getOptimalPrecision('exponential', $precision);
        return round(exp($x), $precision);
    }

    /**
     * Calculates the natural logarithm of a number
     *
     * @param float|int $x Input number (must be positive)
     * @param int|null $precision Number of decimal places to round to
     * @return float|string Natural logarithm or 'undefined' if x <= 0
     * @throws InvalidArgumentException
     */
    public static function logarithm(float|int $x, ?int $precision = null): float|string {
        if ($x <= 0) {
            throw new InvalidArgumentException("Logarithm argument must be positive. Got: {$x}");
        }
        $precision = self::getOptimalPrecision('exponential', $precision);
        return round(log($x), $precision);
    }

    /**
     * Calculates the square root of the absolute value of a number
     *
     * @param float|int $x Input number
     * @param int|null $precision Number of decimal places to round to
     * @return float Square root of |x|
     * @throws InvalidArgumentException
     */
    public static function squareRoot(float|int $x, ?int $precision = null): float {
        if ($x < 0) {
            throw new InvalidArgumentException("Square root argument must be non-negative. Got: {$x}");
        }
        $precision = self::getOptimalPrecision('exponential', $precision);
        return round(sqrt(abs($x)), $precision);
    }

    /**
     * Calculates the sine of an angle
     *
     * @param float|int $angle Angle in radians
     * @param int|null $precision Number of decimal places to round to
     * @return float Sine value
     */
    public static function sine(float|int $angle, ?int $precision = null): float {
        $precision = self::getOptimalPrecision('trigonometric', $precision);
        return round(sin($angle), $precision);
    }

    /**
     * Calculates the cosine of an angle
     *
     * @param float|int $angle Angle in radians
     * @param int|null $precision Number of decimal places to round to
     * @return float Cosine value
     */
    public static function cosine(float|int $angle, ?int $precision = null): float {
        $precision = self::getOptimalPrecision('trigonometric', $precision);
        return round(cos($angle), $precision);
    }

    /**
     * Calculates the tangent of an angle
     *
     * @param float|int $angle Angle in radians
     * @param int|null $precision Number of decimal places to round to
     * @return float|string Returns 'undefined' for angles where tangent is undefined (π/2 + nπ)
     */
    public static function tangent(float|int $angle, ?int $precision = null): float|string {
        // Check if angle is π/2 + nπ where tangent is undefined
        $normalized = fmod($angle, M_PI); // Normalize to [0, π]
        if (abs($normalized - M_PI_2) < 0.00000001) {
            return 'undefined';
        }

        $precision = self::getOptimalPrecision('trigonometric', $precision);
        return round(tan($angle), $precision);
    }

    /**
     * Generates a random integer within specified range
     *
     * @param int $min Lower bound (inclusive)
     * @param int $max Upper bound (inclusive)
     * @return int Random integer
     * @throws RandomException
     */
    public static function randomInt(int $min = 1, int $max = 10): int {
        return random_int($min, $max);
    }

    /**
     * Generates a random integer using Mersenne Twister algorithm
     *
     * @param int $min Lower bound (inclusive)
     * @param int $max Upper bound (inclusive)
     * @return int Random integer
     */
    public static function mtRandomInt(int $min = 1, int $max = 10): int {
        return mt_rand($min, $max);
    }

    /**
     * Generates a random float between 0 and 1 using combined linear congruential generator
     *
     * @return float Random float between 0 and 1
     */
    public static function lcgValue(): float {
        return lcg_value();
    }

    /**
     * Checks if first number is greater than second
     *
     * @param float|int $a First number
     * @param float|int $b Second number
     * @return bool True if a > b, false otherwise
     */
    public static function isGreaterThan(float|int $a, float|int $b): bool {
        return $a > $b;
    }

    /**
     * Checks if first number is less than second
     *
     * @param float|int $a First number
     * @param float|int $b Second number
     * @return bool True if a < b, false otherwise
     */
    public static function isLessThan(float|int $a, float|int $b): bool {
        return $a < $b;
    }

    /**
     * Checks if two numbers are equal
     *
     * @param float|int $a First number
     * @param float|int $b Second number
     * @return bool True if a == b, false otherwise
     */
    public static function isEqual(float|int $a, float|int $b): bool {
        return $a == $b;
    }

    /**
     * Checks if two numbers are not equal
     *
     * @param float|int $a First number
     * @param float|int $b Second number
     * @return bool True if a != b, false otherwise
     */
    public static function isNotEqual(float|int $a, float|int $b): bool {
        return $a != $b;
    }

    /**
     * Checks if first number is greater than or equal to second
     *
     * @param float|int $a First number
     * @param float|int $b Second number
     * @return bool True if a >= b, false otherwise
     */
    public static function isGreaterOrEqual(float|int $a, float|int $b): bool {
        return $a >= $b;
    }

    /**
     * Checks if first number is less than or equal to second
     *
     * @param float|int $a First number
     * @param float|int $b Second number
     * @return bool True if a <= b, false otherwise
     */
    public static function isLessOrEqual(float|int $a, float|int $b): bool {
        return $a <= $b;
    }

    /**
     * Performs bitwise AND operation
     *
     * @param int $a First integer
     * @param int $b Second integer
     * @return int Result of bitwise AND
     */
    public static function bitwiseAnd(int $a, int $b): int {
        return $a & $b;
    }

    /**
     * Performs bitwise OR operation
     *
     * @param int $a First integer
     * @param int $b Second integer
     * @return int Result of bitwise OR
     */
    public static function bitwiseOr(int $a, int $b): int {
        return $a | $b;
    }

    /**
     * Performs bitwise XOR operation
     *
     * @param int $a First integer
     * @param int $b Second integer
     * @return int Result of bitwise XOR
     */
    public static function bitwiseXor(int $a, int $b): int {
        return $a ^ $b;
    }

    /**
     * Performs bitwise NOT operation
     *
     * @param int $a Input integer
     * @return int Result of bitwise NOT
     */
    public static function bitwiseNot(int $a): int {
        return ~$a;
    }

    /**
     * Performs left shift operation
     *
     * @param int $a Number to shift
     * @param int $positions Number of positions to shift
     * @return int Result after left shift
     * @throws InvalidArgumentException
     */
    public static function leftShift(int $a, int $positions = 1): int {
        if ($positions < 0) {
            throw new InvalidArgumentException("Shift amount must be non-negative. Got: {$positions}");
        }
        return $a << $positions;
    }

    /**
     * Performs right shift operation
     *
     * @param int $a Number to shift
     * @param int $positions Number of positions to shift
     * @return int Result after right shift
     * @throws InvalidArgumentException
     */
    public static function rightShift(int $a, int $positions = 1): int {
        if ($positions < 0) {
            throw new InvalidArgumentException("Shift amount must be non-negative. Got: {$positions}");
        }
        return $a >> $positions;
    }
}
```

</details>

**Example of Use:**

```php
use Apphp\MLKit\Math\Linear\Scalar;

// Example values
$a = 10;
$b = 3;
$x = -4.2;
$angle = M_PI / 4; // 45 degrees
$vector = [1, 2, 3];

// Arithmetic Operations
echo "Arithmetic Operations:\n---------\n";
echo "$a + $b = " . Scalar::add($a, $b) . "\n";
echo "$a - $b = " . Scalar::subtract($a, $b) . "\n";
echo "$a * $b = " . Scalar::multiply($a, $b) . "\n";
echo "$a / $b = " . Scalar::divide($a, $b) . "\n";
echo "$a % $b = " . Scalar::modulus($a, $b) . "\n";
echo "$a ^ $b = " . Scalar::power($a, $b) . "\n";

// Scalar-Vector Operations
echo "\nScalar-Vector Operations:\n---------\n";
echo '2 * [1, 2, 3] = ';
print_r(Scalar::multiplyVector(2, $vector));
echo '5 + [1, 2, 3] = ';
print_r(Scalar::addToVector(5, $vector));

// Rounding Operations
echo "\nRounding Operations:\n---------\n";
echo "ceil($x) = " . Scalar::ceiling($x) . "\n";
echo "floor($x) = " . Scalar::floor($x) . "\n";
echo "round($x) = " . Scalar::round($x) . "\n";
echo 'e^2 = ' . Scalar::exponential(2) . "\n";
echo 'ln(2.718) = ' . Scalar::logarithm(2.718) . "\n";
echo "√|$x| = ";
try {
    echo Scalar::squareRoot($x) . "\n";
} catch (Exception $e) {
    echo $e->getMessage() . "\n";
}

// Trigonometric Operations
echo "\nTrigonometric Operations:\n---------\n";
echo 'sin(π/4) = ' . Scalar::sine($angle) . "\n";
echo 'cos(π/4) = ' . Scalar::cosine($angle) . "\n";
echo 'tan(π/4) = ' . Scalar::tangent($angle) . "\n";

// Comparison Operations
echo "\nComparison Operations:\n---------\n";
echo "$a > $b = " . (Scalar::isGreaterThan($a, $b) ? 'true' : 'false') . "\n";
echo "$a < $b = " . (Scalar::isLessThan($a, $b) ? 'true' : 'false') . "\n";
echo "$a = $b = " . (Scalar::isEqual($a, $b) ? 'true' : 'false') . "\n";

// Bitwise Operations
echo "\nBitwise Operations:\n---------\n";
echo "$a << 2 = " . Scalar::leftShift($a, 2) . "\n";
echo "$a >> 1 = " . Scalar::rightShift($a, 1) . "\n";

// Random Number Generation
echo "\nRandom Number Generation:\n---------\n";
echo 'Random (0-100): ' . Scalar::randomInt(0, 100) . "\n";
echo 'MT Random (0-100): ' . Scalar::mtRandomInt(0, 100) . "\n";
echo 'LCG Value: ' . Scalar::lcgValue() . "\n";

// Examples with mixed types (integers and floats)
echo "\nMixed Type Examples:\n---------\n";
echo '5 + 2.5 = ' . Scalar::add(5, 2.5) . "\n";
echo '3 * 0.5 = ' . Scalar::multiply(3, 0.5) . "\n";
echo '2 ^ 1.5 = ' . Scalar::power(2, 1.5) . "\n";
echo '1.5 * [1, 2, 3] = ';
print_r(Scalar::multiplyVector(1.5, $vector));

// Precision Control Examples
echo "\nPrecision Control Examples:\n---------\n";
Scalar::setPrecision(3);
echo '1/3 (precision=3): ' . Scalar::divide(1, 3) . "\n";
Scalar::setOperationPrecision('basic_arithmetic', 4);
echo '1/3 (arithmetic precision=4): ' . Scalar::divide(1, 3) . "\n";
Scalar::resetPrecision();
```

{% hint style="info" %}
To try this code yourself, install the example files from the official GitHub repository: [https://github.com/apphp/ai-with-php-examples](https://github.com/apphp/ai-with-php-examples)
{% endhint %}

# Definition and Operations

### Scalars: The Simplest Mathematical Objects

#### Definition

A scalar is a single numerical value. It can be a real number (like **3**, **-0.5**, or **π**) or a complex number (like **2 + 3i**).

In PHP  it can be written like a constant or a variable:

```php
const MY_SCALAR = 1;
$x = 2;
$y = 3;
```

#### Properties

* Scalars have magnitude but no direction.
* They can be used to scale (multiply) other mathematical objects.

#### Examples in Real-World Applications

* Temperature readings
* Price of a stock
* Speed of a car

### Scalar Operations with PHP

In PHP it can be written as follows:

#### Basic Arithmetic Operations

```php
$a = 5;
$b = 2;

echo $a + $b;  // Addition: 7
echo $a - $b;  // Subtraction: 3
echo $a * $b;  // Multiplication: 10
echo $a / $b;  // Division: 2.5
echo $a % $b;  // Modulus: 1
echo $a ** $b; // Exponentiation: 25
```

#### Scalar-Vector Operations

```php
$scalar = 2;
$vector = [1, 2, 3];

// Scalar-vector multiplication
$result = array_map(function($x) use ($scalar) {
    return $x * $scalar;
}, $vector);
print_r($result); // Output: Array ( [0] => 2 [1] => 4 [2] => 6 )

// Scalar-vector addition
$result = array_map(function($x) use ($scalar) {
    return $x + $scalar;
}, $vector);
print_r($result); // Output: Array ( [0] => 3 [1] => 4 [2] => 5 )
```

#### Scalar Functions

```php
$x = -3.7;

echo abs($x);       // Absolute value: 3.7
echo ceil($x);      // Ceiling: -3
echo floor($x);     // Floor: -4
echo round($x);     // Round: -4
echo exp($x);       // Exponential: 0.024787521766663
echo log($x);       // Natural logarithm: NAN (Not a Number, as log of negative is undefined)
echo sqrt(abs($x)); // Square root of absolute value: 1.9235384061671
```

#### Trigonometric Operations

```php
$angle = M_PI / 4; // 45 degrees in radians

echo sin($angle); // Sine: 0.70710678118655
echo cos($angle); // Cosine: 0.70710678118655
echo tan($angle); // Tangent: 1
```

#### Random Number Generation

```php
echo rand(1, 10);  // Random integer between 1 and 10
echo mt_rand(1, 10);  // Faster random integer between 1 and 10
echo lcg_value();  // Random float between 0 and 1
```

#### Comparison Operations

```php
$a = 5;
$b = 3;

var_dump($a > $b);   // bool(true)
var_dump($a < $b);   // bool(false)
var_dump($a == $b);  // bool(false)
var_dump($a != $b);  // bool(true)
var_dump($a >= $b);  // bool(true)
var_dump($a <= $b);  // bool(false)
```

#### Bitwise Operations

```php
$a = 5;  // 101 in binary
$b = 3;  // 011 in binary

echo $a & $b;  // Bitwise AND: 1 (001 in binary)
echo $a | $b;  // Bitwise OR: 7 (111 in binary)
echo $a ^ $b;  // Bitwise XOR: 6 (110 in binary)
echo ~$a;      // Bitwise NOT: -6
echo $a << 1;  // Left shift: 10 (1010 in binary)
echo $a >> 1;  // Right shift: 2 (10 in binary)
```

These scalar operations form the foundation for more complex calculations in PHP, particularly in the context of mathematical and machine learning applications. They can be used to manipulate individual values, perform element-wise operations on arrays, and implement various algorithms.

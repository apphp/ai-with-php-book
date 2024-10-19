# Vectors

### Vectors in Linear Algebra

Vectors are fundamental objects in linear algebra, representing quantities with both magnitude and direction. Unlike scalars, which are single numbers, vectors consist of ordered lists of numbers. They can be visualized as arrows in space, where the length of the arrow represents magnitude and its orientation represents direction. Vectors are crucial in many mathematical and real-world applications, from physics to computer graphics and machine learning. Key operations with vectors include addition, scalar multiplication, and dot products. Understanding vectors is essential for grasping more complex concepts in linear algebra, such as matrix operations and linear transformations.

#### Definition

A vector is an ordered list of numbers, often represented as an arrow in space. It has both magnitude and direction.

In PHP  it can be written as an array:

```php
$vector = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11];
echo count($vector);   
// output: 11 - the length of $myVector
```

#### Representation

Vectors can be written in various ways:

* As a column: $$v = [2, 3, -1]^T$$
* As a row: $$v = [2 3 -1]$$
* In set notation: $$v = (2, 3, -1)$$

#### Properties

* Vectors have magnitude (length) and direction.
* The dimension of a vector is the number of components it has.

#### Types of Vectors

1. Zero Vector: A vector with all components equal to zero.
2. Unit Vector: A vector with a magnitude of 1.
3. Basis Vector: A set of vectors that can represent any vector in the space through linear combination.

### Vector Operations

#### 1. Addition and Subtraction

Vectors of the same dimension can be added or subtracted component-wise.

Example 1: $$[2, 3] + [1, -1] = [2+1, 3+(-1)] = [3, 2]$$

Example 2: $$[2, 3] - [1, -1] = [1, 4]$$

{% hint style="info" %}
To add vectors, we add their corresponding components.&#x20;
{% endhint %}

<details>

<summary>Step by step explanation</summary>

1. Align the vectors: \[2, 3] \[1, -1]
2. Add the first components: 2 + 1 = 3
3. Add the second components: 3 + (-1) = 2
4. Combine the results: \[3, 2]

Therefore, \[2, 3] + \[1, -1] = \[3, 2]

This result means that when we add these two vectors, we get a new vector with the first component 3 and the second component 2.

</details>

#### 2. Scalar Multiplication

A vector can be multiplied by a scalar, scaling each component.

Example: $$3 * [2, -1] = [6, -3]$$   &#x20;

<details>

<summary>Step by step explanation</summary>

1. Identify the components:
   * Scalar: 3
   * Vector: \[2, -1]
2.  Multiply the scalar by each component of the vector: First component:

    * 3 \* 2 = 6

    Second component:

    * 3 \* (-1) = -3
3. Construct the resulting vector: \[6, -3]

Therefore, 3 \* \[2, -1] = \[6, -3]

The resulting vector \[6, -3] is in the same direction as the original vector \[2, -1], but its magnitude has been tripled.

</details>

#### 3. Dot Product

The dot product of two vectors results in a scalar.

Example: $$[1, 2] · [3, 4] = 13 + 24 = 11$$&#x20;

{% hint style="info" %}
The dot product (also known as scalar product) of two vectors is calculated by multiplying corresponding components and then summing the results.
{% endhint %}

<details>

<summary>Step by step explanation</summary>

1. Identify the vectors:
   * Vector 1: \[1, 2]
   * Vector 2: \[3, 4]
2. Multiply corresponding components:
   * First components: 1 \* 3 = 3
   * Second components: 2 \* 4 = 8
3. Sum the results: 3 + 8 = 11
4. The final result: \[1, 2] · \[3, 4] = 11

Here's a more detailed breakdown:

1. (1 \* 3) = 3
2. (2 \* 4) = 8
3. 3 + 8 = 11

Key points about the dot product:

* The result is a scalar (a single number), not a vector.
* The dot product is commutative: \[1, 2] · \[3, 4] = \[3, 4] · \[1, 2]
* It's used to calculate the angle between vectors and in many other applications in physics and mathematics.

Geometrically, the dot product is related to the angle between the vectors and their magnitudes. A positive dot product indicates an acute angle, while a negative dot product indicates an obtuse angle.

</details>

#### 4. Cross Product

The cross product of two 3D vectors results in a vector perpendicular to both.

Example: $$[1, 0, 0] × [0, 1, 0] = [0, 0, 1]$$

{% hint style="info" %}
The cross product is an operation on two vectors in three-dimensional space, resulting in a vector that is perpendicular to both of the vectors being multiplied.
{% endhint %}

<details>

<summary>Step by step explanation</summary>

1. Identify the vectors:
   * Vector a = \[1, 0, 0] (this is the unit vector in the x-direction, often denoted as i)
   * Vector b = \[0, 1, 0] (this is the unit vector in the y-direction, often denoted as j)
2. Recall the cross product formula for 3D vectors: \
   For a = \[a1, a2, a3] and b = \[b1, b2, b3], a × b = \[a2b3 - a3b2, a3b1 - a1b3, a1b2 - a2b1]
3. Let's calculate each component:
   * First component: a2b3 - a3b2 = (0 \* 0) - (0 \* 1) = 0 - 0 = 0
   * Second component: a3b1 - a1b3 = (0 \* 0) - (1 \* 0) = 0 - 0 = 0
   * Third component: a1b2 - a2b1 = (1 \* 1) - (0 \* 0) = 1 - 0 = 1
4. Combining the results: \[1, 0, 0] × \[0, 1, 0] = \[0, 0, 1]

Therefore, the cross product \[1, 0, 0] × \[0, 1, 0] = \[0, 0, 1]

Key points:

* The resulting vector \[0, 0, 1] is perpendicular to both input vectors.
* This result is actually the unit vector in the z-direction (often denoted as k).
* The magnitude of the cross product is equal to the area of the parallelogram formed by the two vectors.
* The cross product is anti-commutative: a × b = -(b × a)

Geometrically, this result makes sense because:

* \[1, 0, 0] points in the positive x-direction
* \[0, 1, 0] points in the positive y-direction
* Their cross product \[0, 0, 1] points in the positive z-direction, which is perpendicular to both x and y directions.

</details>

### Vector Operations with PHP

In PHP  it can be written as a class **Vector** with implementation of a set of vector operations:

<details>

<summary>Class Vector</summary>

```php
<?php

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

// Example usage
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

</details>

Run this code by yourself:

{% embed url="https://onlinephp.io/c/9d39020b-7048-46bf-af27-a02add1c8513" %}
Vector Operations
{% endembed %}

### Vector Spaces

A vector space is a collection of vectors that is closed under addition and scalar multiplication. This concept is crucial for understanding linear algebra and its applications.

### Applications in Machine Learning

1. Data Representation: Each data point in a dataset can be represented as a vector.
2. Feature Vectors: In machine learning models, features of an instance are often represented as vectors.
3. Weight Vectors: Many ML algorithms use vectors to represent the weights of different features.

### Conclusion

Understanding scalars and vectors is crucial for mastering linear algebra. These concepts form the foundation for more advanced topics like matrices, linear transformations, and eigenvalues. As you progress in your study of linear algebra, you'll see how these basic ideas combine to create powerful tools for solving complex problems in various fields.

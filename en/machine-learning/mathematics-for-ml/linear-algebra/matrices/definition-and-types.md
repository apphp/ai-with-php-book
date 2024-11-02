# Definition and Types

### Definition of Matrices

A matrix is a rectangular array of numbers, symbols, or expressions arranged in rows and columns. In general, a matrix with m rows and n columns is called an $$m × n$$ matrix.

#### General form of a matrix A:

#### &#x20;$$A = \begin{bmatrix} a_{11} & a_{12} & \dots & a_{1n} \\ a_{21} & a_{22} & \dots & a_{2n} \\ \vdots & \vdots & \ddots & \vdots \\ a_{m1} & a_{m2} & \dots & a_{mn} \end{bmatrix}$$

Where $$a_{ij}$$ represents the element in the i-th row and j-th column.

In PHP  matrices can be written like n-dimensional arrays:

```php
// 1-dimensional array
$matrixD1 = [1, 2];

// 2-dimensional array
$matrixD2 = [
    [1, 2],[3, 4],[5, 6]
];

// 3-dimensional array
$matrixD3 = [
    [
        [1, 2, 3],
        [4, 5, 6],
        [7, 8, 9]
    ],
    [
        [10, 11, 12],
        [13, 14, 15],
        [16, 17, 18]
    ],
    [
        [19, 20, 21],
        [22, 23, 24],
        [25, 26, 27]
    ]
];
```

### Types of Matrices

#### Row matrix:

A matrix with only one row $$(1 × n)$$.&#x20;

Example: $$\begin{bmatrix} 1\ 2\ 3\ 4 \end{bmatrix}$$

#### Column matrix: &#x20;

A matrix with only one column $$(m × 1)$$.&#x20;

Example: $$A = \begin{bmatrix} 1 \\ 2 \\ 3 \end{bmatrix}$$

#### Square matrix:&#x20;

A matrix with an equal number of rows and columns (n × n).&#x20;

Example: $$A = \begin{bmatrix} 1 & 2 \\ 3 & 4 \end{bmatrix}$$

#### Diagonal matrix:&#x20;

A square matrix where all elements outside the main diagonal are zero.&#x20;

Example: $$A = \begin{bmatrix} 1 & 0 & 0 \\ 0 & 2 & 0 \\ 0 & 0 & 3 \end{bmatrix}$$

#### Identity matrix:&#x20;

A square matrix with 1s on the main diagonal and 0s elsewhere, usually denoted by I.&#x20;

Example: $$A = \begin{bmatrix} 1 & 0 & 0 \\ 0 & 1 & 0 \\ 0 & 0 & 1 \end{bmatrix}$$

#### Zero matrix:&#x20;

A matrix where all elements are zero, usually denoted by 0.&#x20;

Example: $$A = \begin{bmatrix} 0 & 0 \\ 0 & 0 \end{bmatrix}$$

#### Symmetric matrix:&#x20;

A square matrix that is equal to its transpose $$(A = A^T)$$.&#x20;

Example: $$[1\ 2\ 3] [2\ 4\ 5] [3\ 5\ 6]$$

```
A = [1 2 3]
    [2 4 5]
    [3 5 6]
```

#### Skew-symmetric matrix:&#x20;

A square matrix A where .&#x20;

Example: $$[0\ 1\ -2] [-1\ 0\ 3] [2\ -3\ 0]$$

```
A = [0 1 -2]
    [-1 0 3]
    [2 -3 0]
```

#### Upper triangular matrix:&#x20;

A square matrix where all elements below the main diagonal are zero.&#x20;

Example: $$[1\ 2\ 3] [0\ 4\ 5] [0\ 0\ 6]$$

```
A = [1 2 3]
    [0 4 5]
    [0 0 6]
```

#### Lower triangular matrix:&#x20;

A square matrix where all elements above the main diagonal are zero.&#x20;

Example: $$[1\ 0\ 0] [2\ 3\ 0] [4\ 5\ 6]$$

```
A = [1 0 0]
    [2 3 0]
    [4 5 6]
```

Understanding these types of matrices is crucial for working with more advanced concepts in linear algebra, such as matrix operations, determinants, and eigenvalues

# Implementation in PHP

### Propositional Logic with PHP

In PHP it can be written using 2 simple classes `Proposition` and `PropositionalLogic` with implementation of a set of propositional logic operations.

**Example of Use:**

```php
use Apphp\MLKit\Reasoning\Logic\Propositional\Proposition;
use Apphp\MLKit\Reasoning\Logic\Propositional\PropositionalLogic;

// Initialize the propositional logic system
$logic = new PropositionalLogic();

// Define propositions
$p = new Proposition('P', false);
$q = new Proposition('Q', false);
$r = new Proposition('R', false);


// Example 1: Basic Logical Operations
echo "\n---------------------------------------";
echo "\n| Example 1: Basic Logical Operations";
echo "\n---------------------------------------\n";
echo "Demonstrating basic logical operations (AND, OR, NOT)\n";

// Truth table for AND operation
echo "\nAND Operation (P ∧ Q):\n";
$truthTableAND = $logic->generateTruthTable([$p, $q], function () use ($logic, $p, $q) {
    return $logic->evaluateCompound(
        [$p, $q],
        [['operator' => 'AND', 'operands' => [0, 1]]]
    );
});
$logic->printTruthTable($truthTableAND);

// Truth table for OR operation
echo "\nOR Operation (P ∨ Q):\n";
$truthTableOR = $logic->generateTruthTable([$p, $q], function () use ($logic, $p, $q) {
    return $logic->evaluateCompound(
        [$p, $q],
        [['operator' => 'OR', 'operands' => [0, 1]]]
    );
});
$logic->printTruthTable($truthTableOR);

// Truth table for NOT operation
echo "\nNOT Operation (¬P):\n";
$truthTableNOT = $logic->generateTruthTable([$p], function () use ($logic, $p) {
    return $logic->evaluateCompound(
        [$p],
        [['operator' => 'NOT', 'operands' => [0]]]
    );
});
$logic->printTruthTable($truthTableNOT);

// Example 2: Basic Derived Operations
echo "\n---------------------------------------";
echo "\n| Example 2: Basic Derived Operations";
echo "\n---------------------------------------\n";
echo "Demonstrating IMPLIES, IFF, XOR operations\n";

// Truth table for IMPLIES operation
echo "\nIMPLIES Operation (P → Q):\n";
$truthTableIMPLIES = $logic->generateTruthTable([$p, $q], function () use ($logic, $p, $q) {
    return $logic->evaluateCompound(
        [$p, $q],
        [['operator' => 'IMPLIES', 'operands' => [0, 1]]]
    );
});
$logic->printTruthTable($truthTableIMPLIES);

// Truth table for IFF operation
echo "\nIFF Operation (P ↔ Q):\n";
$truthTableIFF = $logic->generateTruthTable([$p, $q], function () use ($logic, $p, $q) {
    return $logic->evaluateCompound(
        [$p, $q],
        [['operator' => 'IFF', 'operands' => [0, 1]]]
    );
});
$logic->printTruthTable($truthTableIFF);

// Truth table for XOR operation
echo "\nXOR Operation (P ⊕ Q):\n";
$truthTableXOR = $logic->generateTruthTable([$p, $q], function () use ($logic, $p, $q) {
    return $logic->evaluateCompound(
        [$p, $q],
        [['operator' => 'XOR', 'operands' => [0, 1]]]
    );
});
$logic->printTruthTable($truthTableXOR);

// Example 3: Alternative Operations
echo "\n---------------------------------------";
echo "\n| Example 3: Alternative Operations";
echo "\n---------------------------------------\n";
echo "Demonstrating NAND and NOR operations\n";

// Truth table for NAND operation
echo "\nNAND Operation (P ↑ Q):\n";
$truthTableNAND = $logic->generateTruthTable([$p, $q], function () use ($logic, $p, $q) {
    return $logic->evaluateCompound(
        [$p, $q],
        [['operator' => 'NAND', 'operands' => [0, 1]]]
    );
});
$logic->printTruthTable($truthTableNAND);

// Truth table for NOR operation
echo "\nNOR Operation (P ↓ Q):\n";
$truthTableNOR = $logic->generateTruthTable([$p, $q], function () use ($logic, $p, $q) {
    return $logic->evaluateCompound(
        [$p, $q],
        [['operator' => 'NOR', 'operands' => [0, 1]]]
    );
});
$logic->printTruthTable($truthTableNOR);

// Example 4: Common Logical Equivalences
echo "\n---------------------------------------";
echo "\n| Example 4: Common Logical Equivalences";
echo "\n---------------------------------------\n";
echo "Demonstrating important logical equivalences\n";

// Double Negation Law: ¬¬P ↔ P
echo "\nDouble Negation Law (¬¬P ↔ P):\n";
$truthTableDoubleNeg = $logic->generateTruthTable([$p], function () use ($logic, $p) {
    return $logic->evaluateCompound(
        [$p],
        [
            ['operator' => 'NOT', 'operands' => [0]],
            ['operator' => 'NOT', 'operands' => [1]],
            ['operator' => 'IFF', 'operands' => [2, 0]],
        ]
    );
});
$logic->printTruthTable($truthTableDoubleNeg);

// Contrapositive Law: (P → Q) ↔ (¬Q → ¬P)
echo "\nContrapositive Law ((P → Q) ↔ (¬Q → ¬P)):\n";
$truthTableContrapositive = $logic->generateTruthTable([$p, $q], function () use ($logic, $p, $q) {
    return $logic->evaluateCompound(
        [$p, $q],
        [
            ['operator' => 'IMPLIES', 'operands' => [0, 1]],  // P → Q
            ['operator' => 'NOT', 'operands' => [1]],         // ¬Q
            ['operator' => 'NOT', 'operands' => [0]],         // ¬P
            ['operator' => 'IMPLIES', 'operands' => [2, 3]],  // ¬Q → ¬P
            ['operator' => 'IFF', 'operands' => [1, 4]],       // (P → Q) ↔ (¬Q → ¬P)
        ]
    );
});
$logic->printTruthTable($truthTableContrapositive);

// Example 5: Material Implication using evaluateCompound
// (P → Q) ¬P ∨ Q)
echo "\n---------------------------------------";
echo "\n| Example 5: Material Implication";
echo "\n---------------------------------------\n";
echo "Formula: (P → Q) ¬P ∨ Q)\n";
echo "This demonstrates that P implies Q is logically equivalent to not-P or Q\n";

$truthTable5 = $logic->generateTruthTable([$p, $q], function () use ($logic, $p, $q) {
    return $logic->evaluateCompound(
        [$p, $q],
        [
            ['operator' => 'IMPLIES', 'operands' => [0, 1]],  // P → Q
            ['operator' => 'NOT', 'operands' => [0]],         // ¬P
            ['operator' => 'OR', 'operands' => [2, 1]],       // ¬P ∨ Q
            ['operator' => 'IFF', 'operands' => [1, 3]],       // (P → Q) ¬P ∨ Q)
        ]
    );
});
$logic->printTruthTable($truthTable5);

// Example 6: Advanced Logic Operations
echo "\n---------------------------------------";
echo "\n| Example 6: Advanced Logic Operations";
echo "\n---------------------------------------\n";
echo "Formula: (P NAND Q) XOR (Q NOR R)\n";
echo "This demonstrates the use of NAND, NOR, and XOR operators\n";

$truthTable6 = $logic->generateTruthTable([$p, $q, $r], function () use ($logic, $p, $q, $r) {
    return $logic->evaluateCompound(
        [$p, $q, $r],
        [
            ['operator' => 'NAND', 'operands' => [0, 1]],  // P NAND Q
            ['operator' => 'NOR', 'operands' => [1, 2]],   // Q NOR R
            ['operator' => 'XOR', 'operands' => [3, 4]],    // (P NAND Q) XOR (Q NOR R)
        ]
    );
});
$logic->printTruthTable($truthTable6);

// Example 7: Working with Proposition Objects
echo "\n---------------------------------------";
echo "\n| Example 7: Working with Proposition Objects";
echo "\n---------------------------------------\n";
echo "Demonstrating Proposition object features\n\n";

$p = new Proposition('P', true);
echo 'Original: ' . $p . "\n";

$notP = $p->negate();
echo 'Negation: ' . $notP . "\n";

$copy = $p->copy();
$copy->setValue(false);
echo 'Original after copy modification: ' . $p . "\n";
echo 'Modified copy: ' . $copy . "\n";
```

{% hint style="info" %}
To try this code yourself, install the example files from the official GitHub repository: [https://github.com/apphp/ai-with-php-examples](https://github.com/apphp/ai-with-php-examples)
{% endhint %}

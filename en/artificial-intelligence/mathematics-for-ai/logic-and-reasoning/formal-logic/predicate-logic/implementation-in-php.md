# Implementation in PHP

### Predicate Logic with PHP

In PHP it can be written using 5 simple classes `Term`, `Domain`, `Predicate`, `AtomicFormula` and `PredicateLogic` with implementation of a set of propositional logic operations.

**Example of Use:**

```php
use app\classes\logic\AtomicFormula;
use app\classes\logic\Domain;
use app\classes\logic\Predicate;
use app\classes\logic\PredicateLogic;
use app\classes\logic\Term;

//echo "\n\n====== FIRST-ORDER PREDICATE LOGIC EXAMPLES ======\n\n";

// Define the domain (universe of discourse)
$domain = new Domain();
$domain->addObject('Socrates', ['type' => 'human', 'mortal' => true]);
$domain->addObject('Plato', ['type' => 'human', 'mortal' => true]);
$domain->addObject('Zeus', ['type' => 'god', 'mortal' => false]);
$domain->addObject('Athena', ['type' => 'god', 'mortal' => false]);
$domain->addObject('Fido', ['type' => 'dog', 'mortal' => true]);

// Define predicates
$humanPredicate = new Predicate('Human', 1);
$mortalPredicate = new Predicate('Mortal', 1);
$dogPredicate = new Predicate('Dog', 1);
$mammalPredicate = new Predicate('Mammal', 1);

// Define terms
$x = new Term('x', true); // Variable x
$y = new Term('y', true); // Variable y
$socrates = new Term('Socrates', false); // Constant Socrates
$plato = new Term('Plato', false); // Constant Plato
$fido = new Term('Fido', false); // Constant Fido

// Define atomic formulas
$humanX = new AtomicFormula($humanPredicate, [$x]);
$mortalX = new AtomicFormula($mortalPredicate, [$x]);
$humanSocrates = new AtomicFormula($humanPredicate, [$socrates]);
$mortalSocrates = new AtomicFormula($mortalPredicate, [$socrates]);
$dogFido = new AtomicFormula($dogPredicate, [$fido]);
$mammalX = new AtomicFormula($mammalPredicate, [$x]);
$dogX = new AtomicFormula($dogPredicate, [$x]);

// Create predicate logic engine
$logic = new PredicateLogic();

// Example 1: Classical Syllogism
echo "Example 1: Classical Syllogism\n";
echo "-------------------------------\n";
echo "Type: First-Order Predicate Logic with Categorical Syllogism\n";
echo "Features: Universal quantification, predication, deductive reasoning\n";
echo "Formula: (∀x: Human(x) → Mortal(x)) ∧ Human(Socrates) → Mortal(Socrates)\n";
echo "This is the classic 'All humans are mortal, Socrates is human, therefore Socrates is mortal' argument\n\n";

// Premise 1: All humans are mortal (∀x: Human(x) → Mortal(x))
$allHumansAreMortal = $logic->forAll($domain, 'x', function ($assignment) use ($logic, $domain, $humanX, $mortalX) {
    $isHuman = $humanX->evaluate($domain, $assignment);
    $isMortal = $mortalX->evaluate($domain, $assignment);
    return $logic->IMPLIES($isHuman, $isMortal);
});

// Premise 2: Socrates is human
$socratesIsHuman = $humanSocrates->evaluate($domain);

// Conclusion: Socrates is mortal
$socratesIsMortal = $mortalSocrates->evaluate($domain);

// Detailed output with formal representation
echo "Step-by-step evaluation:\n";
echo "Premise 1: ∀x: Human(x) → Mortal(x)\n";
echo "Formal interpretation: For all objects x in the domain, if x is human, then x is mortal.\n";
echo 'Result: ' . ($allHumansAreMortal ? 'True' : 'False') . "\n\n";

echo "Premise 2: Human(Socrates)\n";
echo "Formal interpretation: The object 'Socrates' satisfies the Human predicate.\n";
echo 'Result: ' . ($socratesIsHuman ? 'True' : 'False') . "\n\n";

echo "Conclusion: Mortal(Socrates)\n";
echo "Formal interpretation: The object 'Socrates' satisfies the Mortal predicate.\n";
echo 'Result: ' . ($socratesIsMortal ? 'True' : 'False') . "\n\n";

echo 'The syllogism is ' . (($allHumansAreMortal && $socratesIsHuman && $socratesIsMortal) ? 'valid' : 'invalid') . ".\n\n";

// Example 2: Existential Quantifier
echo "\nExample 2: Existential Quantifier\n";
echo "-------------------------------\n";
echo "Type: First-Order Predicate Logic with Existential Quantification\n";
echo "Features: Existence assertion\n";
echo "Formula: ∃x: Human(x) ∧ Mortal(x)\n";
echo "This checks if there exists at least one human who is mortal\n\n";

$existsHumanAndMortal = $logic->exists($domain, 'x', function ($assignment) use ($logic, $domain, $humanX, $mortalX) {
    $isHuman = $humanX->evaluate($domain, $assignment);
    $isMortal = $mortalX->evaluate($domain, $assignment);
    return $logic->AND($isHuman, $isMortal);
});

echo "Evaluation of ∃x: Human(x) ∧ Mortal(x):\n";
echo "Formal interpretation: There exists at least one object x in the domain such that x is human AND x is mortal.\n";
echo 'Result: ' . ($existsHumanAndMortal ? 'True' : 'False') . "\n";

// Get examples of objects that satisfy the formula
$satisfyingObjects = [];
foreach ($domain->getAllObjects() as $objectName => $properties) {
    $assignment = ['x' => $objectName];
    $isHuman = $humanX->evaluate($domain, $assignment);
    $isMortal = $mortalX->evaluate($domain, $assignment);
    if ($logic->AND($isHuman, $isMortal)) {
        $satisfyingObjects[] = $objectName;
    }
}

if (!empty($satisfyingObjects)) {
    echo 'Objects that satisfy this formula: ' . implode(', ', $satisfyingObjects) . "\n\n";
} else {
    echo "No objects satisfy this formula.\n\n";
}

// Example 3: Universal Quantifier with Multiple Predicates
echo "\nExample 3: Universal Quantifier with Multiple Predicates\n";
echo "-------------------------------\n";
echo "Type: First-Order Predicate Logic with Universal Quantification\n";
echo "Features: Taxonomic classification reasoning\n";
echo "Formula: ∀x: Dog(x) → Mammal(x)\n";
echo "This checks if all dogs are mammals\n\n";

// Add mammal property to domain objects
$domain->addObject('Fido', ['type' => 'dog', 'mortal' => true, 'mammal' => true]);

// All dogs are mammals
$allDogsAreMammals = $logic->forAll($domain, 'x', function ($assignment) use ($logic, $domain, $dogX, $mammalX) {
    $isDog = $dogX->evaluate($domain, $assignment);
    $isMammal = $mammalX->evaluate($domain, $assignment);
    return $logic->IMPLIES($isDog, $isMammal);
});

echo "Evaluation of ∀x: Dog(x) → Mammal(x):\n";
echo "Formal interpretation: For all objects x in the domain, if x is a dog, then x is a mammal.\n";
echo 'Result: ' . ($allDogsAreMammals ? 'True' : 'False') . "\n\n";

// Example 4: Nested Quantifiers
echo "\nExample 4: Nested Quantifiers\n";
echo "-------------------------------\n";
echo "Type: First-Order Predicate Logic with Nested Quantification\n";
echo "Features: Quantifier ordering, relational predicates\n";
echo "Formula: ∀x: ∃y: GreaterThan(y, x)\n";
echo "This checks if for every number x in the domain  [1, 2, 3, 4, 5], there exists a number y that is greater than x\n\n";

// Domain with numbers
$numberDomain = new Domain();
$numberDomain->addObject('1', ['value' => 1]);
$numberDomain->addObject('2', ['value' => 2]);
$numberDomain->addObject('3', ['value' => 3]);
$numberDomain->addObject('4', ['value' => 4]);
$numberDomain->addObject('5', ['value' => 5]);

$greaterThanPredicate = new Predicate('GreaterThan', 2);
$equalsPredicate = new Predicate('Equals', 2);

// For all x, there exists a y such that y > x
$forAllExistsGreaterThan = $logic->forAll($numberDomain, 'x', function ($xAssignment) use ($logic, $numberDomain, $greaterThanPredicate) {
    return $logic->exists($numberDomain, 'y', function ($yAssignment) use ($logic, $numberDomain, $greaterThanPredicate, $xAssignment) {
        $xValue = $xAssignment['x'];
        $yValue = $yAssignment['y'];
        $greaterThanFormula = new AtomicFormula($greaterThanPredicate, [
            new Term($yValue, false),
            new Term($xValue, false),
        ]);
        return $greaterThanFormula->evaluate($numberDomain);
    });
});

echo "Evaluation of ∀x: ∃y: GreaterThan(y, x):\n";
echo "Formal interpretation: For all numbers x in the domain, there exists at least one number y such that y is greater than x.\n";
echo 'Result: ' . ($forAllExistsGreaterThan ? 'True' : 'False') . "\n";
echo "Explanation: This formula is false because when x = 5, there is no y in our domain that satisfies GreaterThan(y, 5).\n";
echo "For x = 1, 2, 3, and 4, we can find values of y that are greater, but not for x = 5.\n";
echo "Since the universal quantifier requires ALL values to satisfy the condition, the overall result is False.\n\n";

// Example 5: Arithmetic Predicates with Constants
echo "\nExample 5: Arithmetic Predicates with Constants\n";
echo "-------------------------------\n";
echo "Type: First-Order Predicate Logic with Relational Predicate\n";
echo "Features: Constants in predicate expressions\n";
echo "Formula: ∃a: GreaterThan(a, 3)\n";
echo "This checks if there exists a number greater than 3\n\n";

$greaterThan3 = $logic->exists($numberDomain, 'a', function ($assignment) use ($logic, $numberDomain, $greaterThanPredicate) {
    $aValue = $assignment['a'];
    $three = '3';
    $greaterThanFormula = new AtomicFormula($greaterThanPredicate, [
        new Term($aValue, false),
        new Term($three, false),
    ]);
    return $greaterThanFormula->evaluate($numberDomain);
});

echo "Evaluation of ∃a: GreaterThan(a, 3):\n";
echo "Formal interpretation: There exists at least one number a in the domain such that a is greater than 3.\n";
echo 'Result: ' . ($greaterThan3 ? 'True' : 'False') . "\n";

// Get examples of objects that satisfy the formula
$satisfyingNumbers = [];
foreach ($numberDomain->getAllObjects() as $objectName => $properties) {
    $assignment = ['a' => $objectName];
    $greaterThanFormula = new AtomicFormula($greaterThanPredicate, [
        new Term($objectName, false),
        new Term('3', false),
    ]);
    if ($greaterThanFormula->evaluate($numberDomain)) {
        $satisfyingNumbers[] = $objectName;
    }
}

if (!empty($satisfyingNumbers)) {
    echo 'Numbers that satisfy this formula: ' . implode(', ', $satisfyingNumbers) . "\n";
} else {
    echo "No numbers satisfy this formula.\n";
}
```

{% hint style="info" %}
To try this code yourself, install the example files from the official GitHub repository: [https://github.com/apphp/ai-with-php-examples](https://github.com/apphp/ai-with-php-examples)
{% endhint %}

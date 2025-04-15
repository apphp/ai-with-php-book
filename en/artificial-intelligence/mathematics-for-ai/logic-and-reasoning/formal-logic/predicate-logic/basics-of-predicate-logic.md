# Basics of Predicate Logic

### **Predicates and Variables**

A predicate is a function that represents a property or a relationship between objects. It takes variables as input and returns true or false.

Examples:

* `Tall(John)` → "John is tall."
* `Loves(Alice, Bob)` → "Alice loves Bob."

Here, `Tall()` and `Loves()` are predicates, while `John`, `Alice`, and `Bob` are constants representing objects.

### **Syntax of First-Order Logic**

The syntax of FOL determines which collection of symbols form valid logical expressions. The key elements include:

**Basic Elements of First-Order Logic**

Following are the basic elements of FOL syntax:

* **Constant**: `1, 2, A, John, Mumbai, cat,....`
* **Variables**: `x, y, z, a, b,....`
* **Predicates**: `Brother, Father, >,....`
* **Function**: `sqrt, LeftLegOf, ....`
* **Connectives**: `∧, ∨, ¬, ⇒, ⇔`
* **Equality**: `==`
* **Quantifier**: `∀, ∃`

### **Quantifiers in Predicate Logic**

Quantifiers extend logic to statements involving multiple objects.

*   **Universal Quantifier (∀)**: Expresses that a property holds for all elements in a given domain.

    Example: `∀x Tall(x)` → "All people are tall."
*   **Existential Quantifier (∃)**: Indicates that there exists at least one element satisfying a property.

    Example: `∃x Loves(x, Bob)` → "Someone loves Bob."

### **Properties of Quantifiers**

* In universal quantifier, `∀x∀y` is similar to `∀y∀x`.
* In existential quantifier, `∃x∃y` is similar to `∃y∃x`.
* `∃x∀y` is not similar to `∀y∃x`.

### **Free and Bound Variables**

The quantifiers interact with variables that appear in a suitable way. There are two types of variables in first-order logic:

*   **Free Variable**: A variable is said to be a free variable in a formula if it occurs outside the scope of the quantifier.

    Example: `∀x ∃(y)[P (x, y, z)]`, where `z` is a free variable.
*   **Bound Variable**: A variable is said to be a bound variable in a formula if it occurs within the scope of the quantifier.

    Example: `∀x [A (x) B( y)]`, where `x` and `y` are bound variables.

### **Examples of FOL Using Quantifiers**

1. **All birds fly.**
   * Predicate: `fly(bird)`.
   * Representation: `∀x bird(x) → fly(x)`.
2. **Every man respects his parent.**
   * Predicate: `respect(x, y)`, where `x = man`, and `y = parent`.
   * Representation: `∀x man(x) → respects(x, parent)`.
3. **Some boys play cricket.**
   * Predicate: `play(x, y)`, where `x = boys`, and `y = game`.
   * Representation: `∃x boys(x) → play(x, cricket)`.
4. **Not all students like both Mathematics and Science.**
   * Predicate: `like(x, y)`, where `x = student`, and `y = subject`.
   * Representation: `¬∀x [ student(x) → like(x, Mathematics) ∧ like(x, Science)]`.
5. **Only one student failed in Mathematics.**
   * Predicate: `failed(x, y)`, where `x = student`, and `y = subject`.
   * Representation: `∃x [ student(x) → failed (x, Mathematics) ∧ ∀y [¬(x==y) ∧ student(y) → ¬failed (x, Mathematics)]]`.

### **Atomic and Complex Sentences in FOL**

*   **Atomic Sentences**: The simplest statements in FOL, formed from a predicate followed by a sequence of terms.

    Example: `Brother(Ravi, Ajay)` ("Ravi and Ajay are brothers").
*   **Complex Sentences**: Built by combining atomic sentences using logical connectives.

    Example: `∀x (Man(x) → Respects(x, Parent))` ("Every man respects his parent").

### **Examples of Predicate Logic in AI**

#### **Example 1: AI in Expert Systems (Medical Diagnosis)**

Consider an AI-powered medical diagnosis system.

* `HasFever(x) → "x has a fever."`
* `HasCough(x) → "x has a cough."`
* `HasFlu(x) → "x has the flu."`

The rule:

`∀x (HasFever(x) ∧ HasCough(x)) → HasFlu(x)`

This means: "For all x, if x has a fever and a cough, then x has the flu."

If the AI system knows:

* `HasFever(John)` is true.
* `HasCough(John)` is true.

Then, it can infer:

* `HasFlu(John)` is true (John has the flu).

#### **Example 2: AI in Chatbots (Understanding Relationships)**

Consider an AI chatbot programmed to understand family relationships.

* `Parent(x, y) → "x is a parent of y."`
* `Sibling(x, y) → "x is a sibling of y."`

The rule:

`∀x, y, z (Parent(x, y) ∧ Parent(x, z) ∧ (y ≠ z)) → Sibling(y, z)`

This means: "If x is a parent of y and x is a parent of z, and y and z are different, then y and z are siblings."

If the chatbot knows:

* `Parent(Mary, John)` is true.
* `Parent(Mary, Alice)` is true.

It can infer:

* `Sibling(John, Alice)` is true (John and Alice are siblings).

### **Conclusion**

Predicate logic extends propositional logic by allowing AI to reason about objects, their properties, and their relationships. By using predicates, variables, and quantifiers, AI can perform more complex reasoning tasks, making it essential for applications like expert systems, chatbots, and NLP. Understanding predicate logic is crucial for developing AI systems capable of making intelligent inferences and interacting meaningfully with the world.

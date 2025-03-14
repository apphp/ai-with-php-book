# Propositional Logic

Propositional logic, also known as **Boolean** logic, is a fundamental concept in AI that deals with statements that are either true or false. It provides a systematic way to represent knowledge and reason about facts using logical operators. AI systems utilize propositional logic in various applications, such as expert systems, automated reasoning, and problem-solving. This article explores the basics of propositional logic, its key components, and practical examples of its use in AI.

### **Basics of Propositional Logic**

#### **1. Propositions**

A proposition is a declarative statement that can be either true (T) or false (F), but not both. Examples include:

* "It is Sunday."
* "The Sun rises from the West." (False proposition)
* "3 + 3 = 7." (False proposition)
* "5 is a prime number." (True proposition)

Propositional logic is based on symbolic variables representing logical statements, typically denoted as A, B, C, P, Q, R, etc.

#### **2. Logical Connectives**

Logical connectives are used to connect two simpler propositions or represent a sentence logically. We can create compound propositions with the help of logical connectives. There are mainly five connectives, which are given as follows:

* **Negation (¬P)** – A sentence such as ¬P is called the negation of P. A literal can be either a positive literal or a negative literal.
*   **Conjunction (P ∧ Q)** – A sentence that has the ∧ connective, such as P ∧ Q, is called a conjunction.

    Example: "Rohan is intelligent and hardworking." It can be written as:

    * P = Rohan is intelligent
    * Q = Rohan is hardworking
    * Representation: P∧ Q
*   **Disjunction (P ∨ Q)** – A sentence that has the ∨ connective, such as P ∨ Q, is called disjunction, where P and Q are the propositions.

    Example: "Ritika is a doctor or an engineer."

    * P = Ritika is a doctor
    * Q = Ritika is an engineer
    * Representation: P ∨ Q
*   **Implication (P → Q)** – A sentence such as P → Q is called an implication. Implications are also known as if-then rules.

    Example: "If it is raining, then the street is wet."

    * P = It is raining
    * Q = The street is wet
    * Representation: P → Q
*   **Biconditional (P ⇔ Q)** – A sentence such as P ⇔ Q is a biconditional sentence.

    Example: "If I am breathing, then I am alive."

    * P = I am breathing
    * Q = I am alive
    * Representation: P ⇔ Q

#### **Summarized Table for Propositional Logic Connectives**

<table><thead><tr><th width="156.546875">Logical Connective</th><th width="97.1875">Symbol</th><th>Meaning</th><th>Example</th></tr></thead><tbody><tr><td>Negation</td><td>¬P</td><td>The opposite of P</td><td>¬(It is raining) = It is not raining</td></tr><tr><td>Conjunction</td><td>P ∧ Q</td><td>Both P and Q must be true</td><td>It is raining ∧ It is cold</td></tr><tr><td>Disjunction</td><td>P ∨ Q</td><td>At least one of P or Q must be true</td><td>It is raining ∨ It is cold</td></tr><tr><td>Implication</td><td>P → Q</td><td>If P is true, then Q must also be true</td><td>If it rains → The street is wet</td></tr><tr><td>Biconditional</td><td>P ⇔ Q</td><td>P is true if and only if Q is true</td><td>I am breathing ⇔ I am alive</td></tr></tbody></table>

#### **Truth Tables for All Logical Connectives**

**Negation (¬P)**

| P     | ¬P    |
| ----- | ----- |
| True  | False |
| False | True  |

**Conjunction (P ∧ Q)**

| P     | Q     | P ∧ Q |
| ----- | ----- | ----- |
| True  | True  | True  |
| True  | False | False |
| False | True  | False |
| False | False | False |

**Disjunction (P ∨ Q)**

| P     | Q     | P ∨ Q |
| ----- | ----- | ----- |
| True  | True  | True  |
| True  | False | True  |
| False | True  | True  |
| False | False | False |

**Implication (P → Q)**

| P     | Q     | P → Q |
| ----- | ----- | ----- |
| True  | True  | True  |
| True  | False | False |
| False | True  | True  |
| False | False | True  |

**Biconditional (P ⇔ Q)**

| P     | Q     | P ⇔ Q |
| ----- | ----- | ----- |
| True  | True  | True  |
| True  | False | False |
| False | True  | False |
| False | False | True  |

**Truth Table with Three Propositions**

We can build a proposition composing three propositions P, Q, and R. This truth table consists of 8 rows (2³ combinations of truth values).

| P     | Q     | R     | P ∧ (Q ∨ R) |
| ----- | ----- | ----- | ----------- |
| True  | True  | True  | True        |
| True  | True  | False | True        |
| True  | False | True  | True        |
| True  | False | False | False       |
| False | True  | True  | False       |
| False | True  | False | False       |
| False | False | True  | False       |
| False | False | False | False       |

#### **Precedence of Connectives**

Like arithmetic operators, logical connectives have a precedence order that should be followed while evaluating a propositional statement:

1. **Parenthesis** (Highest precedence)
2. **Negation (¬)**
3. **Conjunction (∧)**
4. **Disjunction (∨)**
5. **Implication (→)**
6. **Biconditional (↔) (Lowest precedence)**

For better clarity, parentheses should be used to ensure the correct interpretation of statements.

#### **Logical Equivalence and Properties of Operators**

Logical equivalence is a property where two propositions yield the same truth values across all possible scenarios. If two propositions A and B are logically equivalent, they are written as **A ⇔ B**.

Some essential properties of propositional logic include:

* **Commutativity:** P ∧ Q = Q ∧ P, P ∨ Q = Q ∨ P
* **Associativity:** (P ∧ Q) ∧ R = P ∧ (Q ∧ R)
* **Identity element:** P ∧ True = P, P ∨ True = True
* **Distributivity:** P ∧ (Q ∨ R) = (P ∧ Q) ∨ (P ∧ R)
* **De Morgan’s Laws:** ¬(P ∧ Q) = ¬P ∨ ¬Q, ¬(P ∨ Q) = ¬P ∧ ¬Q
* **Double Negation:** ¬(¬P) = P

#### **Conclusion**

Propositional logic provides a structured way to represent and process information in AI. Its ability to model real-world scenarios using logical statements makes it a valuable tool for reasoning, decision-making, and automation. While simple, propositional logic forms the foundation for more advanced logical systems used in AI and machine learning, enabling more sophisticated forms of knowledge representation and reasoning.

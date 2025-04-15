# Classification of Logics

Logic is a structured way of reasoning. It allows us to draw valid conclusions, make decisions, and solve problems. In AI and Machine Learning ML, logic is essential. It helps systems understand information and make decisions in a reliable way. This chapter explores how logic is classified into **formal**, **informal**, and **semi-formal** types, along with other useful categories like deductive, modal, and fuzzy logic.

### What Is Logic?

Logic is the study of reasoning — deciding what is true or false, and how to connect facts to reach conclusions.

**Example of simple logic:**

* Fact 1: All cats are animals.
* Fact 2: Luna is a cat.
* Conclusion: Luna is an animal.

This is how logical reasoning works: using known facts to learn something new. AI systems do this in different ways, depending on the type of logic used.

### Basic Classifications of Logic

On of the possible ways of classification of logics is following: Formal, Informal and Semi-Formal

* **Formal Logic**\
  Formal logic (also called symbolic or mathematical logic) follows strict rules and uses symbols to represent ideas. It is highly structured and easy to verify by a computer.
* **Informal Logic**\
  Informal logic is how people reason in everyday life. It’s based on language, experience, and common sense rather than symbols or strict steps.
* **Semi-Formal Logic**\
  Semi-formal logic combines elements of both formal and informal logic. It uses structured rules but allows for uncertainty and natural language interpretation.

#### Why This Classification Matters in AI & ML

Each type of logic supports a different kind of AI system:

<table><thead><tr><th width="170.578125">Logic Type</th><th>Best For</th></tr></thead><tbody><tr><td><strong>Formal</strong></td><td>Systems needing certainty and provable rules (e.g. robots, theorem solvers)</td></tr><tr><td><strong>Informal</strong></td><td>Human-like understanding (e.g. chatbots, language models)</td></tr><tr><td><strong>Semi-formal</strong></td><td>Decisions under uncertainty (e.g. expert systems, legal AI)</td></tr></tbody></table>

### Other Important Classifications of Logic

Beyond the basic categories, logic can also be divided into specialized types used across AI, mathematics, and computer science.

#### Deductive vs. Inductive Logic

* **Deductive Logic**: Goes from general to specific. If the rules are true, the conclusion must be true.\
  **Example:**
  * All mammals breathe air.
  * Whales are mammals.
  * → Whales breathe air.
  * **Used in:** Programming, theorem provers, knowledge bases
* **Inductive Logic**: Goes from specific to general. It creates general rules from observations.\
  **Example:**
  * This apple fell.
  * That apple fell.
  * → All apples fall.
  * **Used in:** Machine learning, pattern recognition, AI training

#### Monotonic vs. Non-Monotonic Logic

*   **Monotonic Logic**: Adding new facts doesn’t change earlier conclusions.\
    **Example:**

    * All birds fly.
    * Tweety is a bird.
    * → Tweety flies.

    **Problem:** Doesn’t handle exceptions like penguins.
*   **Non-Monotonic Logic**: Allows earlier conclusions to change with new information.\
    **Example:**

    * Birds usually fly.
    * But now we learn Tweety is a penguin.
    * → Update: Tweety does not fly.

    **Used in:** Real-world AI systems with incomplete or changing information

#### Modal Logic

Adds terms like “must,” “might,” and “possibly.” Used for reasoning about obligations, beliefs, or future events.

**Operators:**

* □ = necessarily
* ◇ = possibly

**Example:**

* “The robot must stop if an obstacle is detected.” (□ Stop)
* “It might rain tomorrow.” (◇ Rain)

**Used in:** AI planning, simulation, robotics, security modeling

#### Fuzzy Logic

Deals with degrees of truth. Not just true or false, but in between (e.g., 0.7 true).

**Example:**

* Temperature is not just “hot” or “cold,” but “70% hot”
* “The room is somewhat clean.”

**Used in:** Smart devices, natural language processing, image analysis

#### Temporal Logic

Adds time-based reasoning (before, after, always, eventually).

**Example:**

* “The system should always log out users after 10 minutes.”
* “The alarm must ring before the door opens.”

**Used in:** Scheduling, process verification, real-time systems

### Summary Table

<table><thead><tr><th width="179.6328125">Logic Type</th><th>Description</th><th>Used In</th></tr></thead><tbody><tr><td><strong>Formal</strong></td><td>Rule-based, symbolic, provable</td><td>Programming, theorem proving</td></tr><tr><td><strong>Informal</strong></td><td>Natural, flexible, based on context</td><td>Chatbots, conversation, essays</td></tr><tr><td><strong>Semi-Formal</strong></td><td>Mixed, structured with some flexibility</td><td>Legal tech, expert systems</td></tr><tr><td><strong>Deductive</strong></td><td>General to specific reasoning</td><td>Rule engines, compilers</td></tr><tr><td><strong>Inductive</strong></td><td>From data to general patterns</td><td>Machine learning</td></tr><tr><td><strong>Monotonic</strong></td><td>Stable reasoning that doesn’t change</td><td>Classical logic systems</td></tr><tr><td><strong>Non-Monotonic</strong></td><td>Adaptive reasoning that updates with new info</td><td>Dynamic knowledge bases</td></tr><tr><td><strong>Modal</strong></td><td>Reasoning about possibility and necessity</td><td>Planning, simulation, robotics</td></tr><tr><td><strong>Fuzzy</strong></td><td>Reasoning with degrees of truth</td><td>Smart systems, fuzzy controllers</td></tr><tr><td><strong>Temporal</strong></td><td>Logic with time-based conditions</td><td>Scheduling, automation, real-time AI</td></tr></tbody></table>

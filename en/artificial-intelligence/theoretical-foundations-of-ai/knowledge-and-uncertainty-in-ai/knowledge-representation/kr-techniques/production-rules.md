# Production Rules

### 4. Production Rules

Production rules represent knowledge in the form of **IF-THEN** statements, allowing AI systems to make decisions based on conditions.

**Key Components:**

1. **Set of Production Rules:** A collection of IF-THEN statements.
2. **Working Memory:** Stores the current state of the system.
3. **Recognize-Act Cycle:** The process of checking conditions and executing corresponding actions.

**Schema:**

```
IF (at bus stop AND bus arrives) THEN (get into the bus)
IF (on bus AND paid AND empty seat) THEN (sit down)
IF (on bus AND unpaid) THEN (pay charges)
IF (bus arrives at destination) THEN (get down from the bus)
```

**Advantages:**

* Expressed in a natural and modular format.
* Easy to modify, add, or remove rules.

**Disadvantages:**

* Lacks learning capabilities, as results are not stored for future reference.
* Inefficient when handling multiple simultaneous rules.

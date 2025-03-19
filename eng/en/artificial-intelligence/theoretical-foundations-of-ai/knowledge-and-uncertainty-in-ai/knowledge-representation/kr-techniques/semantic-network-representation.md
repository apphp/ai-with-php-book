# Semantic Network Representation

### 2. Semantic Network Representation

Semantic networks represent knowledge using a graph-based structure, where **nodes** represent objects or concepts and **edges** define relationships between them.

**Types of Relations:**

* **IS-A Relationship:** Represents inheritance (e.g., "A cat is a mammal").
* **Kind-Of Relationship:** Describes categorization (e.g., "Mammals are animals").

**Schema:**

```
  [Jerry] --IS-A--> [Cat]
  [Cat] --IS-A--> [Mammal]
  [Mammal] --IS-A--> [Animal]
  [Jerry] --HAS--> [Brown Color]
  [Jerry] --OWNED-BY--> [Priya]
```

**Advantages:**

* Provides a natural and intuitive way to represent knowledge.
* Easily extendable and visually interpretable.

**Disadvantages:**

* Can become computationally expensive due to network traversal.
* Lacks standardization for defining relationships.
* Does not inherently support quantifiers such as "for all" or "some."

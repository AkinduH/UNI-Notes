### **Decomposition**

Decomposition involves breaking a relation (table) into smaller relations to improve database normalization and avoid redundancy or anomalies like the **repetition-of-information problem**.


### **Lossy Decomposition**

A decomposition is lossy if the original relation cannot be reconstructed from its decomposed relations due to insufficient or ambiguous information.

### **Lossless Decomposition**

A decomposition is **lossless** if the original relation can be reconstructed exactly from the decomposed relations.

![[Pasted image 20241124173619.png]]

The **common attributes** between R1 and R2 (denoted as R1​∩R2​) should form a **superkey** in at least one of the decomposed relations (R1​ or R2​).


### **First Normal Form (1NF)**

The **First Normal Form (1NF)** is a fundamental property in relational database design, ensuring that all attributes in a relation have atomic (indivisible) values.

Non-atomic values lead to redundancy, storage inefficiency, and increased complexity.


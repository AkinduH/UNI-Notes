
**Models of Computation & Languages**

Computational models are abstract machines that recognize languages of varying complexity, forming a hierarchy of language types. Different types of abstract machines correspond to different levels of this hierarchy.

### Types of Abstract Machines:

1. **Finite Automata (FA)**
2. **Pushdown Automata (PDA)**
3. **Linear-Bounded Automata (LBA)**
4. **Turing Machines (TM)**

---

### **The Chomsky Hierarchy**

The Chomsky Hierarchy classifies languages based on their complexity and the computational power required to recognize them. It consists of four levels:

|Type|Abstract Machine|Language Class (Grammar)|
|---|---|---|
|3|Finite Automaton|Regular|
|2|Pushdown Automaton|Context-Free|
|1|Linear-Bounded Automaton|Context-Sensitive|
|0|Turing Machine|Recursively Enumerable (Unrestricted, Phrase-Structure)|

---

### **Finite Automata (FA)**

- A **finite automaton** is a simple computational model known as a finite-state machine.
- **Limitation of Finite Automata:**
    - FA has little or no memory; it can only keep track of its current state.
    - This restricts it to recognizing only simple languages.
---

### **Pushdown Automata (PDA)**

- **Pushdown Automata extend FA by including a stack memory**:
    - Recognizes **context-free languages**.
    - Context-free grammars are important as they describe the syntax of programming languages.
    - The auxiliary stack provides additional memory to track nested structures.

---

### **Turing Machines (TM)**

- A **pushdown automaton** is still not a fully general computational model.
- **Turing Machine** is a general computing device capable of executing any step-by-step computational procedure.
- It can recognize more complex languages than FA and PDA.
- A **Turing machine** can perform any computation that a modern computer can.


### **CFL and CSL Comparison**

| Feature               | Context-Free (CFL) | Context-Sensitive (CSL)                                                                        |
| --------------------- | ------------------ | ---------------------------------------------------------------------------------------------- |
| **Automaton Type**    | Pushdown Automaton | Linear-Bounded Automaton                                                                       |
| **Nested Structures** | ✅ Yes              | ✅ Yes                                                                                          |
| **Memory**            | Stack              | Bounded Tape (More Memory)                                                                     |
| **Example Language**  | $a^n b^n$          | $a^n b^n c^n$                                                        ${ ww  \|  w ∈ (a, b)* }$ |
| **Complexity**        | Less Powerful      | More Powerful                                                                                  |

In summary:

- **CFLs** are useful for syntax parsing (like programming languages).
- **CSLs** can handle more complex constraints, like enforcing relationships between multiple symbols.


**Language:** { $a^n b^n$ | n ≥ 1 } (e.g., `ab`, `aabb`, `aaabbb`)

### **Step-by-step PDA Processing:**

1. **Push `a`s onto the stack**
    - Read `a` → Push onto stack
    - Example: For input `aaabbb`, stack after `aaa` looks like `aaa` (top of stack is `a`).
2. **Pop `a`s when `b`s arrive**
    - Read `b` → Pop an `a` from the stack.
    - If the number of `b`s matches the number of `a`s, the stack becomes empty at the end.
3. **Accept if stack is empty when input ends**






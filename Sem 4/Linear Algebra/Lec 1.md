### Definition of a Group \((G, *)\):
1. **Non-empty set (\(G \neq \emptyset\))**:  
   The set \(G\) must contain at least one element. The smallest group is the trivial group \(\{e\}\), where \(e\) is the identity element.

2. **Binary operation (\(*: G \times G \to G\))**:  
   The operation \(*\) takes two elements from \(G\) and returns another element in \(G\). This ensures **closure** (property 3), meaning \(a * b \in G\) for all \(a, b \in G\). While closure is technically implied by the definition of a binary operation, it is often explicitly stated for emphasis.

3. **Associativity**:  
   For all \(a, b, c \in G\), \((a * b) * c = a * (b * c)\). This property allows flexibility in grouping elements during computation but does *not* require the operation to be commutative (i.e., \(a * b = b * a\) is not guaranteed).

4. **Identity Element (\(e\))**:  
   There exists an element \(e \in G\) such that for all \(a \in G\), \(e * a = a * e = a\). This element is **unique** (proven using group axioms).

5. **Inverses**:  
   For every \(a \in G\), there exists an inverse \(\overline{a} \in G\) such that \(a * \overline{a} = \overline{a} * a = e\). Each inverse is also **unique**.


- **Special Cases**:  
  - **Abelian Groups**: Groups where the operation is commutative (\(a * b = b * a\)).  
  - **Non-Abelian Groups**: Groups where commutativity does not hold (e.g., matrix multiplication).  

## Example 1
![[Pasted image 20250201192648.png]]
![[Pasted image 20250201192715.png]]
![[Pasted image 20250201192748.png]]
![[Pasted image 20250201192808.png]]
![[Pasted image 20250201193138.png]]

## Example 2
![[Pasted image 20250201214213.png]]
![[Pasted image 20250201214236.png]]
![[Pasted image 20250201214257.png]]
![[Pasted image 20250201214319.png]]
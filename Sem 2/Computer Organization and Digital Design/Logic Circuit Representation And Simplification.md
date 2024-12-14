
Logic circuits are a abstraction for transistor circuits.

#### The “Buffer” Gate

- Two inverter, or NOT, gates connected in “series” so as to invert, then re-invert, a binary bit perform the function of a buffer. Buffer gates merely serve the purpose of signal amplification: taking a “weak” signal source that isn’t capable of sourcing or sinking much current, and boosting the current capacity of the signal so as to be able to drive a load.


What is a logic family?

Digital Integrated circuits are produced using several different circuit configurations and production technologies. Each such approach is called a specific logic family.

Definition: A logic family is a collection of different integrated circuit chips that have similar input, output, and internal circuit characteristics, but they perform different logic gate functions such as AND, OR, NOT, etc.

The idea is that different logic gate functions which belongs to the same logic family, will have identical electrical characteristics (electrically compatible with each other).

These families may vary by speed, power consumption, cost, voltage and current levels.

#### Boolean expressions 

![[Pasted image 20240302132425.png]]


![[Pasted image 20240302132538.png]]


![[Pasted image 20240302132616.png]]

(A+BB') = (A+B).(A+B')

(A+B).(A'+B') = (A'B)+(AB')

#### Karnaugh Maps

 ![[Pasted image 20240302134248.png]]



![[Pasted image 20240310214007.png]]



Depending on the number of variables in your boolean function, sketch an  map. The map could be 1 variable vs 1, or 1 vs 2, or 2 vs 2, etc.


Rules for grouping cells containing 1s

1. Groups must only contain 1 or X (don’t care term)
2. Each group must contain 2^n number of cells (i.e 1, 2, 4, 8, 16, …)
3. Each cell containing a 1 should be at least in one group
4. Groups may be horizontal or vertical, but not diagonal
5. Each group should be as large as possible
6. There should be as few groups as possible
7. Groups may overlap
8. Groups may wrap around the table. E.g: a. The leftmost cell in a row can be grouped with the rightmost cell in the same row b. Top cell of a column may be grouped with the bottom cell of the same column


![[Pasted image 20240310214032.png]]



[[Standard Forms & Canonical Forms]]
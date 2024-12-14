
**1. Big O, O** 
Big O notation is used to describe the upper bound of an algorithm’s running time. It measures the worst-case time complexity. Formally, a function f(n) is in the set O(g(n)) if, for any positive constant c, and for sufficiently large n, there exists a constant n₀ such that:

$$
0≤f(n)≤ c⋅g(n) for all n>n0​
$$

In simpler terms, f(n) grows at most as fast as g(n). 
Example: If f(n) = 4n³ + 10n² + 5n, then g(n) = n³.

**2. Little Oh, o** 
We use o-notation to denote an upper bound that is not asymptotically tight. Formally, a function f(n) is in the set o(g(n)) if, for any positive constant c, there exists a value n₀ such that:

$$
0≤f(n)< c⋅g(n) for all n>n0​
$$

In simpler terms, f(n) grows much slower than g(n). 
Example: If f(n) = 4n³ + 10n² + 5n, then g(n) = n⁴.

**3. Big Omega, Ω** 
The notation Ω(n) is the formal way to express the lower bound of an algorithm’s running time. It measures the best case time complexity. Formally, a function f(n) is in the set Ω(g(n)) if, for any positive constant c, and for sufficiently large n, there exists a constant n₀ such that:

$$
0≤c⋅g(n)≤f(n)for all n>n0​
$$

In simpler terms, f(n) grows at least as fast as g(n). 
Example: If f(n) = 4n³ + 10n² + 5n, then g(n) = n³.

**4. Little Omega, ω** 
We use ω-notation to denote a lower bound that is not asymptotically tight. Formally, a function f(n) is in the set ω(g(n)) if, for any positive constant c, and for sufficiently large n, there exists a constant n₀ such that:

$$
0≤c⋅g(n)<f(n)for all n>n0​
$$

In simpler terms, f(n) grows much faster than g(n). 
Example: If f(n) = 4n³ + 10n² + 5n, then g(n) = n².

**5.Theta, Θ**
Theta notation is used to describe both the lower bound and the upper bound of an algorithm’s running time. It measures the average-case time complexity. 

Formally, a function f(n) is in the set Θ(g(n)) if and only if f(n) is in both O(g(n)) and Ω(g(n)). 

In simpler terms, f(n) grows at the same rate as g(n). 
Example: If f(n) = 4n³ + 10n² + 5n, then g(n) = n³.

## Relations Between O,Ω,Θ

![[Pasted image 20240507123816.png]]

[[Sorting Algos]]


# Order of Convergence in numerical methods

$$
|ϵ(n+1)|=k|ϵ(n)|^p
$$
where:
- k is a constant,
- p is the order of convergence,

1. p = 1 (Linear convergence):
    - The error is reduced by a constant factor in each iteration.
    - Examples: Bisection method, False Position method.
2. p = 2 (Quadratic convergence):
    - The error is approximately squared in each iteration.
    - Examples: Newton-Raphson method, Secant method.
3. p > 2 (Higher-order convergence):
    - The error is reduced at a rate faster than quadratic convergence.
    - Examples: Higher-order Newton methods, Halley's method (p = 3).

When diverging p doesn't exist.


we need to use the formula for the number of iterations in the bisection method, which is given by:

$$
n=log((b-a)/tolerance​)/(log(2))​
$$

where `a` and `b` are the initial interval boundaries and `tolerance` is the desired accuracy.



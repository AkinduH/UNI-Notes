
### Optimization with Local Search

- Ideal for optimization problems that aim to maximize or minimize an objective function F(x).
- F(x): objective function, where x is often a vector of values.
- Begins with a complete configuration.
- Successor of a state S: a state with a single element modified.
- Moves from the current state to a neighboring state.
- Requires low memory as it doesn’t maintain a search tree or graph.


---

### 1. **Stochastic Hill Climbing**

- Chooses an "uphill" move at random.
- Probability of selection varies with the steepness of the move.
- Slower convergence than steepest ascent.
- Can find better solutions in rugged state landscapes by avoiding some local maxima.

### 2. **First-Choice Hill Climbing**

- Randomly generates successors until one improves over the current state.
- Useful when a state has many (e.g., thousands) of successors.
- Reduces computation by not evaluating all successors.

### 3. **Random-Restart Hill Climbing**

- Starts from random positions multiple times.
- Stops when a goal is found; saves the best result across all searches.
- Completeness approaches 1 if all states can be generated.
- Effective in avoiding local maxima and finding global optima through multiple retries.
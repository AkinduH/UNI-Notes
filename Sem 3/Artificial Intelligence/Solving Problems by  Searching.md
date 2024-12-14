
**Problem-Solving Agents**: 
These agents plan by considering a sequence of actions to achieve a goal state, primarily using search algorithms.
Use atomic representations.
Only the simplest environments: episodic, single agent, fully observable, deterministic, static, and discrete.

**What is a search problem?** 
A search problem is a task where an agent needs to find a path from a starting point to a goal. Think of it like trying to navigate a maze.

**Key components of a search problem:**

- **State space:** All the possible places the agent can be.
- **Initial state:** Where the agent starts.
- **Goal state:** Where the agent wants to reach.
- **Actions:** What the agent can do to move from one state to another.
- **Transition model:** How actions change the current state.
- **Action cost:** The "price" of performing an action.

**Example:** Imagine you're lost in a city and want to find the nearest restaurant.

- **State space:** All the streets and intersections in the city.
- **Initial state:** Your current location.
- **Goal state:** A restaurant.
- **Actions:** Walking, turning left or right, taking a bus.
- **Transition model:** Walking to the next block, turning a corner, boarding a bus.
- **Action cost:** Time taken or distance traveled.

**Finding a solution:** The goal is to find a sequence of actions (a path) that takes the agent from the initial state to the goal state. The best path is often the one with the lowest cost.

**In essence,** a search problem is a puzzle where you need to figure out the best way to get from point A to point B.


### Uninformed Search Algorithms

These algorithms have no additional information about the problem domain. Examples include:

![[Pasted image 20241111162503.png]]

**Key Takeaways:**
- **BFS** is suitable for problems with uniform action costs and where the solution is relatively shallow.
- **Uniform-cost search** is suitable for problems with non-uniform action costs and where finding the optimal solution is crucial.
- **DFS** is suitable for problems with deep, narrow state spaces and where finding any solution is acceptable.
- **Depth-limited search** can be useful for controlling the search depth and reducing memory usage.

### Informed Search Algorithms

![[Pasted image 20241111204202.png]]

![[Pasted image 20241111204421.png]]

### Heuristic Functions: Guiding the Search

**Understanding Heuristics**

Heuristic functions are like guides or estimates that help search algorithms make informed decisions. They provide a "best guess" of how close a given state is to the goal. A good heuristic can significantly improve the efficiency of a search algorithm.

**Methods for Constructing Heuristics:**

1. **Relaxing the Problem:**
    
    - Simplify the problem to make it easier to solve.
    - The solution to the simplified problem can provide a lower bound on the cost of the original problem.
2. **Pattern Databases:**
    
    - Pre-calculate the optimal solution costs for specific subproblems.
    - Use these precomputed values to estimate the cost of reaching the goal from a given state.
3. **Landmarks:**
    
    - Identify important intermediate states (landmarks) on the path to the goal.
    - Estimate the cost based on the distance to the nearest landmark.
4. **Learning from Experience:**
    
    - Analyze past solutions to the problem to identify patterns and relationships.
    - Use these patterns to create a heuristic function that estimates the cost based on similar situations.

[[Local Search and  Optimization Problems]]
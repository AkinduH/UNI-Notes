
## **Brute Force Algorithms**

Brute force algorithms consider all possible solutions, one by one, and select the optimal solution. Also known as exhaustive search method.

- straightforward, usually follows the problem definition
- rarely the efficient approach
- might be suitable only for smaller inputs

For example, the time complexity for the Traveling Salesman Problem using brute force is O(n!), and for the Knapsack Problem, it’s O(2^n).

##### Pros:

- The brute force approach is a guaranteed way to find the correct solution by listing all the possible candidate solutions for the problem.
- It is a generic method and not limited to any specific domain of problems.
- The brute force method is ideal for solving small and simpler problems.
- It is known for its simplicity and can serve as a comparison benchmark.

##### Cons:

- The brute force approach is inefficient. For real-time problems, algorithm analysis often goes above the _**O(N!)***_ order of growth.
- This method relies more on compromising the power of a computer system for solving a problem than on a good algorithm design.
- Brute force algorithms are slow.
- Brute force algorithms are not constructive or creative compared to algorithms that are constructed using some other design paradigms

<div style="page-break-after: always;"></div>

## **Dynamic Programming**

***Dynamic Programming*** is a method used in mathematics and computer science to solve complex problems by breaking them down into simpler subproblems. By solving each subproblem only once and storing the results, it avoids redundant computations, leading to more efficient solutions for a wide range of problems.

##### How Does Dynamic Programming (DP) Work?

- ***Identify Subproblems:*** Divide the main problem into smaller, independent subproblems.
- ***Store Solutions:*** Solve each subproblem and store the solution in a table or array.
- ***Build Up Solutions:*** Use the stored solutions to build up the solution to the main problem.
- ***Avoid Redundancy:*** By storing solutions, DP ensures that each subproblem is solved only once, reducing computation time.

If the subproblems share resources and are not independent, then dynamic programming may not provide the correct solution

Dynamic programming can be achieved using two approaches:

##### **1. Top-Down Approach (Memoization):**

In the top-down approach, also known as **memoization***, we start with the final solution and recursively break it down into smaller subproblems. To avoid redundant calculations, we store the results of solved subproblems in a memoization table.

Let’s breakdown Top down approach:

- Starts with the final solution and recursively breaks it down into smaller subproblems.
- Stores the solutions to subproblems in a table to avoid redundant calculations.
- Suitable when the number of subproblems is large and many of them are reused.

Memoization is typically implemented using recursion and is well-suited for problems that have a relatively small set of inputs.

##### **2. Bottom-Up Approach (Tabulation):**

In the bottom-up approach, also known as **tabulation**, we start with the smallest subproblems and gradually build up to the final solution. We store the results of solved subproblems in a table to avoid redundant calculations.

Let’s breakdown Bottom-up approach:

- Starts with the smallest subproblems and gradually builds up to the final solution.
- Fills a table with solutions to subproblems in a bottom-up manner.
- Suitable when the number of subproblems is small and the optimal solution can be directly computed from the solutions to smaller subproblems.

Tabulation is typically implemented using iteration and is well-suited for problems that have a large set of inputs.

<div style="page-break-after: always;"></div>

|                         | **  Memoization**                                                                                                                                                             | **Tabulation**                                                                                                                                                    |
| ----------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Approach**            | Top-down                                                                                                                                                                      | Bottom-up                                                                                                                                                         |
| **Implementation**      | Recursive                                                                                                                                                                     | Iterative                                                                                                                                                         |
| **Storage**             | Caches the results of function calls                                                                                                                                          | Stores the results of subproblems in a table                                                                                                                      |
| **Problem Suitability** | Well-suited for problems with a relatively small set of inputs                                                                                                                | Well-suited for problems with a large set of inputs                                                                                                               |
| **Usage**               | Used when the subproblems have overlapping subproblems                                                                                                                        | Used when the subproblems do not overlap                                                                                                                          |
| **Overhead**            | can have additional overhead due to the cost of recursion, such as the cost of function call stack maintenance.                                                               | overhead does not exist in this method which uses iteration.                                                                                                      |
| **Time complexity**     | same because they solve the same number of subproblems.                                                                                                                       | same because they solve the same number of subproblems.                                                                                                           |
| **State**               | State transition relation is easy to think                                                                                                                                    | State transition relation is difficult to think                                                                                                                   |
| **Code**                | Code is easy and less complicated                                                                                                                                             | Code gets complicated when lot of conditions are required                                                                                                         |
| **Speed**               | Slow due to lot of recursive calls and return statements                                                                                                                      | Fast, as we directly access previous states from the table                                                                                                        |
| **Subproblem solving**  | If some subproblems in the subproblem space need not be solved at all, the memoized solution has the advantage of solving only those subproblems that are definitely required | If all subproblems must be solved at least once, a bottom-up dynamic-programming algorithm usually outperforms a top-down memoized algorithm by a constant factor |
| **Table Entries**       | In Memoized version, all entries of the lookup table are not necessarily filled. The table is filled on demand.                                                               | In Tabulated version, starting from the first entry, all entries are filled one by one                                                                            |

##### Memoization implementation of Fib(n):

```c++
#include <iostream>
#include <unordered_map>

int fibonacci(int n, std::unordered_map<int, int>& cache) {
	if (cache.find(n) != cache.end()) {
		return cache[n];
	}
	int result;
	if (n == 0) {
		result = 0;
	} else if (n == 1) {
		result = 1;
	} else {
		result = fibonacci(n-1, cache) + fibonacci(n-2, cache);
	}
	cache[n] = result;
	return result;
}
```


##### Tabulation implementation of Fib(n):

```c++
#include <iostream>
#include <vector>

int fibonacci(int n)
{
	if (n == 0) {
		return 0;
	}
	else if (n == 1) {
		return 1;
	}
	else {
		std::vector<int> table(n + 1, 0);
		table[0] = 0;
		table[1] = 1;
		for (int i = 2; i <= n; i++) {
			table[i] = table[i - 1] + table[i - 2];
		}
		return table[n];
	}
}
```

<div style="page-break-after: always;"></div>


## **Greedy Algorithms**


A **greedy algorithm** is a type of optimization algorithm that makes locally optimal choices at each step with the goal of finding a globally optimal solution. It operates on the principle of “taking the best option now” without considering the long-term consequences.


### Steps for Creating a Greedy Algorithm:

1. ***Define the problem:*** Clearly state the problem to be solved and the objective to be optimized.
2. ***Identify the greedy choice***: Determine the locally optimal choice at each step based on the current state.
3. ***Make the greedy choice:*** Select the greedy choice and update the current state.
4. ***Repeat:*** Continue making greedy choices until a solution is reached.

##### **Standard Greedy Algorithms :**
- Prim’s Algorithm
- Kruskal’s Algorithm
- Dijkstra’s Algorithm

***Greedy algorithms may not always find the best possible solution.***
***To use Greedy algorithm the problem must not contain overlapping subproblems.***

A problem that can be solved using the Greedy approach follows the below-mentioned properties:
- Optimal substructure property. 
- Minimization or Maximization of quantity is required.
- Ordered data is available such as data on increasing profit, decreasing cost, etc.
- Non-overlapping subproblems.

## **Applications of Greedy Approach:**

Greedy algorithms are used to find an optimal or near optimal solution to many real-life problems. Few of them are listed below:

(1) Make a change problem
(2) Knapsack problem/Knapsack with fractions
(3) Minimum spanning tree
(4) Single source shortest path
(5) Activity selection problem 
(6) Job sequencing problem
(7) Huffman code generation.
(8) Job scheduling
(9) Interval scheduling
(10) Greedy set cover

| Feature         | Greedy Algorithm                                                                                                          | Dynamic Programming                                                                                                                                 |
| --------------- | ------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------- |
| Approach        | Makes the locally optimal choice at each stage with the hope of finding a global optimum.                                 | Breaks down the problem into smaller subproblems and solves each subproblem once, storing the solutions in a table to avoid redundant computations. |
| Optimality      | May not always guarantee an optimal solution, as it makes decisions based on local optimality.                            | Guarantees an optimal solution by considering all possible cases.                                                                                   |
| Recursion       | Follows a problem-solving heuristic of making the locally optimal choice at each stage.                                   | Usually based on a recurrent formula that uses previously calculated states.                                                                        |
| Memoization     | More efficient in terms of memory as it doesn't need to store previous choices.                                           | Requires a dynamic programming table for memoization, which increases the memory complexity.                                                        |
| Time Complexity | Generally faster, e.g., Dijkstra's algorithm has a time complexity of O(E log V + V log V).                               | Generally slower, e.g., Bellman-Ford algorithm has a time complexity of O(VE).                                                                      |
| Computation     | Computes its solution by making its choices in a serial forward fashion, never looking back or revising previous choices. | Computes its solution bottom-up or top-down by synthesizing them from smaller optimal sub-solutions.                                                |
| Examples        | Fractional knapsack problem.                                                                                              | 0/1 knapsack problem.                                                                                                                               |
|                 |                                                                                                                           |                                                                                                                                                     |

<div style="page-break-after: always;"></div>

> [! ]
> ## 0/1 Knapsack problem using different approaches


1. **Brute Force Algorithm**:

```c++

int knapsack(vector<int>& weight, vector<int>& value, int capacity, int n) {
    if (n == 0 || capacity == 0) {
        return 0;
    }

    if (weight[n - 1] > capacity) {
        return knapsack(weight, value, capacity, n - 1);
    } else {
        return max(value[n - 1] + knapsack(weight, value,
        capacity - weight[n - 1], n - 1),
        knapsack(weight, value, capacity, n - 1));
    }
}

knapsack(weight, value, capacity, n)
```

<div style="page-break-after: always;"></div>


2. **Dynamic Programming (Top-down Approach)**:

```c++
int knapsack(vector<int>& weight, vector<int>& value, int capacity, int n, vector<vector<int>>& dp) {
    if (n == 0 || capacity == 0) {
        return 0;
    }

    if (dp[n][capacity] != -1) {
        return dp[n][capacity];
    }

    if (weight[n - 1] > capacity) {
        dp[n][capacity] = knapsack(weight, value, capacity, n - 1,dp);
        
    } else {
        dp[n][capacity] = max(value[n - 1] + knapsack(weight, value, capacity - weight[n - 1], n - 1, dp),knapsack(weight, value, capacity, n - 1, dp));
        
    }

    return dp[n][capacity];
}


vector<vector<int>> dp(n + 1, vector<int>(capacity + 1, -1));
knapsack(weight, value, capacity, n, dp)
```

<div style="page-break-after: always;"></div>


3. **Dynamic Programming (Bottom-up Approach)**:

```c++

int knapsack(vector<int>& weight, vector<int>& value, int capacity, int n) {
    vector<vector<int>> dp(n + 1, vector<int>(capacity + 1, 0));

    for (int i = 1; i <= n; i++) {
        for (int j = 1; j <= capacity; j++) {
            if (weight[i - 1] > j) {
                dp[i][j] = dp[i - 1][j];
            } else {
                dp[i][j] = max(value[i - 1] + dp[i - 1][j - weight[i - 1]], dp[i - 1][j]);
            }
        }
    }

    return dp[n][capacity];
}

knapsack(weight, value, capacity, n)
```

<div style="page-break-after: always;"></div>


4. **Greedy Algorithm**:

```c++

int knapsack(vector<int>& weight, vector<int>& value, int capacity) {
    int n = weight.size();
    vector<pair<double, int>> items(n);

    // Calculate the value-to-weight ratio for each item
    for (int i = 0; i < n; i++) {
        items[i] = make_pair((double)value[i] / weight[i], i);
    }

    // Sort the items in decreasing order of value-to-weight ratio
    sort(items.begin(), items.end(), greater<>());

    int totalValue = 0;
    int currentWeight = 0;

    // Greedily add items to the knapsack
    for (auto& item : items) {
        int index = item.second;
        if (currentWeight + weight[index] <= capacity) {
            currentWeight += weight[index];
            totalValue += value[index];
        } else {
            break;
        }
    }

    return totalValue;
}

knapsack(weight, value, capacity) 
```


<div style="page-break-after: always;"></div>


### descriptions and differences of the four algorithms:

|                                          | Description                                                                                                                                                                                                                                                 | Difference                                                                                                                                                                                                                                                                                                                    |
| ---------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Brute Force Algorithm                    | The brute force approach tries all possible combinations of items to be included in the knapsack and selects the combination that provides the maximum total value without exceeding the knapsack capacity.                                                 | The brute force algorithm has an exponential time complexity $O(2^n)$, as it needs to consider all possible combinations of items. This makes it inefficient for large problem instances.                                                                                                                                     |
| Dynamic Programming (Top-down Approach)  | The top-down dynamic programming approach solves the problem by recursively breaking it down into smaller subproblems and storing the solutions to avoid redundant computations. It uses memoization to store the results of previously solved subproblems. | The top-down dynamic programming approach has a better time complexity of $O(n*capacity)$ compared to the brute force method, as it avoids redundant computations by storing intermediate results. However, it still requires additional memory to store the memoization table.                                               |
| Dynamic Programming (Bottom-up Approach) | The bottom-up dynamic programming approach builds a table of solutions, starting from the base cases and gradually filling in the table to find the optimal solution.                                                                                       | The bottom-up dynamic programming approach has the same time complexity $O(n*capacity)$ as the top-down approach, but it doesn't require the additional memory for the recursive call stack. Instead, it uses a 2D table to store the intermediate results.                                                                   |
| Greedy Algorithm                         | The greedy algorithm for the 0/1 Knapsack problem follows a heuristic of selecting the item with the highest value-to-weight ratio and adding it to the knapsack as long as the capacity is not exceeded.                                                   | The greedy algorithm has a much lower time complexity $O(n log n)$ compared to the dynamic programming approaches, as it only needs to sort the items and then iterate through them. However, it does not guarantee an optimal solution, as the locally optimal choices made at each step may not lead to the global optimum. |




## Sorting Algos

| Sorting Algorithm | Best Case Time | Average Time | Worst Case Time | Space Complexity | Stability | In-Place Sorting |
| ----------------- | -------------- | ------------ | --------------- | ---------------- | --------- | ---------------- |
| Bubble Sort       | O(n)           | O(n²)        | O(n²)           | O(1)             | Yes       | Yes              |
| Insertion Sort    | O(n)           | O(n²)        | O(n²)           | O(1)             | Yes       | Yes              |
| Selection Sort    | O(n²)          | O(n²)        | O(n²)           | O(1)             | No        | Yes              |
| Merge Sort        | O(n log n)     | O(n log n)   | O(n log n)      | O(n)             | Yes       | No               |
| Quick Sort        | O(n log n)     | O(n log n)   | O(n²)           | O(log n)         | No        | Yes              |

## Linked List

| Operation                   | Time Complexity (Singly Linked List) | Time Complexity (Doubly Linked List) | Space Complexity |
| --------------------------- | ------------------------------------ | ------------------------------------ | ---------------- |
| Accessing by Index          | O(n)                                 | O(n)                                 | O(1)             |
| Insertion at Beginning      | O(1)                                 | O(1)                                 | O(1)             |
| Insertion at End            | O(n)                                 | O(1)                                 | O(1)             |
| Insertion at Given Position | O(n)                                 | O(n)                                 | O(1)             |
| Deletion at Beginning       | O(1)                                 | O(1)                                 | O(1)             |
| Deletion at End             | O(n)                                 | O(1)                                 | O(1)             |
| Deletion at Given Position  | O(n)                                 | O(n)                                 | O(1)             |
| Searching                   | O(n)                                 | O(n)                                 | O(1)             |

## Stack

| **Operations** | **Complexity** |
| -------------- | -------------- |
| push()         | O(1)           |
| pop()          | O(1)           |
| isEmpty()      | O(1)           |
| size()         | O(1)           |

## Queue

| **Operations** | **Complexity** |
| -------------- | -------------- |
| enqueue()      | O(1)           |
| dequeue()      | O(1)           |
| isEmpty()      | O(1)           |
| size()         | O(1)           |

## Priority Queue

| **Operations** | **Complexity**                |
| -------------- | ----------------------------- |
| insert()       | O(log n) (with a binary heap) |
| remove()       | O(log n) (with a binary heap) |
| peek()         | O(1)                          |
| isEmpty()      | O(1)                          |
| size()         | O(1)                          |


## Binary Search Tree (BST)

| Operation             | Time Complexity (Worst Case) | Time Complexity (Balanced BST) |
| --------------------- | ---------------------------- | ------------------------------ |
| Search                | O(h)                         | O(log n)                       |
| Insertion             | O(h)                         | O(log n)                       |
| Deletion              | O(h)                         | O(log n)                       |
| FindMin/FindMax       | O(h)                         | O(log n)                       |
| Successor/Predecessor | O(h)                         | O(log n)                       |

Here, h is the height of the tree, and n is the number of nodes in the tree. In a balanced BST, the height h is $O(log⁡n)$, while in the worst case (a skewed tree), the height h is $O(n)$.


## Heap

|Operation|Time Complexity|
|---|---|
|Heapify|O(log n)|
|Build-Heap|O(n)|
|Heap-Increase-Key|O(log n)|
|Heap-Insert|O(log n)|
|Heap-Extract-Max|O(log n)|
|Heap-Peek|O(1)|

## Hash Table

| Hash Table Type           | Insertion (Avg/Worst) | Search (Avg/Worst)  | Deletion (Avg/Worst)          |
| ------------------------- | --------------------- | ------------------- | ----------------------------- |
| Basic (Ideal)             | $O(1)$                | $O(1)$              | $O(1)$                        |
| Chaining (Linked Lists)   | $O(1)$   /  $O(n)$    | $O(1+α)$  /  $O(n)$ | $O(1+α)$ / $O(n$)             |
| Open Addressing (Probing) | $O(1/(1-α)) / O(n)$   | $O(1/(1-α)) / O(n)$ | $O(1/(1-α))$ / $O(n)$<br><br> |

## Graph

| Criteria         | Adjacency List                                                                                                                                                                                         | Adjacency Matrix                                                                                                           |
| ---------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | -------------------------------------------------------------------------------------------------------------------------- |
| Space Complexity | $O(V+E)$                                                                                                                                                                                               | $O(V^2)$                                                                                                                   |
| Time Complexity  | To list of adjacent elements of u takes $O(degree(u))$ time. Therefore to check if an edge (u,v) is present take $O(degree(u))$ time and $O(V)$ is the worst case, where V is the set of all vertices. | To list all adjacent elements of u takes O(V) time, but to check if an edge (u,v) is present takes constant time $(O(1))$. |

## BFS & DFS

|                  | Breadth-First Search (BFS) | Depth-First Search (DFS) |
| ---------------- | -------------------------- | ------------------------ |
| Time Complexity  | O(V+E)                     | O(V+E)                   |
| Space Complexity | O(V)                       | O(V)                     |

## MST

| Operation / Algorithm            | Time Complexity |
| -------------------------------- | --------------- |
| **Kruskal’s Algorithm**          | $O(Elog⁡E)$     |
| **Prim’s Algorithm**             | $O((V+E)log⁡V)$ |
| **Prim’s with Adjacency Matrix** | $O(V^2)$        |

## SSSP

| Feature              | Dijkstra’s Algorithm                                    | Bellman-Ford Algorithm                    |
| -------------------- | ------------------------------------------------------- | ----------------------------------------- |
| **Time Complexity**  | $O(V^2)$ for dense graph, $O(E log V)$ for sparse graph | $O(E)$    (Best)          $O(VE)$   (Avg) |
| **Space Complexity** | $O(V)$                                                  | $O(V)$                                    |

## 0/1 Knapsack problem 

| Algorithm                           | Time Complexity   |
| ----------------------------------- | ----------------- |
| **Brute Force Algorithm**           | $O(2^n)$          |
| **Dynamic Programming (Top-down)**  | $O(n * capacity)$ |
| **Dynamic Programming (Bottom-up)** | $O(n * capacity)$ |
| **Greedy Algorithm**                | $O(n log n)$      |

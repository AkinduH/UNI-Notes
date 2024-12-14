A graph data structure is a collection of nodes that have data and are connected to other nodes.

More precisely, a graph is a data structure (V, E) that consists of

- A collection of vertices V
- A collection of edges E, represented as ordered pairs of vertices (u,v)
- |E| ≤ |V|^2

![[Pasted image 20240408125753.png]]

Graphs can be categorized in various different ways. One such categorization is as follows;

- **Directed Graphs** - Here the edges are directed
- **Undirected Graphs** - Here the edges are not directed
- **Weighted Graphs** - Weights are associated to edges

## Graph Terminology

| Term               | Definition                                                                                                                                                                                                                                         |
| ------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Degree             | Number of edges connected to a vertex                                                                                                                                                                                                              |
| In-degree          | Number of edges coming into a vertex in a directed graph                                                                                                                                                                                           |
| Out-degree         | Number of edges going out of a vertex in a directed graph                                                                                                                                                                                          |
| Simple path        | Path between two vertices are simply if no vertex is repeated.                                                                                                                                                                                     |
| Path cost          | The cummulative sum of all the weights of the edges in a path in a weighted graph                                                                                                                                                                  |
| Reachability       | A vertex U is reachable from V if there is a path from U to V                                                                                                                                                                                      |
| Length of a Path   | Number of edges in the path                                                                                                                                                                                                                        |
| Subgraph           | A subset of the vertices of a graph                                                                                                                                                                                                                |
| Dense Graph        | A graph is dense if the number of edges (\|E\|) is nearly equal to \|V\|2                                                                                               (\|V\| (\|V\| - 1) / 2 - Undirected   and   \|V\| (\|V\| - 1) - Directed ) |
| Sparse Graph       | A graph is sparse if the number of edges \|E\| << \|V\|2                                                                                                                                                                                           |
| Strongly Connected | There is a path between every pair of vertices. Therefore \|E\| is greater than or equal to \|V\| - 1                                                                                                                                              |

## Graph Representation

### 1. Adjacency Matrix

An adjacency matrix is a 2D array of V x V vertices. Each row and column represent a vertex.
If the value of any element `a[i][j]` is 1, it represents that there is an edge connecting vertex i and vertex j.

![[Pasted image 20240408130233.png]]

### 2. Adjacency List

An adjacency list represents a graph as an array of linked lists.
The index of the array represents a vertex and each element in its linked list represents the other vertices that form an edge with the vertex.

![[Pasted image 20240408131855.png]]

**Space and Time Complexities of Adjacency Lists and Matrices**

| Criteria         | Adjacency List                                                                                                                                                                                   | Adjacency Matrix                                                                                                         |
| ---------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------ |
| Space Complexity | O(V+E), therefore space efficient for sparse graphs.                                                                                                                                             | O(V^2), therefore not space efficient, especially for sparse graphs.                                                     |
| Time Complexity  | To list of adjacent elements of u takes O(degree(u)) time. Therefore to check if an edge (u,v) is present take O(degree(u)) time and O(V) is the worst case, where V is the set of all vertices. | To list all adjacent elements of u takes O(V) time, but to check if an edge (u,v) is present takes constant time (O(1)). |


```c++
    vector<vector<int>> adjecent_list(n);
    for (const auto& edge : edges) {
        adjecent_list[edge[0] - 1].push_back(edge[1] - 1);
        adjecent_list[edge[1] - 1].push_back(edge[0] - 1);
    }
```

[[BFS and DFS]]
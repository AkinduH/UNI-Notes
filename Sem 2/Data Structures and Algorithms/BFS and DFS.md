
> [! ]
> ## Breadth First Search

***Breadth First Search (BFS)*** is a fundamental graph traversal algorithm. It involves visiting all the connected nodes of a graph in a level-by-level manner.

A standard BFS implementation puts each vertex of the graph into one of two categories:

1. Visited
2. Not Visited

Let’s discuss the algorithm for the BFS:

1. ***Initialization:*** Enqueue the starting node into a queue and mark it as visited.
2. ***Exploration:*** While the queue is not empty:
    - Dequeue a node from the queue and visit it (e.g., print its value).
    - For each unvisited neighbor of the dequeued node:
        - Enqueue the neighbor into the queue.
        - Mark the neighbor as visited.
3. ***Termination:*** Repeat step 2 until the queue is empty.


#### BFS pseudocode

```pueudocode
create a queue Q 
mark v as visited and put v into Q 
while Q is non-empty 
    remove the head u of Q 
    mark and enqueue all (unvisited) neighbours of u
```

#### Implementation of BFS 

```c++
void bfs(vector<vector<int> >& adjList, int startNode)
{
    // Mark all the vertices as not visited
    vector<bool> visited(vertices,false);
	// Create a queue for BFS
	queue<int> q;

	// Mark the current node as visited and enqueue it
	visited[startNode] = true;
	q.push(startNode);

	// Iterate over the queue
	while (!q.empty()) {
		// Dequeue a vertex from queue and print it
		int currentNode = q.front();
		q.pop();

		// Get all adjacent vertices of the dequeued vertex
		// currentNode If an adjacent has not been visited,
		// then mark it visited and enqueue it
		for (int neighbor : adjList[currentNode]) {
			if (!visited[neighbor]) {
				visited[neighbor] = true;
				q.push(neighbor);
			}
		}
	}
}
```

#### BFS Algorithm Complexity

The time complexity of the BFS  is represented in the form of `O(V + E)`, where V is the number of nodes and E is the number of edges.

The space complexity of the algorithm is `O(V)`.


> [! ]
> ## Depth First Search

A standard DFS implementation puts each vertex of the graph into one of two categories:

1. Visited
2. Not Visited

The purpose of the algorithm is to mark each vertex as visited while avoiding cycles.

The DFS algorithm works as follows:

1. Start by putting any one of the graph's vertices on top of a stack.
2. Take the top item of the stack and add it to the visited list.
3. Create a list of that vertex's adjacent nodes. Add the ones which aren't in the visited list to the top of the stack.
4. Keep repeating steps 2 and 3 until the stack is empty.

```
DFS(graph, startVertex):
    visited = set()  
    stack = []      

    visited.add(startVertex)
    stack.append(startVertex)

    while stack:
        vertex = stack.pop()  

        for neighbor in graph[vertex]:
            if neighbor not in visited:
                visited.add(neighbor)
                stack.append(neighbor)

        # Optionally, process the vertex here 
        # (e.g., print it)
```

#### DFS Pseudocode (recursive implementation)

The pseudocode for DFS is shown below. In the init() function, notice that we run the DFS function on every node. This is because the graph might have two different disconnected parts so to make sure that we cover every vertex, we can also run the DFS algorithm on every node.

```psuedocode
DFS(G, u)
    u.visited = true
    for each v ∈ G.Adj[u]
        if v.visited == false
            DFS(G,v)
     
init() {
    For each u ∈ G
        u.visited = false
     For each u ∈ G
       DFS(G, u)
}
```

#### DFS Implementation

```c++
#include <bits/stdc++.h>
using namespace std;

// Graph class represents a directed graph
// using adjacency list representation
class Graph {
public:
	map<int, bool> visited;
	map<int, list<int> > adj;

	// Function to add an edge to graph
	void addEdge(int v, int w);

	// DFS traversal of the vertices
	// reachable from v
	void DFS(int v);
};

void Graph::addEdge(int v, int w)
{
	// Add w to v’s list.
	adj[v].push_back(w);
}

void Graph::DFS(int v)
{
	// Mark the current node as visited and
	// print it
	visited[v] = true;
	cout << v << " ";

	// Recur for all the vertices adjacent
	// to this vertex
	list<int>::iterator i;
	for (i = adj[v].begin(); i != adj[v].end(); ++i)
		if (!visited[*i])
			DFS(*i);
}

// Driver code
int main()
{
	// Create a graph given in the above diagram
	Graph g;
	g.addEdge(0, 1);
	g.addEdge(0, 2);
	g.addEdge(1, 2);
	g.addEdge(2, 0);
	g.addEdge(2, 3);
	g.addEdge(3, 3);

	cout << "Following is Depth First Traversal"
			" (starting from vertex 2) \n";

	// Function call
	g.DFS(2);

	return 0;
}
```

### Complexity of Depth First Search

The time complexity of the DFS algorithm is  in the form of `O(V + E)`, where `V` is the number of nodes and `E` is the number of edges.

The space complexity of the algorithm is `O(V)`.

---

| Difference                        | Breadth-First Search (BFS)                                                                                                                                 | Depth-First Search (DFS)                                                                                                                                                            |
| --------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Exploration Order                 | Explores the graph layer by layer, visiting all the nodes at the current depth before moving on to the next layer.                                         | Explores the graph as deeply as possible before backtracking. Follows a path to the end before considering other paths.                                                             |
| Data Structure Used               | Typically uses a queue to keep track of the nodes to visit next.                                                                                           | Typically uses a stack (either explicitly or implicitly through recursion) to keep track of the nodes to visit next.                                                                |
| Memory Usage                      | Requires more memory than DFS, as it needs to store all the nodes in the current layer during the traversal.                                               | Generally requires less memory than BFS, as it only needs to keep track of the current path and backtrack when necessary.                                                           |
| Shortest Path (unweighted graphs) | Guaranteed to find the shortest path between two nodes in an unweighted graph.                                                                             | Can be used to find the shortest path between two nodes in an unweighted graph, but it may not find the shortest path.                                                              |
| Shortest Path (weighted graphs)   | Can be used to find the shortest path between two nodes in a weighted graph, using algorithms like Dijkstra's algorithm.                                   | Not suitable for finding the shortest path in weighted graphs, as it may not explore the paths in the order of increasing weight.                                                   |
| Topological Sorting               | Not suitable for topological sorting, as it explores the graph layer by layer rather than following the directed edges.                                    | Can be used to perform topological sorting of a directed acyclic graph (DAG).                                                                                                       |
| Cycle Detection                   | Can also be used to detect cycles in a graph.                                                                                                              | Can be used to detect cycles in a graph.                                                                                                                                            |
| Connectivity Check                | Can be used to check if a graph is connected or to find the connected components of a graph.                                                               | Can also be used to check if a graph is connected or to find the connected components.                                                                                              |
| Time Complexity                   | O(V+E)                                                                                                                                                     | O(V+E)                                                                                                                                                                              |
| Space Complexity                  | O(V)                                                                                                                                                       | O(V)                                                                                                                                                                                |
| Applications                      | Useful for finding the shortest path between two nodes, breadth-limited search, and solving problems that require a more uniform exploration of the graph. | Useful for exploring the entire graph and finding all the nodes. Can be used for depth-limited search, edge classification, and solving problems that require an exhaustive search. |
**BFS Uses**

- Finding shortest paths in unweighted graphs
- Minimum spanning tree construction (e.g., Prim's algorithm)
- Connected component detection
- Peer-to-peer network exploration
- Web crawling
- Garbage collection (Cheney's algorithm)
- Broadcasting in networks
- Ford-Fulkerson algorithm (network flow)

**DFS Uses**

- Finding connected components
- Topological sorting
- Cycle detection
- Finding strongly connected components (e.g., Tarjan's or Kosaraju's algorithm)
- Solving puzzles with only one solution (e.g., mazes)
- Pathfinding in AI (when the path doesn't need to be optimal)


![[dfs-vs-bfs.gif]]

[[MST]]
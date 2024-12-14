
The Single Source Shortest Paths (SSSP) algorithm finds a path from a source vertex to every other vertex in a graph, such that the sum of the weights of the path edges is minimized.

Sub paths of Shortest Paths are Shortest Paths.

##  Dijkstra's Algorithm

The algorithm uses a greedy approach in the sense that we find the next best solution hoping that the end result is the best solution for the whole problem.


![[20190604151429474.gif]]

##### Djikstra's algorithm pseudocode

```pseudocode
function dijkstra(G, S)
    for each vertex V in G
        distance[V] <- infinite
        previous[V] <- NULL
        If V != S, add V to Priority Queue Q
    distance[S] <- 0
	
    while Q IS NOT EMPTY
        U <- Extract MIN from Q
        for each unvisited neighbour V of U
            tempDistance <- distance[U] + edge_weight(U, V)
            if tempDistance < distance[V]
                distance[V] <- tempDistance
                previous[V] <- U
    return distance[], previous[]
```

##### Code for Dijkstra's Algorithm

```c++
#include <iostream>
#include <vector>
#include <limits>

#define INF numeric_limits<int>::max()

using namespace std;

// Function to find the vertex with minimum distance value,
// from the set of vertices not yet included in shortest path tree
int minDistance(const vector<int>& dist, const vector<bool>& visited, int V) {
    int minDist = INF, minIndex = -1;
    
    for(int i = 0 ; i < V; i++){
        if(dist[i] != INF && visited[i] == false && dist[i] < minDist){
            minDist = dist[i];
            minIndex = i;
        }
    }

    return minIndex;
}

// Dijkstra's algorithm for single source shortest path
vector<int> dijkstra(const vector<vector<int>>& graph, int src) {
    int V = graph.size();
    vector<int> dist(V, INF); // Initialize distances as infinite
    vector<bool> visited(V, false); // Mark all vertices as not visited

    dist[src] = 0; // Distance of source vertex from itself is always 0

    // Find shortest path for all vertices
    for (int count = 0; count < V - 1; ++count) {
    
        int min_dis_vertex = minDistance(dist,visited,V);

        visited[min_dis_vertex] = true;

        // Update dist value of the adjacent vertices of the picked vertex
        for (int v = 0; v < V; ++v) {
        
            int distance_updater = graph[min_dis_vertex][v];
            if(distance_updater != INF && dist[min_dis_vertex] + distance_updater < dist[v]){
                dist[v] = dist[min_dis_vertex] + distance_updater;
            }
        }
    }

    // Return the shortest distance from src to dest
    return dist;
}

// Function to print shortest distances from source node to all other nodes
void printShortestDistances(const vector<int>& dist) {
    int V = dist.size();
    for (int i = 0; i < V; ++i) {
        cout << "Node " << i << ": " << dist[i] << endl;
    }
}

int main() {
    vector<vector<int>> graph = {
        {INF, 7, 9, INF, INF, 14},
        {7, INF, 10, 15, INF, INF},
        {9, 10, INF, 11, INF, 2},
        {INF, 15, 11, INF, 6, INF},
        {INF, INF, INF, 6, INF, 9},
        {14, INF, 2, INF, 9, INF},
    };
    
    int src;
    cin >> src;

    vector<int> dist = dijkstra(graph, src);
    printShortestDistances(dist);

    return 0;
}
```

#### Dijkstra's Algorithm Complexity

Time Complexity: `O(E Log V)`
               `O(V^2) for a dense graph`
where, E is the number of edges and V is the number of vertices.

Space Complexity: `O(V)`

Used **Priority queue**

## Bellman Ford's Algorithm

Negative weight edges can create negative weight cycles i.e. a cycle that will reduce the total path distance by coming back to the same point.

![[negative-weight-cycle_1.webp]]

Shortest path algorithms like Dijkstra's Algorithm that aren't able to detect such a cycle can give an incorrect result because they can go through a negative weight cycle and reduce the path length.


Bellman Ford Algorithm uses the dynamic programming approach.
This works by overestimating the length of the path from the starting vertex to all other vertices. Then it iteratively relaxes those estimates by finding new paths that are shorter than the previously overestimated paths.

![[Pasted image 20240407092522.png]]

![[Pasted image 20240407092456.png]]

![[Pasted image 20240407092540.png]]

![[Pasted image 20240407092554.png]]

![[Pasted image 20240407092611.png]]

![[Pasted image 20240407092627.png]]

![[Pasted image 20240407092639.png]]

##### Bellman Ford Pseudocode

```pseudocode
function bellmanFord(G, S)
  for each vertex V in G
    distance[V] <- infinite
      previous[V] <- NULL
  distance[S] <- 0

  for each vertex V in G				
    for each edge (U,V) in G
      tempDistance <- distance[U] + edge_weight(U, V)
      if tempDistance < distance[V]
        distance[V] <- tempDistance
        previous[V] <- U

  for each edge (U,V) in G
    If distance[U] + edge_weight(U, V) < distance[V}
      Error: Negative Cycle Exists

  return distance[], previous[]
```

Bellman Ford's algorithm and Dijkstra's algorithm are very similar in structure. While Dijkstra looks only to the immediate neighbors of a vertex, Bellman goes through each edge in every iteration.


#### Code for Bellman Ford's Algorithm

```c++
#include <bits/stdc++.h>

// Struct for the edges of the graph
struct Edge {
  int u;  //start vertex of the edge
  int v;  //end vertex of the edge
  int w;  //w of the edge (u,v)
};

// Graph - it consists of edges
struct Graph {
  int V;        // Total number of vertices in the graph
  int E;        // Total number of edges in the graph
  struct Edge* edge;  // Array of edges
};

// Creates a graph with V vertices and E edges
struct Graph* createGraph(int V, int E) {
  struct Graph* graph = new Graph;
  graph->V = V;  // Total Vertices
  graph->E = E;  // Total edges

  // Array of edges for graph
  graph->edge = new Edge[E];
  return graph;
}

// Printing the solution
void printArr(int arr[], int size) {
  int i;
  for (i = 0; i < size; i++) {
    printf("%d ", arr[i]);
  }
  printf("\n");
}

void BellmanFord(struct Graph* graph, int u) {
  int V = graph->V;
  int E = graph->E;
  int dist[V];

  // Step 1: fill the distance array and predecessor array
  for (int i = 0; i < V; i++)
    dist[i] = INT_MAX;

  // Mark the source vertex
  dist[u] = 0;

  // Step 2: relax edges |V| - 1 times
  for (int i = 1; i <= V - 1; i++) {
    for (int j = 0; j < E; j++) {
      // Get the edge data
      int u = graph->edge[j].u;
      int v = graph->edge[j].v;
      int w = graph->edge[j].w;
      if (dist[u] != INT_MAX && dist[u] + w < dist[v])
        dist[v] = dist[u] + w;
    }
  }

  // Step 3: detect negative cycle
  // if value changes then we have a negative cycle in the graph
  // and we cannot find the shortest distances
  for (int i = 0; i < E; i++) {
    int u = graph->edge[i].u;
    int v = graph->edge[i].v;
    int w = graph->edge[i].w;
    if (dist[u] != INT_MAX && dist[u] + w < dist[v]) {
      printf("Graph contains negative w cycle");
      return;
    }
  }

  // No negative weight cycle found!
  // Print the distance and predecessor array
  printArr(dist, V);

  return;
}

int main() {
    // Create a graph
    int V = 5; // Total vertices
    int E = 9; // Total edges

    // Array of edges for graph
    struct Graph* graph = createGraph(V, E);

    // edge 0 --> 2
    graph->edge[0].u = 0;
    graph->edge[0].v = 2;
    graph->edge[0].w = 4;

    // edge 0 --> 4
    graph->edge[1].u = 0;
    graph->edge[1].v = 4;
    graph->edge[1].w = 5;

    // edge 1 --> 2
    graph->edge[2].u = 1;
    graph->edge[2].v = 2;
    graph->edge[2].w = -4;

    // edge 2 --> 0
    graph->edge[3].u = 2;
    graph->edge[3].v = 0;
    graph->edge[3].w = -3;

    // edge 3 --> 0
    graph->edge[4].u = 3;
    graph->edge[4].v = 0;
    graph->edge[4].w = 4;

    // edge 3 --> 2
    graph->edge[5].u = 3;
    graph->edge[5].v = 2;
    graph->edge[5].w = 7;

    // edge 3 --> 4
    graph->edge[6].u = 3;
    graph->edge[6].v = 4;
    graph->edge[6].w = 3;

    // edge 4 --> 1
    graph->edge[7].u = 4;
    graph->edge[7].v = 1;
    graph->edge[7].w = 2;

    // edge 4 --> 2
    graph->edge[8].u = 4;
    graph->edge[8].v = 2;
    graph->edge[8].w = 3;

    BellmanFord(graph, 3); // 3 is the source vertex
    return 0;
}
```


#### Bellman Ford's Complexity

##### Time Complexity

| Best Case Complexity    | O(E)  |
| ----------------------- | ----- |
| Average Case Complexity | O(VE) |
| Worst Case Complexity   | O(VE) |
**Time Complexity when graph is disconnected********:
    - **All the cases:** O($E*(V^2)$)
##### Space Complexity

Space complexity is O(V)


Used **Array, List**

Bellman-Ford algorithm will fail if the graph contains any negative edge cycle.

---

| Feature:         | Dijkstra’s                                                                                                                                                                             | Bellman Ford                                                                                                                                              |
| ---------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Optimization     | optimized for finding the shortest path between a single source node and all other nodes in a graph with non-negative edge weights                                                     | Bellman-Ford algorithm is optimized for finding the shortest path between a single source node and all other nodes in a graph with negative edge weights. |
| Relaxation       | Dijkstra’s algorithm uses a greedy approach where it chooses the node with the smallest distance and updates its neighbors                                                             | the Bellman-Ford algorithm relaxes all edges in each iteration, updating the distance of each node by considering all possible paths to that node         |
| Time Complexity  | Dijkstra’s algorithm has a time complexity of O(V^2) for a dense graph and O(E log V) for a sparse graph, where V is the number of vertices and E is the number of edges in the graph. | Bellman-Ford algorithm has a time complexity of O(VE), where V is the number of vertices and E is the number of edges in the graph.                       |
| Negative Weights | Dijkstra’s algorithm does not work with graphs that have negative edge weights, as it assumes that all edge weights are non-negative.                                                  | Bellman-Ford algorithm can handle negative edge weights and can detect negative-weight cycles in the graph.                                               |

- A **dense graph** is a graph in which the number of edges is close to the maximum number of edges. In other words, there is an edge between every pair of vertices. For a directed graph, the maximum number of edges is
    V(V−1)
    
    , and for an undirected graph, it’s
    V(V−1)/2
    
    , where V is the number of vertices.
    
- A **sparse graph**, on the other hand, is a graph in which the number of edges is far less than the maximum possible number of edges. In other words, there are very few edges compared to the number of vertices.


[[Algorithm Design Techniques]]

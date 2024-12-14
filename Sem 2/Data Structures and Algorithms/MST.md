
### Spanning Tree

 A spanning tree is a tree-like subgraph of a connected, undirected graph that includes all the vertices of the graph

It is a subset of the edges of the graph that forms a tree (acyclic) where every node of the graph is a part of the tree.

Properties of a Spanning Tree
    - The number of vertices (V) in the graph and the spanning tree is the same.
    - There is a fixed number of edges in the spanning tree which is equal to one less than the total number of vertices ( E = V-1 ).
    - The spanning tree should not be disconnected, as in there should only be a single source of component, not more than that.
    - The spanning tree should be acyclic, which means there would not be any cycle in the tree.
    - The total cost (or weight) of the spanning tree is defined as the sum of the edge weights of all the edges of the spanning tree.


#### Minimum Spanning Tree: 

A minimum spanning tree (MST) is defined as a spanning tree that has the minimum weight among all the possible spanning trees


#### Steiner Minimum Tree

The Steiner Minimum Tree is a concept in combinatorial optimization that seeks to find the minimum-weight tree connecting a designated set of vertices, called terminals, in an undirected, weighted graph. 
The tree may include non-terminals, which are called Steiner vertices or Steiner points.

![[Screenshot (280).png]]

### Generic algorithm for finding a Minimum Spanning Tree (MST) in a graph

1. **Initialize**: Start with an empty set `A`. This set will eventually contain the edges of the MST.
    
2. **Check Spanning Tree**: The loop continues until `A` forms a spanning tree. A spanning tree is a tree that includes all the vertices of the graph.
    
3. **Find Safe Edge**: In each iteration of the loop, find a safe edge `(u, v)` to add to `A`. A safe edge is an edge that, if added to `A`, will keep `A` acyclic and connected.
    
4. **Add Edge**: Add the safe edge `(u, v)` to the set `A`.
    
5. **Return MST**: Once `A` forms a spanning tree, the algorithm ends, and `A` is returned. `A` is the set of edges that forms the Minimum Spanning Tree of the graph.


```
Python

def Generic_MST(G, w):
    A = set()  # Initialize an empty set A
    while not is_spanning(A, G):  # while A does not form a spanning tree
        u, v = find_safe_edge(A, G, w)  # find an edge (u,v) that is safe for A
        A.add((u, v))  # add edge (u,v) to A
        
    return A  # return the set A
```


#### **Safe Edge (u, v) ∈ E - A**: 

An edge `(u, v)` that is not in `A` (i.e., `(u, v) ∈ E - A`) is considered safe if adding it to `A` still keeps `A` as a subset of edges in some MST. In other words, `(u, v)` is safe if it is possible to extend `(V, A ∪ {(u, v)})` into a MST. This means that adding the edge `(u, v)` to `A` won’t prevent `A` from being able to be extended into a MST.


##### Definitions

1. **Cut (S, V-S)**: A cut in a graph is a partition of the vertices into two disjoint subsets. It is represented as (S, V-S), where S is one subset, and V-S is the other subset. V is the set of all vertices in the graph.
    
2. **Edge Crossing the Cut**: An edge (u, v) is said to cross the cut (S, V-S) if one endpoint (u or v) is in S and the other endpoint is in V-S.
    
3. **Cut Respects A**: Given a subset of edges A, a cut (S, V-S) respects A if no edge in A crosses the cut. In other words, all edges in A have both endpoints in either S or V-S, but not one in each.
    
4. **Light Edge Crossing a Cut**: Among all edges that cross a cut (S, V-S), an edge is a light edge if it has the minimum weight. This concept is used in Prim’s algorithm for finding MSTs, where the algorithm always chooses the light edge that connects a vertex in the MST to a vertex outside the MST.


### Finding a safe edge in a graph

1. **Graph G = (V, E)**: This represents a connected, undirected, and weighted graph where `V` is the set of vertices and `E` is the set of edges.
    
2. **Subset of Edges A ⊆ E**: This means that `A` is a subset of the edges `E` in the graph. In the context of MST, `A` is the set of edges that are part of some MST.
    
3. **Theorem**: The theorem states that for a cut `(S, V-S)` that respects `A`, if `(u, v)` is a light edge crossing this cut, then the edge `(u, v)` is safe for `A`.
    
    - **Cut (S, V-S) Respects A**: A cut `(S, V-S)` respects `A` if no edge in `A` crosses the cut. In other words, all edges in `A` have both endpoints in either `S` or `V-S`, but not one in each.
        
    - **Light Edge (u, v) Crossing the Cut**: Among all edges that cross the cut `(S, V-S)`, an edge is a light edge if it has the minimum weight.
        
    - **Safe Edge (u, v)**: An edge `(u, v)` is safe for `A` if adding it to `A` still keeps `A` as a subset of edges in some MST. In other words, `(u, v)` is safe if it is possible to extend `(V, A ∪ {(u, v)})` into a MST.


### Algorithms for computing Minimum Spanning Trees

- **Kruskal’s Algorithm**:
    - It starts with a forest with single vertex trees.
    - It adds edges in increasing order of weight.
    - The trees merge into a single tree, which is the MST.
    
- **Prim’s Algorithm**:
    - It starts with a single vertex as the root node of the tree.
    - It adds one node at a time to the current tree.
    - The tree grows until it spans all vertices, forming the MST.


### *Kruskal’s algorithm*

- It emphasizes that trees merge into a single tree during execution.
- Each tree is connected, meaning there are no isolated vertices.
- Adding a new edge to a tree might induce a cycle if it connects vertices within the same tree.
- If an edge connects two separate trees, it won’t create a cycle; thus, such edges should be added to ensure all vertices are connected in the final MST.

![[kruskal.gif]]


```
function Kruskal(G):
    A = ∅
    For each vertex v in G.V:
        MAKE-SET(v)
    Sort the edges of G.E into non-decreasing order by weight w
    For each edge (u, v) in G.E, taken in non-decreasing order by
    weight:
        if FIND-SET(u) ≠ FIND-SET(v):
            A = A ∪ {(u, v)}
            UNION(u, v)
    return A
```

- `MAKE-SET(v)`: This operation makes a new set by creating a new element with a parent pointer to itself.
- `FIND-SET(u)`: This operation returns the representative (or master node) of the set that contains `u`. It can be implemented by following the chain of parent pointers from `u` upwards through the tree until an element is reached whose parent is itself.
- `UNION(u, v)`: This operation merges two disjoint sets into a single set. The two sets are assumed to be disjoint prior to the operation.


#### Time complexity 

- **Lines 1-3 (initialization)**: This step involves initializing the data structures used in the algorithm, which takes O(V) time where V is the number of vertices in the graph.
    
- **Line 4 (sorting)**: The edges of the graph are sorted in non-decreasing order by weight. This step takes O(E log E) time where E is the number of edges in the graph.
    
- **Lines 6-8 (set-operation)**: These lines involve the set operations (find and union) performed on the disjoint sets of vertices. This step also takes O(E log E) time.
    
- **Total**: The total time complexity of Kruskal’s algorithm is 
$$
 O(E log E)
$$
- which is the same as the time complexity of sorting the edges. This is because the dominating factor in the time complexity is the sorting of the edges, and the time taken for the set operations is within the same order of magnitude.


Used **Disjoint Set (Union-Find)** , **Edge List**

### *Prim’s algorithm*

1. **Initialization**: Start with a set of vertices `S` that is currently part of the MST, and its complement `(V-S)`. Here, `V` is the set of all vertices in the graph.
    
2. **Cut of the Graph**: A cut `(S, V-S)` of the graph is made. A cut is a partition of the vertices of a graph into two disjoint subsets. The cut respects the set of tree edges `A`, meaning no edge in `A` crosses the cut.
    
3. **Edge Selection**: The next edge to be added to the MST is the light edge. A light edge respects a cut if its ends are in different subsets defined by the cut, and it has the minimum weight of any edge crossing the cut.
    
4. **Tree Growth**: The tree grows by adding the selected light edge to the MST, and moving the newly connected vertex from `(V-S)` to `S`. This process continues until `S` equals `V`, meaning the tree spans all the vertices in `V`.

![[prim.gif]]


```
function MST-PRIM(G, w, r):

    // Initialization
    for each vertex u in G.V:  
        u.key = ∞      // Set initial key (distance) to infinity
        u.π = NIL     // Set parent to null (not yet connected to MST)
    r.key = 0        // Starting vertex's distance is 0 
    Q = G.V         // Initialize priority queue with all vertices

    // Main Loop
    while Q ≠ ∅:          // While there are vertices not in the MST
        u = EXTRACT-MIN(Q)   // Extract vertex with minimum key value 
        for each v in G.Adj[u]:  // For each neighbor of u:
            if v in Q and w(u, v) < v.key:  
                v.π = u           // Update v's parent to u
                v.key = w(u, v)  // Update v's key to the edge weight

    // Implicitly, the MST is formed by the edges (v, v.π) for each v in G.V

```


- `u.key = ∞` and `u.π = NIL`: For each vertex `u` in the graph `G`, set `u.key` to infinity and `u.π` to `NIL`. The key value of a vertex represents the minimum weight of any edge connecting that vertex to a vertex in the MST. The `π` attribute of a vertex is the parent of that vertex in the MST.
    
- `r.key = 0`: The key of the root `r` is set to 0.
    
- `Q = G.V`: A priority queue `Q` is initialized with all vertices in the graph `G`. The vertices are ordered by their key values in the queue.
    
- `u = EXTRACT-MIN(Q)`: While the queue `Q` is not empty, extract the vertex `u` with the minimum key value.
    
- `if v in Q and w(u, v) < v.key`: For each vertex `v` adjacent to `u`, if `v` is in `Q` and the weight of the edge `(u, v)` is less than `v.key`, then update `v.π` to `u` and `v.key` to `w(u, v)`.

#### Time complexity 

- **Extracting the vertex from the queue**: This operation takes O(log V) time. The vertex with the minimum key value is extracted from the priority queue.
    
- **Decreasing the key of the neighboring vertex**: For each incident edge, if the neighboring vertex is in the queue and the weight of the edge is less than the key of the neighboring vertex, the key of the neighboring vertex is updated. This operation also takes O(log V) time.
    
- **Other steps**: The other steps in Prim’s algorithm are constant time operations. These include operations like initializing the keys of all vertices to infinity and setting the key of the root to 0.
    
- **Overall running time**: The overall running time of Prim’s algorithm is 
$$
O((V + E)log V)
$$

If Adjacency matrix and searching used $O(V ^2)$

Used Priority Queue, Heap

> [!NOTE]
> A complete graph with n vertices has n^(n-2) spanning trees
> 
> 	A complete graph is an undirected graph in which every pair of distinct vertices is connected by a unique edge.
> 	Every vertex in the graph has degree n-1
> 	Has All total n*(n-1)/2 edges
> 

> [!NOTE]
> In a DFS traversal of a directed graph, edges are classified as follows:
> 
> - **Tree edges**: Edges that are part of the DFS tree.
> - **Back edges**: Edges that point from a node to one of its ancestors in the DFS tree.
> - **Forward edges**: Edges that point from a node to a descendant in the DFS tree.
> - **Cross edges**: Edges that point from a node to another node that is neither an ancestor nor a descendant.


[[Single-Source Shortest Path]]

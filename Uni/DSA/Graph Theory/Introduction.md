## Terminology

Definition
A set of vertices and edges (ordered if directed graph)
$$G = (V, E)$$
$$ E \in V \times V$$

Subgraph
$$ G_1 \subseteq G \;,\; G_1 = (V_1, E_1)$$
then
$$ V_1 \subseteq V \;\; E_1 \subseteq E$$
Simple graph - No loops nor parallel/multiple edges

Complete graph ($K_n$) - A graph with n vertices and an edge between each pair of vertices

Bipartite graph - A graph where the vertices can be divided into two disjoint sets such that all edges connect a vertex in one set to a vertex in another set. There are no edges between vertices in the disjoint sets
- A graph is not bipartite if it has a odd length cycle
- A graph is not bipartite if it has a loop
- Complete bipartite graph has every vertex in one set joined to every vertex in the other set

Multigraph - A graph with parallel edges. If there are m parallel edges associated with vertices u and v then the edge (u, v) has a multiplicity of m

Pseudograph - A graph with loops and multiple edges

Weighted graph - A graph in which each edge e is assigned a non negative real number

Degree - The number of edges incident with a vertex in a graph is called the degree of the vertex. In a pseudograph, a loop contributes 2 to a vertex's degree
- In-degree
- Out-degree
- Handshaking theorem - Let $G=(V, E)$ be an undirected graph with m edges, Then
	$$2m = \sum_{v \in V} deg(v)$$
	Corallary - In an undirected graph, sum of degrees of vertices is even

Isomorphism- Two simple graphs G and H are said to be isomorphic if there is a function
$$\theta:V(G) \rightarrow V(H)$$
such that $\theta$ is a one to one and onto function which preserves the adjacency and non adajacency. That is,
$$(u, v) \in E(G) \Leftrightarrow (\theta(u), \theta(v)) \in E(H)$$
### Walk
While traversing a graph, if the vertices and edges can be repeated, it is called a walk
### Trail
Vertices can be repeated but not edges
### Path
Both vertices and edges cannot be repeated. All paths are trails but not all trails are paths
### Circuit
A closed trail
### Cycle
A closed path
- Cyclic graph - Graph contains atleast one cycle
- Acyclic graph - Graph without cycles
	- dag - directed acyclic graph
	- tree - undirected acyclic graph
	- forest - undirected acyclic graph that is not necessarily connected
### Connected Graph
Atleast one path exist between every pair of vertices
### Traversable
- Open traversable - Traverse each edge once and go to all vertices once (Eularian Circuit #TODO  Check this). For this the graph must have only two odd nodes.
- Closed traversable - Traverse each edge once and go to all vertices once and end up on the starting vertice (Eularian Path #TODO Check this) For this the graph must have zero odd nodes.
Chinese postman problem
Examples
![[Pasted image 20240210193246.png]]
![[Pasted image 20240210194301.png]]
An algorithm for finding an optimal Chinese postman
route is:
Step 1 List all odd vertices.
Step 2 List all possible pairings of odd vertices.
Step 3 For each pairing find the edges that connect the
vertices with the minimum weight.
Step 4 Find the pairings such that the sum of the weights
is minimised.
Step 5 On the original graph add the edges that have been
found in Step 4.
Step 6 The length of an optimal Chinese postman route is
the sum of all the edges added to the total found
in Step 4.
Step 7 A route corresponding to this minimum weight
can then be easily found.

## Running Times
### Representation

If $|E| \approx |V|$ it is called a sparse graph. If $|E| \approx |V|^2$ it is called a dense graph. Depending on the type of graph, different data structures can be used.
#### Adjacency Matrix
Assume $$ V = {1, 2, ..., n} $$
Then represent the graph in a nxn matrix A where
$$A[i, j] = 1 \;\; if \; (i, j) \in E$$
$$ A[i, j] = 0 \;\; if \; (i, j) \notin E $$
Storage required by an adjacency matrix: $O(V^2)$
- This is a dense representation
- Efficient for small graphs
- For undirected graphs
	- Can omit the lower triangle since the matrix is symmetric
	- No self loops then omit the diagonal
	![[Pasted image 20240210202420.png]]

#### Adjacency List
For each vertex v store a list of vertices adjacent to v
Storage:
- For directed graph, items in adjacency list is
	$$\sum out-degree(v) = |E|$$
	Storage taken
	$$O(V+E)$$
- For undirected graph, items in adjacency list is
	$$\sum degree(v) = 2|E|$$
	Storage taken
	$$O(V+E)$$

## Searching
### BFS (Breadth First Search)
```
BFS(G, s)
  s->distance = 0
  s->visited = true
  s->parent = NULL
  Q = {s} // add start point to queue
  while (Q not empty)
	  u = RemoveTop(Q)
	  for each v in u->adj
		  if (v->visited==false)
			  v->visited==true
			  v->distance = u->distance + 1;
			  v->parent = u
			  Enqueue(Q, v)
	  
```
Time complexity: $O(V+E)$
Space complexity: $O(max(degree(v))) = O(E)$
BFS calculates the shortest path distance to root node

### DFS (Depth First Search)
Keep exploring the most discovered vertex that still has unexplored edges. Backtrack when all the edges are explored.
```
time = 0

DFS(G, s)
	s->discover = time
	DFS_visit(s)

DFS_visit(u)
	if u->visited == true 
		return
	time = time + 1
	u->discover = time
	u->visited = true
	for (v in u->adjacent)
		DFS_visit(v)
	time = time + 1
	u->finish = time
```
Time complexity: $O(V+E)$
- DFS_visit runs once per edge in a directed graph and twice in an undirected graph

DFS on adjacency matrix takes $O(V^2)$. This is because for each vertex, we need to iterate through all the other vertices to check if they are adjacent or not.
```c
void dfs(int start, vector<bool>& visited)
{
 
    // Print the current node
    cout << start << " ";
 
    // Set current node as visited
    visited[start] = true;
 
    // For every node of the graph
    for (int i = 0; i < adj[start].size(); i++) {
 
        // If some node is adjacent to the current node
        // and it has not already been visited
        if (adj[start][i] == 1 && (!visited[i])) {
            dfs(i, visited);
        }
    }
}
```


Edge classification in DFS
- Tree Edge
	- During DFS edges that allows us to discover unvisited edges
	- u->discover **<** v->discover **<** v->finish **<** u->finish
- Back Edge
	- Edges that go back to the ancestor
	- v->discover **<** u->discover **<** u->finish **<** v->finish
	- **An undirected graph is acyclic if and only if a DFS yields no back edges**
- Forward Edge
	- Edge going from ancestor to visited node
	- u->discover **<** v->discover **<** v->finish **<** u->finish
- Cross Edge
	- Edges that go to visited nodes that are not ancestor or decendant
	- v->discover **<** v->finish **<** u->discover **<** u->finish

## Minimum Spanning Tree
### Prims Algorithms
Its a greedy algorithm
Procedure:
- Select any vertex and chose the edge of smallest weight from it.
- At each stage, choose the edge of smallest weight joining a vertex already included to a vertex not yet included
- Continue until all vertices are included

```
Prim(G, w, r)
	Q = V[G]
	// O(V)
	for each u in Q
		key[u] = ∞
	key[r] = 0
	p[r] = NULL
	// O(V) as each node is processed once
	while (Q not empty)
		// O(logV)
		u = ExtractMin(Q)
		// O(E) - worst case all vertices are connected to u
		for each v in adj[u]
			// v in Q checks if it is visited
			if (v in Q and w(u, v) < key[v])
				p[v] = u
				key[v] = w(u, v)
```

The running time will depend on the queue
- Binary heap: $O(ElogV)$
- Fibonacci heap: O(VlogV + E)

In graphs with negative weight cycles some shortest paths will not exist. Explain why? 
This is due to the fact that negative weight cycles can lead to infinitely decreasing paths, causing issues with the well-defined nature of shortest paths.

### Dijkstra's Algorithm
```
Dijkstra(G)
	for each v in V
		d[v] = ∞
	d[s] = 0
	Q = V
	while (Q is not null)
		u = ExtractMin(Q)
		for each v in u->Adj[]
			if (d[v] > d[u]+w(u,v))
			d[v] = d[u]+w(u, v)
```

The running time will depend on the queue
- Binary heap: $O(ElogV)$
- Fibonacci heap: O(VlogV + E)

Floyd Warshall
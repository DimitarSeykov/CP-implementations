# Graphs:
<p>
Let V be a set of vertices V = {v | v ∈ N}. <br></li>

Let E be a set of edges connecting those vertices E = {(u, v) | u, v ∈ V}.<br></li>

We will call G(V, E) a graph vith vertices V and edges E.<br></li>

A path in G will be a sequence of vertices from V connected by edges from E:<br>
p = (v1, v2, ..., vn), v1, v2, ... vn ∈ V & ∀ (vi, vi+1) ∈ E.<br></li>

A simple path in G will be a path p, such that no vertex in the path is repeated:<br>
p = (v1, v2, ..., vn), v1, v2, ... vn ∈ V & ∀ (vi, vi+1) ∈ E, ∀ vi, vj : i != j => vi != vj.<br></li>

The length of a path is the number of edges in the path.<br></li>

In a weighted graph the cost of a path is the sum of the cost of all edges in the path.<br></li>

A graph can be represented programatically in a few ways:<br></li>

1. By a list of edges. (eg. vector<pair<int, int>> where each pair is an edge).<br>

2. By an adjacency matrix. A matrix where the cell at postition (i, j) indicates if there is an edge between vertices i and j.<br>

3. By an adjacency list. A list for each vetrex containing all of it's neighbours (eg. vector<int> graph[]).<br>

~~~    
Graph: (1)---(2)---(3)    List of edges: (1, 2)    Adjecency matrix: 0 1 0 0    Adjecency list: 1: 2
              |  /                       (2, 3)                      1 0 1 1                    2: 1, 3, 4
              (4)                        (3, 4)                      0 1 0 1                    3: 2, 4
                                         (2, 4)                      0 1 1 0                    4: 2, 3
~~~                  
If we have a directed graph, then the edges have a direction and can be traversed only one way. The edge (i, j) allows us to go from vetrex i to vetrex j but not the other way around. We must take this into account when we build our graph.<br>
~~~  
Graph: (1)--->(2)----->(3)    List of edges: (1, 2)    Adjecency matrix: 0 1 0 0    Adjecency list: 1: 2
               |        |                    (2, 3)                      0 0 1 1                    2: 3, 4
               |->(4)<--|                    (3, 4)                      0 0 0 1                    3: 4
                                             (2, 4)                      0 0 0 0                    4:
~~~
A graph can have costs on its edges, it that case they are represented as triplets instead of pairs: e = (u, v, cost).</li>
~~~
Graph: (1)--3--(2)--2--(3)    List of edges: (1, 2, 3)    Adjecency matrix: 0 3 0 0
                |     /                      (2, 3, 2)                      3 0 2 1
                1    5                       (3, 4, 5)                      0 2 0 5
                |   /                        (2, 4, 1)                      0 1 5 0
                |  /
                (4)           Adjecency list: 1: (2, 3)
                                              2: (1, 3), (3, 2), (4, 1)
                                              3: (2, 2), (4, 5)
                                              4: (2, 1), (3, 5)
~~~                  
</p>

## DFS
- Depth first search. Visits every vertex reachable from the starting vertex.

- O(V + E) complexity, where V is the number of vertixes, and E is the number of edges.

- <p>We want to visit every vertex only once so we need to keep track of which vertixes we have visited. (bool visited[]).<br>
   
   Let v be the vertex we are currenlty at. First we need to mark it as visited and then we need to recursively visit all of it's neighbours that aren't allready visited. When there are no more vertexes that are not visited, we are done.
   
   We consider every vertex only once and we consider each edge twice (once from evert direction) => O(n + m).</p>
   
   <p align="center">
      <img width="60%" src="https://inginious.org/course/competitive-programming/graphs-dfs/dfs.gif">
   </p>

## BFS
- Breadth first search. Visits every vertex reachable from the starting vertex. Splits the graph into levels. Finds the shortest path from the starting vertex to every other vertex in an unweighted graph.

- O(V + E) complexity, where V is the number of vertixes, and E is the number of edges.

- <p>We want to visit every vertex only once so we need to keep track of which vertixes we have visited. (bool visited[]).<br>
  
  We will use a queue to keep track of which is the current vertex we are at. This will always be the top of the queue.
   
   Let v be the vertex we are currenlty at. First we need to mark it as visited and then we need to add all of it's neighbours that aren't allready visited in the queue in order to be visited next. When there are no more vertexes that are not visited, we are done.<br>
   
   Let s be our starting vertex. The distance to s is 0. To find the distance to all of the neighbours of s is 1, the distance to all of their unvisited neighbours is 2 and so on. Those groups of vertices (that have the same distace to s) are the levels of the bfs. All vertices at level 1 are at distance 1 all at level to at distance 2 and so on.<br>
   
   When we are at vertex u that has level l, all of its unvisited neighbours are at level l + 1, because we must travel trough one more edge in order to reach them. So in order to calculate the distance, every time we add a new vertex to the queue, the distance to it is with 1 bigger that the distance to the vertex we are currently at: distance[new_vertex] = distance[current_vertex] + 1.
   
   We consider every vertex only once and we consider each edge twice (once from evert direction) => O(n + m).</p>
   
   <p align="center">
      <img width="60%" src="https://miro.medium.com/max/1280/0*miG6xdyYzdvrB67S.gif">
   </p>

## Number of paths of fixed length
- Find the number of paths with lenght k between two nodes from the graph G.

- O(V^3 log k) complexity.

- <p>Let's take a look at the case where k = 1.<br>
  
  If we build the adjacency matrix Adj of the graph, then in the cell (u, v) we will have the number of ways to reach v from u by using only one edge: 1 way if they are connected, 0 ways if they are not.<br>
  
  Now let's take a look at the case where we know the number of paths with length k - 1 from u to v and we want to find out the number of paths with lenght k from u to v.<br>
  
  Then the number of paths from u to v with lenght k can be computed by counting all paths with length k - 1 that start at u end in a vertex that is connected to v. We can just sum the number of paths ending at every vertex connected to v. In order to do this we can take the paths ending at vertex i multiplied by Adj[i][v], which will be 1 if i is connected to v and 0 if it is not, and sum this value for every vertex i, which will add up to the total number ot paths from u to v with lenght k.<br>
  
  Let Pk-1 is the matrix that contains all the paths from u to v with lenght k - 1 in cell (u, v). Then we can calucate Pk with the help of the adjacency matrix Adj and Pk-1:<br>
              
  Pk [u][v] = Pk-1[u][0] * Adj[0][v] + Pk[u][1] * Adj[1][v] + ... + Pk-1[u][n - 1] * Adj[n - 1][v] = sum (Pk-1[u][i] * Adj[i][v]),<br>
  i = 0, 1, ..., n.<br>
  
  This operation is actually just the multiplication of the two matrices Pk-1 and A => Pk = Pk-1 * Adj.<br>
  
  With this observation we can see that to calculate Pk we can calculate A to the power of k. Pk = Adj^k.<br>
  
  All path with length k from u to v can be found at (Adj^k)[u][v].
  </p>

## Belman Ford
- Find the shortes path to every vertex from a starting vertex s in a weighted graph.

- O(VE) complexity.

- <p>We will create an array of distances dist[], which will contain the answer to the problem.<br>
  
  In the beginning we all elements of dist[] equal to infinity INT_MAX except for dist[s], which will be equal to 0, because it is our starting point.<br>
  
  Let (u, v, cost) in an edge from E. If the distance to v is greater that the distance to u plust the cost, then we can reach v more efficiently by using the path to u and the edge (u, v, cost) instead of the current path to v. This is called rexalxation and we will use it to solve our problem.<br>
  
  Relaxation: dist[u] + cost < dist[v] then dist[v] = dist[u] + cost.<br>

  The algorithm consists of n - 1 phases. Each phase goest through all edges of the graph, and tries to perform a relaxation using each edge (u, v, cost). After tring to relax with each edge n - 1 times the array dist will contain the values of the shortest paths from s to every other vertex in G.
</p>

## Shortest Path Faster Algorithm (SPFA)
- Find the shortes path to every vertex from a starting vertex s in a weighted graph.

- O(VE) complexity but works in O(E) in many caces.

- SPFA is a improvement of the Bellman-Ford algorithm which takes advantage of the fact that not all attempts at relaxation will work. The main idea is to create a queue containing only the vertices that were relaxed but that still could further relax their neighbors. And whenever you can relax some neighbor, you should put him in the queue.

## Dijkstra
- Find the shortes path to every vertex from a starting vertex s in a weighted graph.

- O(E log V) complexity or O(V ^ 2) complexity depending on implementation.

- On dense graphs: https://cp-algorithms.com/graph/dijkstra.html

- On sparce graphs: https://cp-algorithms.com/graph/dijkstra_sparse.html

## Floyd Warshall
- Find the shortest path between every pair of vertices from the weighted graph G(V, E).

- O(V ^ 3) complexity.

- <p>The approach we will take is that we will find the shortest path between every pair of vertices (u, v, cost) that passes trough at most k edges. Since a path can't have more than n - 1 edges that means that we must compute the shortest paths for k = 0, 1, 2, ... n - 1.<br>

     For k = 0 (the case where we do not use any edges) we simply mark the distance from every vertex to itself as 0 (dist[i][i][0] = 0).<br>

     For k = 1 (the case there we use only a single edge) we mark the distance between every two vertices that are connected by an edge (u, v, cost) as that edges cost (dist[i][j][1] = cost, where (i, j, cost) ∈ E). The distance between all pairs of vertices that are not conected by an edge we init as INT_MAX.<br>

     Once we have computed the aswers for 0, 1, ... k - 1, we can compute the asnwer for k as follows:<br>

     For every triplet of vertices (u, v, i), and a fixed ammount for edges passed from u to i, let that number be j, we check if:<br>

     distance[u][i][j] + distance[i][v][k - j] < distance[u][v][k - 1] or distance[u][i][k - j] + distance[i][v][j] < distance[u][v][k - 1]<br>

     If so, we can reach v from u, by passing through k edges and with a path with a lower cost than the best path that uses less than k edges. To perform this procedure will require O(V ^ 4) time, but we can see that this can be lowered to O(V ^ 3) if we drop the requiremet for the number of edges. When updating the distance from u to v, the best possible path would be to use: distance[u][i][k - 1] + distance[i][v][k - 1] even if it uses more than k edges since if it does we will simply use it at a later stage (when we are at the neccessery k) => we need only the best path found for every pair (i, j), the value at distance[i][j][k] and not any of the values distance[i][j][x], where x < k which makes the requirement for the numebr of edges redundant in our state and we can modifiy it to distance[u][v].<br>

     Instead of calculating the optimal path from u to v using at most k edges, we can simply calculate the optimal path from u to v by checking if we can lower it's cost by adding an additional vertex along the way, without bothering with the exact number of edges in the path.<br>
     
     We again initialize distance[i][i] = 0 and distance[i][j] as the cost of the edge between i and j if there is one and INT_MAX if there isn't. Then we try to optimise the cost of the path between each pair of vertices (u, v), by tring to add an arbitary vertex i along the way:<br>

     if distance[u][i] + distance[i][v] < distance[u][v] then distance[u][v] = distance[u][i] + distance[i][v]<br>
     
     With this optimisation our procedure will run in O(V ^ 3) time.
</p>

## Prim

## Kruskal

## Find cycles

## Find bridges

## Find articulation points

## Topological sort

## Strongly connected components (Koraraju)

## Boolean 2 satisfiability (2-SAT)

## K-th ancestor

## Lowest common ancestor (LSA)

## Max flow (Edmonds Karp)

## Max flow (Dinic)

## Max flow (MPM)

## Min cost max flow (assignment problem)

## Virtual tree

## Path queries on tree (LSA + Segment tree)

## A*


  


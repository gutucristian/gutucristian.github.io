A collection of notes covering information on core Computer Science algorithms and data structures as well as relevant practice problems highlighting these topics.

## Graphs 

The two main graph representations we use when talking about graph problems are the **adjacency list** and **adjancency matrix** approaches. Generally, adjacency lists are the right data structure for most applications of graphs.

### Adjacency List 😎

- An adjacency list is a list of lists
- Each list corresponds to a node `v` (i.e., vertex) on the graph and contains all the edges `(u, v)` that originate from the node `u`
- Thus, an adjacency list takes up `O(V + E)` space (`V` for number of nodes and `E` for number of edges)
- In Python, we can represent an adjacency list as a dictionary. The dictionary’s **keys will be the nodes**, and their **values will be the edges for each node**
- With the help of an adjacency list, we can find all the neighbours for a particular node in constant time (the quick lookup is due to the hashing mechanism of dictionaries)

```code
# This is an adjacency list
graph = {
  'A': ['B', 'C'],
  'B': ['D', 'E'],
  'C': ['F', 'G'],
  'D': [],
  'E': [],
  'F': [],
  'G': []
}
```

**To add a node** to the graph, add a new key to the `graph` dictionary. Until we add neighbours, the value of this key will be an empty list.

**To add a neighbour (e.g, `u`) to a node (e.g., `v`)** append `u` to the value of `v` in the `graph` dictionary. The value of `v` in the `graph` dictionary is a list which holds all of `v`s neighbours.

**To delete a node** from the graph (e.g., `v`) first delete the key corresponding to `v` in the `graph` dictionary. Then cycle through all other nodes in the `graph` and remove any occurence of `v` from each of these nodes neighbours list.

**To get a node's neighbours** access the value of that node in the `graph` dictionary; this is a list that contains all of this node's neighbours.

Pros:
- Easy to get a node's neighbours (just access `graph[v]` where `v` is the node we care about); `O(1)` time complexity
- Harder to tell if a particular node (e.g., `u`) is a neighbour of another node (e.g., `v`) -- to find out you would have to do a linear scan through all the nodes in `graph[v]` and search for `u`; at worst it's `O(V)` time complexity (if `v` has an edge to every other node in the graph)
- Fast and easy to add a node; `O(1)` time complexity
- Easy to delete a node; `O(V+E)` time complexity

### Adjacency Matrix 🧐

Pros:
- Easy to determine if a node has a particular neighbour

References:
- https://courses.csail.mit.edu/6.006/spring11/exams/notes2-2.pdf
- https://stackoverflow.com/questions/2218322/what-is-better-adjacency-lists-or-adjacency-matrices-for-graph-problems-in-c
- https://www.quora.com/What-is-the-time-complexity-for-adding-removing-nodes-and-edges-into-adjacency-list-and-matrix

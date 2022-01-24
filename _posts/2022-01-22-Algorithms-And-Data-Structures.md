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
  'C': ['F', 'G']
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

## Dynamic Programming

Dynamic programming is a computer programming technique.

Fundamentally, it refers to simplifying a complicated problem by breaking it down into simpler sub-problems in a **recursive** manner.

There are two key attributes that a problem must have in order for dynamic programming to be applicable: 
1. **Optimal substructure**
2. **Overlapping sub-problems**

**Optimal substructure** means that the solution to an *optimization problem* can be obtained optimally by breaking it into sub-problems and then recursively finding the optimal solutions to those sub-problems

**Overlapping sub-problems** means that the recursive algorithm solving the problem should solve the same sub-problems over and over, rather than generating new sub-problems

Many dynamic programming problems can be solved using the following sequence:

1. Find a recursive relation
2. Recursive (top-down)
3. Recursive + memo (top-down)
4. Iterative + memo (bottom-up)
5. Iterative + `N` variables (bottom-up)

References:
- https://leetcode.com/problems/house-robber/discuss/156523/From-good-to-great.-How-to-approach-most-of-DP-problems.

## Binary Search Trees (BST)

There are three ways of traversing a binary search tree:
1. In-order (visit left sub-tree, current node, right sub-tree)
2. Pre-order (visit current node, left sub-tree, right sub-tree)
3. Post-order (visit left sub-tree, right sub-tree, current node)

In-order, pre-order, and post-order are all variations of the **Depth First Search (DFS)** graph traversal strategy.

**In-order traversal** is useful because it returns the BST in **ascending** order.

**Pre-order traversal** in useful because it returns a list representation of the BST (i.e., it can be used to **copy a tree**). It can also be used to make a prefix expression (Polish notation) from expression trees (by traversing the expression tree in a pre-order fashion). You can also preorder traversal to print out hierarchical format of the tree in a linear format. For example:

```code
- ROOT
    - A
         - B
         - C
    - D
         - E
         - F
             - G
```

**Post-order traversal** can be used to delete a tree or generate a postfix expression of the tree.

References:
- https://towardsdatascience.com/4-types-of-tree-traversal-algorithms-d56328450846
- https://www.quora.com/What-is-the-use-of-pre-order-and-post-order-traversal-of-binary-trees-in-computing
- https://stackoverflow.com/questions/9456937/when-to-use-preorder-postorder-and-inorder-binary-search-tree-traversal-strate

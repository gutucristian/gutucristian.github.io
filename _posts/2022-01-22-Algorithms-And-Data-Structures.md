A collection of notes covering information on core Computer Science algorithms and data structures as well as relevant practice problems highlighting these topics.

## Graphs 

The two main graph representations we use when talking about graph problems are the **adjacency list** and **adjancency matrix** approaches.

### Adjacency List 😎

- An adjacency list is a list of lists
- Each list corresponds to a node `u` (i.e., vertex) on the graph and contains all the edges `(u, v)` that originate from the node `u`
- Thus, an adjacency list takes up `O(V + E)` space
- In Python, we can represent an adjacency list as a dictionary. The dictionary’s **keys will be the nodes**, and their **values will be the edges for each node**
- With the help of an adjacency list, we can find all the neighbours for a particular node in constant time (the quick lookup is due to the hashing mechanism of dictionaries)

Operations:
- Add a node
- Remove a node
- Get the neighbours of a node

Pros:
- East to get a node's neighbours
- Harder to tell if a particular node is a neighbour of another node -- to find out you would have to do a linear scan through all the nodes neighbours
- Easy to add and remove a node

### Adjacency Matrix 🧐

Pros:
- Easy to determine if a node has a particular neighbour

References:
- https://courses.csail.mit.edu/6.006/spring11/exams/notes2-2.pdf

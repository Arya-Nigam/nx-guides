---
jupytext:
  text_representation:
    extension: .md
    format_name: myst
    format_version: 0.13
    jupytext_version: 1.16.7
kernelspec:
  display_name: .venv
  language: python
  name: python3
---

## CHECK PLANARITY

+++

## Planarity Algorithm in NetworkX

## Introduction
A graph is said to be **planar** if it can be drawn on a plane without any of its edges crossing. The `check_planarity` function in NetworkX helps determine whether a given graph is planar and provides either an embedding or a counterexample.

### Mathematical Background
A graph is **planar** if and only if it does not contain a subgraph homeomorphic to **K₅ (complete graph on 5 vertices)** or **K₃,₃ (complete bipartite graph with partition sizes 3 and 3)**. This is known as **Kuratowski’s theorem**.

The planarity check in NetworkX is based on the **Left-Right Planarity Test**, an efficient combinatorial method to determine planarity.

```{code-cell} ipython3
import networkx as nx
import matplotlib.pyplot as plt
```

## Parameters

G: NetworkX graph
counterexample : bool
A Kuratowski subgraph (to proof non planarity) is only returned if set to true.

+++

## Returns

:
(is_planar, certificate)
(bool, NetworkX graph) tuple
is_planar is true if the graph is planar. If the graph is planar certificate is a PlanarEmbedding otherwise it is a Kuratowski subgraph.

```{code-cell} ipython3
def check_graph_planarity(G, show_plot=True):
    """
    Check if a graph is planar and return either an embedding or a counterexample.
    
    Parameters:
        G (networkx.Graph): Input graph.
        show_plot (bool): Whether to visualize the graph.
    
    Returns:
        tuple: (is_planar, certificate) where:
               - is_planar is True if G is planar
               - certificate is a PlanarEmbedding if G is planar, else a Kuratowski subgraph
    """
    is_planar, certificate = nx.check_planarity(G, counterexample=True)
    
    if show_plot:
        plt.figure(figsize=(5, 5))
        nx.draw(G, with_labels=True, node_color='lightblue', edge_color='gray', node_size=500)
        plt.title("Input Graph")
        plt.show()
    
    if is_planar:
        print("The graph is planar. Here is the Planar Embedding:")
        print(certificate.get_data())
    else:
        print("The graph is NOT planar. A Kuratowski subgraph (proof of non-planarity) is:")
        print(certificate.edges())
    
    return is_planar, certificate
```

```{code-cell} ipython3
# Example 1: Planar Graph
G1 = nx.Graph([(0, 1), (0, 2), (1, 2), (2, 3)])
check_graph_planarity(G1)
```

```{code-cell} ipython3
# Save Example 1 to GraphML
nx.write_graphml(G1, "./data/example1.graphml")
```

```{code-cell} ipython3
# Example 2: Non-Planar Graph (K3,3)
G2 = nx.complete_bipartite_graph(3, 3)
check_graph_planarity(G2)
```

```{code-cell} ipython3
# Save Example 2 to GraphML
nx.write_graphml(G2, "./data/example2.graphml")
```

## Summary
- We used `nx.check_planarity(G)` to determine if a graph is planar.
- The function returns a PlanarEmbedding if the graph is planar and a Kuratowski subgraph if it is not.
- The test is based on the **Left-Right Planarity Test**.

This method is useful in applications like **circuit layout design, network visualization, and graph drawing**.

+++

## Notes

A (combinatorial) embedding consists of cyclic orderings of the incident edges at each vertex. Given such an embedding there are multiple approaches discussed in literature to drawing the graph (subject to various constraints, e.g. integer coordinates), see e.g. [2].

The planarity check algorithm and extraction of the combinatorial embedding is based on the Left-Right Planarity Test [1].

A counterexample is only generated if the corresponding parameter is set, because the complexity of the counterexample generation is higher.

+++

## References
[1]
Ulrik Brandes: The Left-Right Planarity Test 2009 http://citeseerx.ist.psu.edu/viewdoc/summary?doi=10.1.1.217.9208

2]
Takao Nishizeki, Md Saidur Rahman: Planar graph drawing Lecture Notes Series on Computing: Volume 12 2004

+++

## IS_PLANAR 

+++

## Planarity: 
    A graph is planar if it can be drawn on a plane without edges crossing.

## Kuratawoski's Theorem:
    A graph is not planar if it contains a subgraph that is homeomorphic to K5 or K3,3.

## NetworkX's is_planar(G) function 
    determines whether a given graph is planar.

+++

import networkx as nx
import matplotlib.pyplot as plt

+++

## Parameters:
G: NetworkX graph
## Returns
 

bo: l
Whether the graph is planar.

```{code-cell} ipython3
# Example 3: A simple planar graph
G1 = nx.Graph([(0, 1), (0, 2)])
print("Is G1 planar?", nx.is_planar(G1))
```

```{code-cell} ipython3
# Example 4: The complete graph K5, which is non-planar
G2 = nx.complete_graph(5)
print("Is K5 planar?", nx.is_planar(G2))
```

```{code-cell} ipython3
# Visualizing the graphs
def draw_graph(G, title):
    plt.figure(figsize=(5, 5))
    nx.draw(G, with_labels=True, node_color="lightblue", edge_color="gray", node_size=500)
    plt.title(title)
    plt.show()

draw_graph(G1, "Planar Graph (G1)")
draw_graph(G2, "Non-Planar Graph (K5)")

# Save as GraphML
nx.write_graphml(G1, "./data/example3.graphml")
nx.write_graphml(G2, "./data/example4.graphml")
```

## Planar Embedding

+++ {"editable": true, "slideshow": {"slide_type": ""}}

## Planar Embedding in NetworkX

class PlanarEmbedding(incoming_graph_data=None, **attr)[source]
Represents a planar graph with its planar embedding.

The planar embedding is given by a combinatorial embedding.

+++

## Combinatorial embedding

Main article: Rotation system
An embedded graph uniquely defines cyclic orders of edges incident to the same vertex. The set of all these cyclic orders is called a rotation system. Embeddings with the same rotation system are considered to be equivalent and the corresponding equivalence class of embeddings is called combinatorial embedding (as opposed to the term topological embedding, which refers to the previous definition in terms of points and curves). Sometimes, the rotation system itself is called a "combinatorial embedding".[5][6][7]

An embedded graph also defines natural cyclic orders of edges which constitutes the boundaries of the faces of the embedding. However handling these face-based orders is less straightforward, since in some cases some edges may be traversed twice along a face boundary. For example this is always the case for embeddings of trees, which have a single face. To overcome this combinatorial nuisance, one may consider that every edge is "split" lengthwise in two "half-edges", or "sides". Under this convention in all face boundary traversals each half-edge is traversed only once and the two half-edges of the same edge are always traversed in opposite directions.

Other equivalent representations for cellular embeddings include the ribbon graph, a topological space formed by gluing together topological disks for the vertices and edges of an embedded graph, and the graph-encoded map, an edge-colored cubic graph with four vertices for each edge of the embedded graph.

+++

## Neighbor ordering:
In comparison to a usual graph structure, the embedding also stores the order of all neighbors for every vertex. The order of the neighbors can be given in clockwise (cw) direction or counterclockwise (ccw) direction. This order is stored as edge attributes in the underlying directed graph. For the edge (u, v) the edge attribute ‘cw’ is set to the neighbor of u that follows immediately after v in clockwise direction.

In order for a PlanarEmbedding to be valid it must fulfill multiple conditions. It is possible to check if these conditions are fulfilled with the method check_structure(). The conditions are:
1. Edges must go in both directions (because the edge attributes differ)
2. Every edge must have a ‘cw’ and ‘ccw’ attribute which corresponds to a correct planar embedding.

As long as a PlanarEmbedding is invalid only the following methods should be called:

1. add_half_edge()
2. connect_components()

Even though the graph is a subclass of nx.DiGraph, it can still be used for algorithms that require undirected graphs, because the method is_directed() is overridden. This is possible, because a valid PlanarGraph must have edges in both directions.

## Half edges:
In methods like add_half_edge the term “half-edge” is used, which is a term that is used in doubly connected edge lists. It is used to emphasize that the edge is only in one direction and there exists another half-edge in the opposite direction. While conventional edges always have two faces (including outer face) next to them, it is possible to assign each half-edge exactly one face. For a half-edge (u, v) that is oriented such that u is below v then the face that belongs to (u, v) is to the right of this half-edge.

+++

__init__(incoming_graph_data=None, **attr)[source]
Initialize a graph with edges, name, or graph attributes.

## Parameters:
## incoming_graph_data : input graph (optional, default: None)
Data to initialize graph. If None (default) an empty graph is created. The data can be an edge list, or any NetworkX graph object. If the corresponding optional Python packages are installed the data can also be a 2D NumPy array, a SciPy sparse array, or a PyGraphviz graph.

## attr : keyword arguments, optional (default= no attributes)
Attributes to add to graph as key=value pairs.

```{code-cell} ipython3
# Create a PlanarEmbedding instance
G = nx.PlanarEmbedding()

# Add half-edges in clockwise order
G.add_half_edge(0, 1)
G.add_half_edge(0, 2, ccw=1)
G.add_half_edge(0, 3, ccw=2)
G.add_half_edge(1, 0)
G.add_half_edge(2, 0)
G.add_half_edge(3, 0)

# Check structure validity
G.check_structure()

# Convert to standard Graph for visualization
graph = nx.Graph()
graph.add_edges_from([(0, 1), (0, 2), (0, 3)])
```

```{code-cell} ipython3
# Draw the graph
plt.figure(figsize=(5, 5))
nx.draw(graph, with_labels=True, node_color='lightblue', edge_color='gray', node_size=2000, font_size=16)
plt.title("Planar Embedding Visualization")
plt.show()
```

```{code-cell} ipython3

```

```{code-cell} ipython3
# Check if the graph is planar
print("Is the graph planar?", nx.is_planar(graph))
```

```{code-cell} ipython3
# Save as GraphML
nx.write_graphml(graph, "./data/planar_embedding.graphml")
```

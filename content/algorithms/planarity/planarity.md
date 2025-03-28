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

```{raw-cell}
CHECK PLANARITY
```

```{code-cell} ipython3
"""
# Planarity Algorithm in NetworkX

## Introduction
A graph is said to be **planar** if it can be drawn on a plane without any of its edges crossing. The `check_planarity` function in NetworkX helps determine whether a given graph is planar and provides either an embedding or a counterexample.

### Mathematical Background
A graph is **planar** if and only if it does not contain a subgraph homeomorphic to **K₅ (complete graph on 5 vertices)** or **K₃,₃ (complete bipartite graph with partition sizes 3 and 3)**. This is known as **Kuratowski’s theorem**.

The planarity check in NetworkX is based on the **Left-Right Planarity Test**, an efficient combinatorial method to determine planarity.
"""
```

```{code-cell} ipython3
import networkx as nx
import matplotlib.pyplot as plt
```

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
nx.write_graphml(G1, "example1.graphml")
```

```{code-cell} ipython3
# Example 2: Non-Planar Graph (K3,3)
G2 = nx.complete_bipartite_graph(3, 3)
check_graph_planarity(G2)
```

```{code-cell} ipython3
# Save Example 2 to GraphML
nx.write_graphml(G2, "example2.graphml")
```

```{code-cell} ipython3
"""
## Summary
- We used `nx.check_planarity(G)` to determine if a graph is planar.
- The function returns a PlanarEmbedding if the graph is planar and a Kuratowski subgraph if it is not.
- The test is based on the **Left-Right Planarity Test**.

This method is useful in applications like **circuit layout design, network visualization, and graph drawing**.
"""
```

```{code-cell} ipython3

```

```{raw-cell}
IS_PLANAR 
```

```{code-cell} ipython3
"""
Planarity: A graph is planar if it can be drawn on a plane without edges crossing.

Kuratawoski's Theorem: A graph is not planar if it contains a subgraph that is homeomorphic to K5 or K3,3.

NetworkX's is_planar(G) function determines whether a given graph is planar.
"""
```

```{code-cell} ipython3
import networkx as nx
import matplotlib.pyplot as plt
```

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
nx.write_graphml(G1, "example3.graphml")
nx.write_graphml(G2, "example4.graphml")
```

```{code-cell} ipython3

```

```{raw-cell}
Planar Embedding
```

```{code-cell} ipython3
"""
# Planar Embedding in NetworkX

This notebook demonstrates how to use NetworkX's `PlanarEmbedding` class.
"""
```

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

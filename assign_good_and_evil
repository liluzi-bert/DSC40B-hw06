from collections import deque

def assign_good_and_evil(graph):
    """
    Return a dict {node: 'good' or 'evil'} if the undirected graph is bipartite.
    Return None if no such labeling exists.

    Expected graph API (dsc40graph.UndirectedGraph):
      - graph.nodes() -> iterable of nodes
      - graph.neighbors(u) -> iterable of neighbors of u
    """
    # Try to get nodes robustly
    if hasattr(graph, "nodes"):
        nodes = list(graph.nodes() if callable(graph.nodes) else graph.nodes)
    elif hasattr(graph, "vertices"):
        v = graph.vertices
        nodes = list(v() if callable(v) else v)
    else:
        try:
            nodes = list(graph)  # if the graph is iterable over nodes
        except TypeError:
            raise AttributeError("Graph must provide nodes() or be iterable over nodes.")

    # Helper to get neighbors (adjust if your API differs)
    def nbrs(u):
        if hasattr(graph, "neighbors"):
            return graph.neighbors(u)
        if hasattr(graph, "adj"):         # e.g., dict u -> set of neighbors
            return graph.adj[u]
        raise AttributeError("Graph must provide neighbors(u) or adj mapping.")

    label = {}  # node -> 'good'/'evil'
    opposite = {'good': 'evil', 'evil': 'good'}

    for start in nodes:
        if start in label:
            continue

        # Start a new component with 'good'
        label[start] = 'good'
        q = deque([start])

        while q:
            u = q.popleft()
            for v in nbrs(u):
                if v not in label:
                    label[v] = opposite[label[u]]
                    q.append(v)
                elif label[v] == label[u]:
                    # Edge connects same-labeled endpoints -> not bipartite
                    return None

    return label

#Informed Search Algorithms

def aStarAlgo(start_node, stop_node):
    open_set = {start_node}
    closed_set = set()

    g = {}  # store distance from starting node
    parents = {}  # parents contains an adjacency map of all nodes

    # distance of starting node from itself is zero
    g[start_node] = 0

    # start_node is root node i.e it has no parent nodes
    parents[start_node] = start_node

    while len(open_set) > 0:
        n = None

        # node with lowest f() = g() + h()
        for v in open_set:
            if n is None or g[v] + heuristic(v) < g[n] + heuristic(n):
                n = v

        if n is None:
            print("Path does not exist!")
            return None

        # if the current node is the stop_node
        if n == stop_node:
            path = []

            while parents[n] != n:
                path.append(n)
                n = parents[n]

            path.append(start_node)
            path.reverse()

            print("Path found:", path)
            return path

        # if the node has neighbors
        if Graph_nodes.get(n) is not None:
            for (m, weight) in get_neighbors(n):

                if m not in open_set and m not in closed_set:
                    open_set.add(m)
                    parents[m] = n
                    g[m] = g[n] + weight

                else:
                    if g.get(m, float('inf')) > g[n] + weight:
                        g[m] = g[n] + weight
                        parents[m] = n

                        if m in closed_set:
                            closed_set.remove(m)
                            open_set.add(m)

        # remove n from open_set and add to closed_set
        open_set.remove(n)
        closed_set.add(n)

    print("Path does not exist!")
    return None


# function to return neighbors and their distances
def get_neighbors(v):
    if v in Graph_nodes:
        return Graph_nodes[v]
    return None


# heuristic distances
def heuristic(n):
    H_dist = {
        'A': 11,
        'B': 6,
        'C': 5,
        'D': 7,
        'E': 3,
        'F': 6,
        'G': 5,
        'H': 3,
        'I': 1,
        'J': 0
    }
    return H_dist[n]


# graph structure
Graph_nodes = {
    'A': [('B', 6), ('F', 3)],
    'B': [('A', 6), ('C', 3), ('D', 2)],
    'C': [('B', 3), ('D', 1), ('E', 5)],
    'D': [('B', 2), ('C', 1), ('E', 8)],
    'E': [('C', 5), ('D', 8), ('I', 5), ('J', 5)],
    'F': [('A', 3), ('G', 1), ('H', 7)],
    'G': [('F', 1), ('I', 3)],
    'H': [('F', 7), ('I', 2)],
    'I': [('E', 5), ('G', 3), ('H', 2), ('J', 3)]
}

aStarAlgo('A', 'J')

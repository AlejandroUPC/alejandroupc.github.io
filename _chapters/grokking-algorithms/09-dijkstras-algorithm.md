---
title: Chapter 9 - Dijkstra's Algorithm
order: 9
date: 2026-07-12
tags: [books, algorithms, grokking-algorithms]
---

## Chapter intro

- Still in the graph world, weighted graphs (graphs with weights on their edges) are presented.

- Dijkstra's famous algorithm finds the shortest (cheapest) path in a graph.

- Graphs with negative-weight edges, where Dijkstra's algorithm does not work.



## Working with Dijkstra's algorithm

BFS told us how to get from `A` to `B` using the fewest jumps possible. The issue is that every jump may have a cost. That cost could represent money, time, or anything else we want to minimize.


If we start with this base graph:

![dj base]({{ '/assets/images/grokking_algos_chapter_9/dj-base.svg' | relative_url }}){: .center-img }

If we assume the edges are just the minutes in time, what's the shortest path to go from start to finish?

If we used BFS, it would choose the path with the fewest edges, which takes 7 minutes in this graph:

![bfs dj]({{ '/assets/images/grokking_algos_chapter_9/bfs-dj.svg' | relative_url }}){: .center-img }

That is not the fastest path. How does Dijkstra's work?

1. Find the unprocessed node with the lowest tentative total cost from the start.
2. Update the cost of the neighbors of this node.
3. Mark the node as processed and repeat until there are no more reachable nodes to process.
4. Reconstruct the final path using the recorded parents.


So first, from start we write our table:

```markdown
|  Node  | time to node|
|--------|-------------|
|    A   |       6     |
|    B   |       2     |
| FINISH |    UNKNOWN  |

```


It seems it's clear jumping to `B` is cheapest, so we move there and update the table:

```markdown
| Node   |time to node |
|--------|-------------|
|    A   |      3+2    |
|    B   |       2     |
| FINISH |       7     |

```

So now:

1. We found a cheaper way to `A`, from node `B` is `5` rather than `6`, so small win here.
2. We now know how to get to `FINISH`, with a cost of 7 from `START` -> `B`.

Those two steps, now that found cheaper ways (`6` to `5` and `UNKNOWN` to `7`) we have updated our table.

Now we move to node `A`:

```markdown
| Node   |time to node |
|--------|-------------|
|    A   |       5     |
|    B   |       2     |
| FINISH |       6     |

```

So it takes `6` minutes to get to finish and we went through all the nodes (`A` and `B`), and with our table we know the cheapest:

1. It takes 5 minutes to get `A`.
2. It takes 2 minutes to get to `B`.
3. It takes 6 minutes to get to `FINISH`.



So Dijkstra's algorithm can be simplified as:

1. Find the unprocessed node with the lowest tentative total cost.
2. Check if there is a cheaper path to the neighbors of this node, if so, update the costs.
3. Mark it as processed and repeat. If we only need one destination, we can stop when that destination is processed.
4. Reconstruct the final path from the parent pointers.


## Terminology

The value on an edge between nodes in a graph is called its weight. For a graph with non-negative weights, Dijkstra's algorithm computes shortest paths. In an unweighted graph, where every edge has the same cost, BFS is enough.

Graphs can have cycles, whether they are weighted or unweighted. With non-negative weights, processing the cheapest unprocessed node still guarantees that its shortest distance is final; a cycle does not invalidate Dijkstra's algorithm.

Graphs can also be directed (the edges determine which way we can travel) or undirected (we can travel in both directions). Dijkstra's algorithm works with either kind, including cycles, as long as all edge weights are non-negative. If negative edges are allowed, use an algorithm such as Bellman–Ford instead. A negative cycle means that no finite shortest path exists.


## Trading for a piano

We have a set of items that we can trade for one another. For example, item `A` can be traded for `B` or `C` plus some money. We can model this as a directed graph with non-negative weights representing the extra money required. We start with a book and want to end up with a piano while spending as little additional money as possible:

![dijkstra-example]({{ '/assets/images/grokking_algos_chapter_9/dij-ex.svg' | relative_url }}){: .center-img }


So we can start from the book, build the node table, and start moving around the graph. Note that we also need a parent column, which we did not use in the previous examples.


```markdown
|parent| node | cost |
|------|------|------|
| Book |  LP  |   5  |
| Book |Poster|   0  |
|   -  |Guitar| inf  |
|   -  |Drums | inf  |
|   -  |Piano | inf  |
```

The cheapest unprocessed node is `Poster`, so we process it and update the table:


```markdown
|parent| node | cost |
|------|------|------|
| Book |  LP  |   5  |
| Book |Poster|   0  |
|Poster|Guitar|  30  |
|Poster|Drums |  35  |
|   -  |Piano | inf  |
```

Now we could move to either `Guitar` or `Drums` from `Poster`, but the cheapest unprocessed node is `LP` with a total cost of 5. We process `LP` and see what happens:

```markdown
|parent| node | cost |
|------|------|------|
| Book |  LP  |   5  |
| Book |Poster|   0  |
|  LP  |Guitar|  20  |
|  LP  |Drums |  25  |
|   -  |Piano | inf  |
```

The routes to `Guitar` and `Drums` became 10 cheaper because, although `Poster` is cheaper than `LP` (0 versus 5), its outgoing edges are more expensive. The cheapest unprocessed node is now `Guitar`, so we process it:


```markdown
|parent| node | cost |
|------|------|------|
| Book |  LP  |   5  |
| Book |Poster|   0  |
|  LP  |Guitar|  20  |
|  LP  |Drums |  25  |
|Guitar|Piano |  40  |
```

We have found a route to `Piano` with a cost of 40. Processing `Guitar` improves that route to 35:

```markdown
|parent| node | cost |
|------|------|------|
| Book |  LP  |   5  |
| Book |Poster|   0  |
|  LP  |Guitar|  20  |
|  LP  |Drums |  25  |
|Guitar|Piano |  35  |
```


We add the cost to `Guitar` to the new edge cost of 15 and reach `Piano` for 35. The cheapest trade is `Book -> LP -> Guitar -> Piano`, as shown by the parent column.



## Negative-weight edges

This example shows why Dijkstra's does not work with negative edge weights. Imagine a graph with three items:


![dijkstra-neg-example]({{ '/assets/images/grokking_algos_chapter_9/dijk-neg.svg' | relative_url }}){: .center-img }


The person owning the poster pays us 7 units of money if we give them our LP. To get from a book to a poster, we therefore have two routes:

1. A direct route costing 0.
2. A route costing -2 (we receive 2 units) through the LP first.

The second route is better. If we add another item that the poster can be traded for, the drums, the cost would be either 35 or 33:

![dijkstra-neg-extended]({{ '/assets/images/grokking_algos_chapter_9/ex-neg-ext.svg' | relative_url }}){: .center-img }


Our initial table then looks like:

```markdown
| node | cost |
|------|------|
|  LP  |   5  |
|Poster|   0  |
| Drums|  inf |
```

Our first step is then to process `Poster`, whose tentative cost is `0`:

```markdown
| node | cost |
|------|------|
|  LP  |   5  |
|Poster|   0  |
| Drums|  35  |
```

The next cheapest node is `LP`. Processing it reveals a cheaper route to `Poster`, but `Poster` has already been processed:

```markdown
| node | cost |
|------|------|
|  LP  |   5  |
|~Poster~|   ~-2~ |
| Drums|  35  |
```
We just updated `Poster` to `-2`, which is less than the `0` we had earlier. We found a cheaper route, but Dijkstra's cannot safely revise a node after processing it because that guarantee depends on all edge weights being non-negative.

If we continued, we would already have processed the nodes needed to reach `Drums`, so the algorithm would incorrectly keep the cost at 35.

The algorithm did not find the cost-33 path.


## Implementation

We will now implement Dijkstra's algorithm in Python using this graph:

![dijkstra-impl]({{ '/assets/images/grokking_algos_chapter_9/dij-impl.svg' | relative_url }}){: .center-img }

We represent the graph with nested dictionaries. Each key is a node, and its value is another dictionary containing each neighbor and the edge weight:


```python
graph = {}
graph["you"] = {"alice":6, "bob":2}
# rest of nodes
graph["alice"] = {"clarie":1}
graph["bob"] = {"clarie": 5, "alice":3}
graph["clarie"] = {}
```


We also create a cost table. It stores the cheapest tentative cost currently known for each node:

```python
import math

costs = {
    "you": 0,
    "alice": 6,
    "bob": 2,
    "clarie": math.inf,
}
```

Finally, we keep track of each node's parent so that we can reconstruct the final path:

```python
parents = {
    "alice": "you",
    "bob": "you",
    "clarie": None,
}

```

We also need to avoid processing the same node more than once, so we keep processed node names in `processed = set()`.


Now we can write the algorithm with the following steps:


1. While there are nodes to process, find the unprocessed node with the lowest cost.
2. Update the costs of its neighbors.
3. If a cost is updated, update that neighbor's parent too.
4. Mark the current node as processed and repeat.

The pseudocode looks like this:


```python
node = find_cheapest_node(costs, processed)
while node is not None: # we have nodes to process
    cost = costs[node]
    neighbours = graph[node]
    for neighbour in neighbours:
        new_cost = cost + neighbours[neighbour]
        if costs[neighbour] > new_cost:
            costs[neighbour] = new_cost
            parents[neighbour] = node
    processed.add(node)
    node = find_cheapest_node(costs, processed)
```

The helper that finds the lowest-cost unprocessed node is:

```python
def find_cheapest_node(costs, processed):
    min_cost = math.inf
    lowest_cost_node = None
    for node in costs:
        cost = costs[node]
        if cost < min_cost and node not in processed:
            min_cost = cost
            lowest_cost_node = node
    return lowest_cost_node
```


Here is a complete implementation. It also records parents and reconstructs the path:


```python
import math


def find_cheapest_node(costs: dict, visited: set) -> str | None:
    cheapest_node = None
    cheapest_cost = math.inf
    for node, cost in costs.items():
        if node in visited or cost == math.inf:
            continue
        if cost < cheapest_cost:
            cheapest_node = node
            cheapest_cost = cost
    return cheapest_node


def dijkstra(graph: dict, start: str, target: str) -> tuple[float, list[str]]:
    costs = {node: math.inf for node in graph}
    costs[start] = 0
    parents = {node: None for node in graph}
    processed = set()

    current_node = find_cheapest_node(costs, processed)
    while current_node is not None:
        current_cost = costs[current_node]
        for neighbour, edge_cost in graph[current_node].items():
            new_cost = current_cost + edge_cost
            if new_cost < costs[neighbour]:
                costs[neighbour] = new_cost
                parents[neighbour] = current_node

        processed.add(current_node)
        if current_node == target:
            break
        current_node = find_cheapest_node(costs, processed)

    if costs[target] == math.inf:
        return math.inf, []

    path = []
    current_node = target
    while current_node is not None:
        path.append(current_node)
        current_node = parents[current_node]
    path.reverse()
    return costs[target], path


def main() -> None:
    graph = {
        "you": {"alice": 6, "bob": 2},
        "alice": {"clarie": 1},
        "bob": {"alice": 3, "clarie": 5},
        "clarie": {},
    }
    cost, path = dijkstra(graph, "you", "clarie")
    print(f"cost={cost}, path={' -> '.join(path)}")


if __name__ == "__main__":
    main()
```

We build the graph and initialize the cost and parent tables. We then repeatedly find the cheapest unprocessed node and check whether reaching each neighbor through it is cheaper than the neighbor's current cost. If it is, we update both the cost and the parent.

The output of this is:


```markdown
Found cheaper path to neighbour='alice' from current_node='bob', 2 was now its 5
Found cheaper path to neighbour='clarie' from current_node='bob', 2 was now its 7
Found cheaper path to neighbour='clarie' from current_node='alice', 5 was now its 6
|--------|--------|
|  node  |  cost  |
|--------|--------|
|  you   |   0    |
| alice  |   5    |
|  bob   |   2    |
| clarie |   6    |
|________|________|
```


## Closing up

- BFS can find the shortest path in an unweighted graph. DFS is not guaranteed to find the shortest path because it explores one branch deeply before considering alternatives.

- Dijkstra's algorithm finds shortest paths in graphs with non-negative edge weights, including directed, undirected, and cyclic graphs. Bellman–Ford can be used when negative edges are present.

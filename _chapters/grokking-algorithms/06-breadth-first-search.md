---
title: Chapter 6 - Breadth-first search
order: 6
date: 2025-12-10
tags: [books, algorithms, grokking-algorithms]
---

## Chapter intro

We learn a new structure, graphs, and a new algorithm to to work with them, breadth-first-search that allows to know the shortest path, for example, between two nodes.
Graphs can be directed, or undirected, and also what is topological sort, a sorting algorithm to expose dependencies between nodes.

## Introduction

A graph models a set of connections, for example, your friends (and the friends of your friends) in a social network.
There are two main pieces in a graph:
- Node: Represents an entity.
- Edge: Represents the connection between two entities.

For example, imagine in the graphic below every node is a person and every edge points if they are friends or not, so John is friend of Elena, and Elena considers John her friend, bidirectionally; but John think's Peter is his friend, but not the other way around; Peter does not consider JJohn to be his friend but Marc. Marc does not consider to be friend with anyone.

![graph-friendship]({{ '/assets/images/grokking_algos_chapter_6/simple-graph-friendhsip.svg' | relative_url }}){: .center-img }

TO define an incomming connection, in the node's perspective, its an in-neighbor, so John's is a Peter in-neighbor, and Mark is an out-neighbor of Peter.

## Breadth-first search

Applied to graphs, can answer (among others):

1. Is there a path between two nodes?
2. What is the shortest path between two nodes?

To answer the first question imagine you have a map with some city names and you wan to go from city A to a city to watch a Football game, imagine the following scenario:


![graph-friendship]({{ '/assets/images/grokking_algos_chapter_6/bfs-simple-ex-1.svg' | relative_url }}){: .center-img }

The logic you might want to follow here is to list all of the cities and ask, does this city host a Football game? If so you're done, otherwise you just cross it from the list and move.

Now imagine the cities you checked initially none host the Football game, but you are convinced there must be at least a city, you might have to dig futher and you start also considering the cities that are next to the previous cities, so your graph now is starting to grow:


![graph-friendship]({{ '/assets/images/grokking_algos_chapter_6/bfs-simple-ex-2.svg' | relative_url }}){: .center-img }

Sow now, whenever you visit a city you add all of its neighbor cities to the list, so you make sure you might potentially look for all the cities (until reaching cities with no neibourgths, like G).

This is a first implementation of what breadth-first search is and a way to see if there is a way between two nodes.

### Finding the hosrtes path

Knowing there is a way to at least a city to a city that hosts a Football game, there might be a shortest way to reach it and we probably want it.
Another concept of the graph algorithms are the degree of connection, aka how many jumps you have to do to reach a node.

In the previous example, B was a first degree connection to A, and then C&D were second degree to A. You might want to be sure, that before you start checking the futhest cities you have checked the clostest ones, right? You dont want to drive until I, which is 5th degree, to find out that C, second degree, has also a Football game.

Well the list we were talking before where you keep checking the cities should already handle this, it should be ordered by degree when you check for the Football game, so:

- B (1st Degree)
- C (2nd Degree)
- D (2nd Degree)
- G (3rd Degree)
- H (3rd Degree)
- E (3rd Degree)
- F (4th Degree)
- I (5th Degree)

You kind of wnat to radiate outwards from the starting point, so BFS (when implemented correctly) does not just find the way but the shortest.

### Queues

Queues makes sense regarding the point above and are useful to make sure certain order is maintained based on how you add items. They work exactly like they do in real live when you are making a line to pay in a supermarket, FIFO (First In First Out), however arrives first in the line will be attended and dispatched by the cashier.

There are two operations:

- Enqueue: Adds an item at the end of the queue.
- Dequeue: Take the first item from the start of the queue.

So imagine you have the following graph:

![graph-friendship]({{ '/assets/images/grokking_algos_chapter_6/simple-graph-friendhsip.svg' | relative_url }}){: .center-img }

What is the shortest way to H? You can start building the list and on each node ask, can I reach `H` from here? 

The full queue would be:

- B (1st Degree)
- C (2nd Degree)
- D (2nd Degree)
- E (3rd Degree)
- G (3rd Degree)
- F (4th Degree)

We go through the queue:

- Can we reach H from the first dateegree nodes (just B?) -> No.
- Can we reach H from the 2nd degree nodes (C or D)? -> Yes, for C! Done.

We can also reach from `G`, which is third degree or even with F which is 4th.

## Implementing the graph


So how can be a graph represented in code? One might think its kind of a linked list that could have multiple "next" nodes, like an array, but maybe a recent structure we saw that combines hash tables and arrays might be sufficient, where we have the constraint of uniqueness per record; a node is unique in the graph and its relations are just stored in arrays.

We could then start building our graph based in the previous example we saw:

```python
cities = {}
cities["A"] = ["B"]
citites["B"] = ["C", "D"]
cities["C"] = ["G", "H"]
cities["G"] = ["H"]
cities["D"] = ["E"]
cities["E"] = ["F"]
cities["F"] = ["H"]
```

A small reminder that the ordering (as we saw in the degrees) matters, by default hash table does not offer ordering, although Python implementation do.


Small note, there is a difference where we map the relations between two nodes by direction or not:
1. Directed graph: When the edges have arrow, so the relation in/out neighbour matters. Imagine a graph representing poker players and who owns money who, seems obvious that two player A and B  can have different amounts of money owning each other.
2. Undirected graph: This means that it does not matter, e.g a family tree where you want to build a tree between siblings. Sibling A is sibling of B, and B is of A. The relation is the same, the reprocity does not matter or any other kind of attribute (e.g amount of money).

Now, what would the algorithm look like  for the following example where we have been looking for someone who sells a pair of shoes we want to buy so bad, to do so we have received the following graph:



![graph-shoes]({{ '/assets/images/grokking_algos_chapter_6/bfs-shoe-graph.svg' | relative_url }}){: .center-img }



```python

# we build a hardcoded function where we return  the seller is True if its name is Karl
def is_shoe_seller(name: str) -> bool:
    return name == "karl"


def find_shoe_seller() -> bool:
    from collections import deque

    search_queue = deque()
    graph = {}

    graph["you"] = ["anna", "marc", "laura"]
    graph["anna"] = ["john", "peter"]
    graph["laura"] = ["peter"]
    graph["marc"] = ["karl", "jess"]
    graph["john"] = []
    graph["peter"] = []
    graph["karl"] = []
    graph["jess"] = []

    search_queue += graph["you"]  # we init the queue with us.

    iteration_cter = 0
    while search_queue:
        print(f"Working on iteration {iteration_cter}.")
        print(f"Queue is {search_queue}.")
        current_check_person = search_queue.popleft()
        if is_shoe_seller(current_check_person):
            print(f"Congrats, {current_check_person} is selling the shoes.")
            return True  # we stop and return
        else:
            print(
                f"{current_check_person} is not selling shoes, we add its neighbours: {graph[current_check_person]}."
            )
            search_queue += graph[
                current_check_person
            ]  # if not found we add their neighbours

        iteration_cter += 1
    return False  # queue got empty


if __name__ == "__main__":
    find_shoe_seller()
```

Now if we run the code above we can see how the algorithm behaves based on what we have learnt:

```plaintext
Working on iteration 0.
Queue is deque(['anna', 'marc', 'laura']).
anna is not selling shoes, we add its neighbours: ['john', 'peter'].
Working on iteration 1.
Queue is deque(['marc', 'laura', 'john', 'peter']).
marc is not selling shoes, we add its neighbours: ['karl', 'jess'].
Working on iteration 2.
Queue is deque(['laura', 'john', 'peter', 'karl', 'jess']).
laura is not selling shoes, we add its neighbours: ['peter'].
Working on iteration 3.
Queue is deque(['john', 'peter', 'karl', 'jess', 'peter']).
john is not selling shoes, we add its neighbours: [].
Working on iteration 4.
Queue is deque(['peter', 'karl', 'jess', 'peter']).
peter is not selling shoes, we add its neighbours: [].
Working on iteration 5.
Queue is deque(['karl', 'jess', 'peter']).
Congrats, karl is selling the shoes.
```

Do you see something odd on iteration 3? Yes, peter is twice so we might be checking it twice and all its neibourghs, which sounds inefficient right? We might want to be able to keep track of all of the neibourghs we visited:

#TODO: revisit example and make sure it matches the example of Peggy.

What is the running time of such an algorithm?

There are two kind of operations in our code, the first one is relataed to the size of the network. In a worst case scenario we will go trhough all the nodes in the graph, so we are looking at something like `O(n)`.

Finally we are also adding all of the visited people in a list, which takes `O(1)` but depends also on the number of people, so its `O(number of people + number of edges)` and usually written as `O(V+E)`.




## Closing up


- BFS tells you the cheapest path between two edges, if any.
- If you are facing a problem that's about searching the shortest path, try to model it as a graph.
- Directed grah has arrows and relationships follow the direction of the arrow, undirected dont have arrows and maps the relationship in a bidirectional manner.
- Queues are FIFO, stacks are LIFO.
- Adding people and the degree of in which they are processed matter.
- Check visited nodes to avoid infinite loops.



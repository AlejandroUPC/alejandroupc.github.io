---
title: Anex A - Performance of AVL trees
order: 14
date: 2026-07-21
tags: [books, algorithms, grokking-algorithms]
---

## Chapter intro

This is a small appendix to understand why AVL trees perform well. The important idea is simple: the height of the tree controls how many nodes we may need to visit, so keeping the height small keeps searching, inserting, and deleting fast.

## The height of a tree

For a binary search tree, searching starts at the root and follows one child at each step. If the tree has height `h`, a search takes `O(h)` time.

If the tree is balanced, its height is around `O(log n)`, where `n` is the number of nodes. This is because each level can contain roughly twice as many nodes as the previous one:

```text
height 0: 1 node
height 1: 2 nodes
height 2: 4 nodes
height 3: 8 nodes
```

So a balanced tree with many nodes can still be searched in a small number of jumps.

The problem is that a normal binary search tree does not guarantee this. If we insert values in sorted order, it can become a chain:

```text
10
  \
   20
     \
      30
        \
         40
```

This tree has height `n - 1`, so searching can become `O(n)`, which is basically the same weakness as a linked list.

## Why AVL trees stay fast

An AVL tree is a binary search tree that keeps the balance factor of every node between `-1` and `1`:

```text
balance factor = height(right subtree) - height(left subtree)
```

Whenever an insertion or deletion makes a node too unbalanced, the tree uses a rotation to reduce its height while preserving the binary search tree ordering.

The tree does not need to be perfectly balanced. It only needs to prevent one side from becoming much taller than the other. That is enough to guarantee that the height remains `O(log n)`.

## A small proof of the height

To understand the guarantee, we can ask a different question: what is the minimum number of nodes an AVL tree can have for a given height?

For a tree of height `h`, the smallest valid AVL tree has one subtree of height `h - 1` and another of height `h - 2`. Therefore:

```text
minimum_nodes(h) = 1 + minimum_nodes(h - 1) + minimum_nodes(h - 2)
```

The first values look like this:

```markdown
| height | minimum nodes |
|--------|---------------|
|   0    |       1       |
|   1    |       2       |
|   2    |       4       |
|   3    |       7       |
|   4    |      12       |
|   5    |      20       |
|   6    |      33       |
```

The number of nodes grows exponentially as the height grows. Turning that around means the height grows logarithmically as the number of nodes grows:

```text
height = O(log n)
```

That is the reason AVL operations remain fast even when the tree contains many values.

## Operation performance

All of the main operations depend on the tree height:

```markdown
| operation | normal BST, worst case | AVL tree |
|-----------|------------------------|----------|
| search    |         O(n)           |  O(log n)|
| insert    |         O(n)           |  O(log n)|
| delete    |         O(n)           |  O(log n)|
```

Searching follows one path from the root, so it takes `O(log n)` in an AVL tree. Insertion first searches for the right position and then may rotate some nodes. Deletion also follows a path and may need to rebalance several ancestors. The path still has logarithmic length, so both operations remain `O(log n)`.

A single rotation only changes a small number of pointers, so the rotation itself is `O(1)`. The important part is that we may need to check the ancestors on the path back to the root, which is why the complete insertion or deletion is `O(log n)`.

The tree stores one node for every value, so its space complexity is `O(n)`.

## AVL trees compared with other structures

AVL trees are useful when we need ordered data and frequent searches:

```markdown
| structure     | search       | insert       | useful when                    |
|---------------|--------------|--------------|--------------------------------|
| unsorted array| O(n)         | O(1)         | data is simple and unsorted    |
| sorted array  | O(log n)     | O(n)         | searches are common            |
| linked list   | O(n)         | O(1)         | inserting nodes is the priority|
| AVL tree      | O(log n)     | O(log n)     | ordered data changes often    |
```

AVL trees are stricter about balance than some other self-balancing trees. This can mean more rotations during updates, but it also gives very reliable search performance.

For data stored on disk, B-trees are usually a better choice because each node stores many keys and children. That lets one disk read bring in more useful data.

## Closing up

- The height of a search tree controls the cost of its operations.

- An unbalanced BST can become a linked list and take `O(n)` to search.

- AVL trees keep every balance factor between `-1` and `1`.

- Rotations preserve the BST ordering while reducing the height of an unbalanced subtree.

- AVL search, insertion, and deletion are all `O(log n)` in the worst case.

- The tree uses `O(n)` space because it stores one node for each value.

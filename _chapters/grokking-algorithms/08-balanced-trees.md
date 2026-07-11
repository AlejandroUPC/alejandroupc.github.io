---
title: Chapter 8 - Balanced Trees
order: 8
date: 2026-07-11
tags: [books, algorithms, grokking-algorithms]
---

## Chapter intro

There is a new structure, Binary Search Trees (BSTs), and some concepts like balancing a tree, AVL trees and how they perform (they seem to perform quite well when compared to arrays and linked lists weakness).

## A balancing act

When going back into precious chapters, there was a clear comparision between arrays and linked lists. Searching data into (sorted) arrays seemed to be quite effective at `O(logn)`, but adding data could be annoying, we had to move all the elements around, leaving us at `O(n)`.
Linekd lists tho were `O(1)` when adding data, just move some pointers around, but searching data was worse off than arrays at `O(n)`.

So if we had this table:

```markdown

|               |   search  | insert |
|---------------|-----------|--------|
|  sorted array | O(log(n)) |  O(n)  |
|  linked list  |   O(n)    |  O(1)  |
|  new structure|   ????    |   ???  |
```


This new structure is a tree, a **balanced** BST, emphasis on balanced.

This is what a BST looks like:




There are a few conditions/copnstraints to be classified as a balanced, one of the most important and that will make our searches faster is that **the values on the left nodes are smaller than the parent, and the right ones always bigger**.


So if we wanted to search a number, we e.g 2 we could:

1. Start from root, 8, is 2 smaller than 8? Go left
2. Is 6 smaller than 2? Go left
3. We are in 2

#### Shortest trees are faster

So as you can expect from the previous example, the shortes a tree is the fastest we can search it, we introduce the concept of height. For example, for a 7 node tree we could have the previous example where it was perfectly balanced:

![standard bst]({{ '/assets/images/grokking_algos_chapter_8/bst-simple.svg' | relative_url }}){: .center-img }

Where there is a total of 2 jumps to go from root to leaf, so the height is 2.

But imagine this tree, which is the worst case:

![worst case bst]({{ '/assets/images/grokking_algos_chapter_8/worst-case-stree.svg' | relative_url }}){: .center-img }

Now to get from the first to the last element we need a total of 6 jumps, making it's height 6, and really you can see that it would take `O(n)` to take find the last time, it's pretty much a linked list?

In the best case scenario the height is `O(logn)`,  and so will be the search time complexity (small refreshed `log_2(7) ~= 2.8`.


## AVL Treees: A type of balanced tree

AVL trees are a type of self-balancing BST, they maintain all the time a height of `O(logn)` ensuring the search will be the fastest possible.

### Rotations

Rotations is moving nodes to end up with a new arrangement, so the tree becomes balanced.

Imagine we have a heavy tree leaning to the right as

```markdown
  10
    \
     20
       \
        30
```

Why need to rotate it left (left because this is right-heavy), what is the way to have a tree of lower height that maintains the binary tree properties? Rotating the 20 to the left and making the new root:

```markdown
     20
    /  \
  10    30

```

Same would apply to this case, where the tree is left-heavy, we'd rotate right:

```markdown
      30
     /
    20
   /
  10
```

But, when do we need exactly to rotate? We can understand a rotation is fine as long as the diff between its leaf-legs is 1, e.g if we add a new node to the previous tree:

```markdown
     20
    /  \
  10    30
          \
           40
```

 Now if we add 50, we need to rotate left:

```markdown
     20
    /  \
  10    30
          \
           40
             \
              50
```

What node do we rotate? There might be multiple valid solutions but the way AVL works is that it starts from the unbalanced side from the left top:

1. The first node 50, is fine its a leaf
2. 40 is still fine, its just one child
3. But 30 is already 2 jumps to leaf, its height its too big compared to the other half which has just one, so we rotate the 30.

So we get:

```markdown
       20
      /  \
    10    40
          /  \
        30    50
```

#### How does AVL tree know its time to rotate?

Well we introduce the concepts of height, or balance factor, which this last can be computed as:

`bf = height(right subtree) - height(left subtree)`

In this chapter, a leaf has height `0` and an empty subtree has height `-1`. So:

- `bf = 0` means the node is balanced
- `bf = 1` or `bf = -1` is still fine
- `bf < -1` or `bf > 1` means we need to rebalance

This information needs to be stored on every node, from root to leaf ideally, for example a left-heavy just by one search binary tree:

![bf-1]({{ '/assets/images/grokking_algos_chapter_8/bf-1.svg' | relative_url }}){: .center-img }

It would be `0` if balanced, and positive if the right leg was heavier. If for the above we added one more node to the left the balance factor would be `-2`, and this is when it knows it needs rebalancing.

A more visual example would be:

![avl tree example]({{ '/assets/images/grokking_algos_chapter_8/avl-tree-example.svg' | relative_url }}){: .center-img }

In this example, the first unbalanced node is `10`:

- `2` is a leaf, so it is fine
- `5` is still fine
- `10` becomes too left-heavy

So we rotate right at `10`.

After the rotation, the subtree becomes balanced again:

![avl balanced]({{ '/assets/images/grokking_algos_chapter_8/avl-balaced.svg' | relative_url }}){: .center-img }

So to unpack what we saw in here before moving forward:

- Binary tree isa  type of tree where eaceh node has a t most 2 children.

- Binary Search Trees is a type of binary tree where all left children are smaller than parent, and all right children are bigger than parent, they are performant if we can guarantee that height remains `O(logn)`.

- AVL's help us to guarantee BSTs remain on that height by rotating using balance factor and height.

Insertions then are very similar than searching, we just have to search the worse case scenario (the leaft) and just insert, so we can assume that for these types of trees insertion is `O(logn)`, so thats it! BST that are balanced are the best of both worlds:

```markdown

|               |    search  |   insert   |
|---------------|------------|------------|
|  sorted array |  O(log(n)) |    O(n)    |
|  linked list  |    O(n)    |    O(1)    |
|        bst    |   O(log(n))| O(log(n))  |
```


## Splay Trees


Not a lot explained here, a kind of cached tree where recently visited nodes are moved to the root, so recent nodes get clustered around the root.
They do not guarantee balance so some searches might take longer than `O(logn)`, so we might have to rotate ourselves at some point.


## B-Trees

B-Trees are very important, are slightly different kidn of binary tree, used often to build databases, here is what one looks like:

![b-tree]({{ '/assets/images/grokking_algos_chapter_8/b-tree.svg' | relative_url }}){: .center-img }


Looks slightly different right:

1. Root is one node, but the their children have already three children, so it looks like it grows fast. So unlike binary trees, they can have more than two.
2. The nodes, besides root, have two keys.


#### What is their adventages?

B-trees are aimed towards physical optimization, at the end of the day computers are physical objects and whenever there is data reading there must be some kind of physical object moving to retrieve that data.
Whenever a computer tries to find some data this is called `seek` time, and this is what B-trees are optimized for.

Imagine going to the grocery store, buying an item and going back home, then going back to the grocery and so on. Wouldn't it be better to go once, try to pack as much as stuff as you can and do a single trip? This is kinda what B-trees aim for, seek once and try to load as much as you can.

B-trees spend more time (`seek`) going through the nodes (they can have multiple keys and more than two children) but we read more data in one go.

The ordering is also quite weird, if you follow the values by order (starting from left bottom) you'll see they still maintain the left smaller, right bigger property from BSTs we just saw.
It can also be observed that the number of childrens is one more as the number of keys.


## Closing up

- BSTs offer the same search performance as arrays but better insertion.

- The performance is dictated by height of the tree (number of jumps from root to leaf).

- These trees can become imbalanced, they can be balanced AVLs does this by rotating.

- B-trees are a generalization of BTS but each node can have multiple keys and children.

- B-trees read more data becase above, but once they spend some time (seek) they load more data.

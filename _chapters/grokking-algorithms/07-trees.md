---
title: Chapter 7 - Trees
order: 7
date: 2025-12-27
tags: [books, algorithms, grokking-algorithms]
---

## Chapter intro

A new structure, simimlar to graphs (or a kind of graph, even) is discovered, trees.
Different algorithms are presented and the difference between breadth-first search (which we learnt recently) and depth-first search.

Finally a very good example using Huffman coding for compression algorithms that uses trees.

## Your first tree 

Trees are a type of graph, and also have nodes and edges. Also (at least in this book), we will only use rooted trees (a node that leads to the rest).
It's relations, aka the nodes linked through edges, can also be described as parents and children(upstream node and downstream node respectively). Finally, when a tree has no children its called a leaf node:



![graph-friendship]({{ '/assets/images/grokking_algos_chapter_7/tree-elements.svg' | relative_url }}){: .center-img }

### File directory

A file, composed of folders and files, can be represented its relation as trees (heck there is even the [tree](https://linux.die.net/man/1/tree) to list and print directories as a tree format).

So imagine we have the following tree, where we decide Desktop is our root node:

```markdown

          pics
         /    \
      2001   odyssey.png
      /   \
   a.png space.png

```

Imagine we'd want to list all of the files, so we could use BFS (breadth-first search) which ensure us it will traverse all the nodes in the tree, all of 'em will be visited.

We can define the logic as:

1. Visit every node in the tree
2. If its a file, print name.
3. If its a node folder, add it to the queue to search later on.

This is similar to prev chapter for the mango seller, the difference is we do not need to keep track of visited, because this is a tree (not a graph) and there are no cycles in trees.

Again, we remember that DFS can be understood like a wave when you drop something on the water, it will keep expanding and visiting all of the nieghborus at the same level (aka radius on the wave), the following section explains another algorithm that works differently.


### A spacy odyssey: Depth-first search

There would be another way of doing this, instead of adding the nodes we keep seeing **and are a folder** to the end of the queue, why dont just go recursively until we reach a children? If its a folder we recursively look for the contents in the folder until we exhaust that path.

If we BFS used the order in which we would visit (and print) the files would have been:

```markdown
odyssey.png
a.png
space.png
```

But if we use DFS we'd see:

```markdown
a.ping
space.png
odyssey.png
```


Now the kind of do a similar thing, but there are some differences and one of the most relevants would be is that the way DFS works it cannot be used to find the shortest path (e.g you follow a deep path from a "bad" branch until the end, when the solution would have been maybe directly in the next branch).

![dfs-depth-not-always-better]({{ '/assets/images/grokking_algos_chapter_7/dfs-depth-not-always-etter.svg' | relative_url }}){: .center-img }

DFS would have gone all the way until the left branch, while BFS would have needed to check just 2 steps (radius = 2) to find the goal.


### Binary Trees

A subtype of trees which are very common, they are based on the premise that every node has two children, used on a lot of applications like Huffman coding, which is the foundation of text compression algorithms.

#### Huffman coding

We are going to be talking about text and computers, if we create a dummy file `test.txt` with the content `tilt` we can se how many bytes we need:

```shell
> cat test.txt                                                                                                                            15:11:25
tilt%
> stat -f%z test.txt                                                                                                             
4
```

(note if you get 5 mb the new line jump rm using `truncate -s -1 test.txt`.

Its 4 bytes because we are most likely using `ISO-8859-1`, which usese 1 byte (8 bits) per letter, so a the first letter we can represet is `0 0 0 0 0 0 0 0` up until `1 1 1 1 1 1 1 1`, aka `256` aka `2^8`.

(nowadays please pick unicode and encoding `UTF-8` for any project at is uses up 1-4 bytes per character).

So if we'd have to decode the word `tilt` to its bits it would be something like running this command:

```shell
 xxd -b test.txt                                                                                                                         15:11:29
00000000: 01110100 01101001 01101100 01110100                    tilt
```

The question here is, do we need really to use 8 bits per letter? Or can we reduce (compress) the amount of information? Let's pick, instead of the 256 possible combinations, how many different letters are in `tilt` and pick a binary representation.

We have a total of `3` differnt letters, so we can do a simple fixed length code with two bits:

- t = `00`
- i = `01`
- l = `10`

This is not Huffman yet, it is just the simple baseline. It works, but it still gives every letter the same amount of bits.

Huffman does something a bit smarter. It looks at how often each symbol appears, and gives the shortest paths to the most common ones. For `tilt`, `t` appears twice and `i` and `l` appear once, so one valid Huffman tree looks like this:

![huffman-tree]({{ '/assets/images/grokking_algos_chapter_7/tilt-hoffman.svg' | relative_url }}){: .center-img }

The idea is that Huffman tries to give the most common letters the shortest paths, so as you can see `t` gets the shorter code. One valid mapping here is:

- t = `1`
- i = `00`
- l = `01`

Every node in the tree stores the number of symbols it represents, or more exactly the total frequency of that branch. The root node is just the sum of all frequencies. To decode, we start at the root, follow the bits left or right until we hit a leaf, write that letter, and then start again from the root. That is why Huffman codes do not need to be the same length.

If we try the same idea with a longer phrase like `a banana`, the tree gets more interesting and you can see why the short paths matter, we can use a `Counter` to check the frequency of each word and see how we could organize our tree:

```python
>>> from collections import Counter
>>> Counter(list('a banana'))
Counter({'a': 4, 'n': 2, ' ': 1, 'b': 1})
```


Here `a` is the most common symbol, so it gets the shortest code. The exact left/right shape can change, but the important part stays the same: the frequent symbols get the small codes, and the rare ones get the longer paths. We can also infer that the counter has a length of 4, so there are really only 4 symbols we need to represent, but a total of 
symbols frequency (number of letters is 8), so the root will have an `8`.
One of the first character, the most common that should be one jump away should be then `a` (whether if you chose it to be left, a `0` or right, a `1`) is up to you:

![huffman-tree]({{ '/assets/images/grokking_algos_chapter_7/a-banana-huffman.svg' | relative_url }})

So, if we wanted to write `a banana` with one valid code, it would look something like `0 111 110 0 10 0 10 0`.


Now as you can see and as expected in Huffman, the symbols have different lenght, so if we we recieve a sequence `1110` how can we know if whether we ar reading `11 10` or `111 0`? Here, without going into much detail is that we can't assume chunks of 8 bits as we did with `8559-1` we simply have to follow digit by digit until we reach a leaf, aka a letter.

That's one of the properties of the resulting trees trying to implement Huffman coding, each letter should be in a leaf, and in trees *there is a unqiue path that leads to each leaf*, because they are acyclic.

Also the fact that we are using a rooted tree, so whenever we reach a leaft we know we have to go back up and start again.


## Closing up

- Trees are widely used everwyhere, and this was just the basic ones. 

- They are a type of graph but without cycles, and binary trees specifically have just two childrens per node.

---
title: Chapter 5 - Hash tables
order: 5
date: 2025-12-08
tags: [books, algorithms, grokking-algorithms]
---


## Chapter intro

Hash tables, (aka python dicts) are a very useful and efficient data structure and when using them we get a lot of the stuff for free, we will learn some of those under the hood.

## Introduction

Imagine you work in a warehouse where you have all of the items somewhat organized in racks, so everytime an order comes you have to go through all the racks until you find the item's price, so this sounds like a linear search `O(n)` or if it its sorted then `O(logn)` using binary search.

How great would it be if we get an search at `O(1)`? You probably need to learn about hash functions then as a start.

## Hash functions

A function where given an input you get back a number (or sequence of bits), there should be two big requierements though:

1. It needs to be deterministic(consistent?), so for the same input you get the same output; if you do `f("shoes")=1` where f is our hashing functions you need to get `1` all the time.
2. A different word, should map **always** to a different number (avoid collisions!).

So now imagine we can store all of the items to an array (remember that access on arrays is very fast), if a hash function can return an integer... maybe we can access the array by that value?

![building a hashmap]({{ '/assets/images/grokking_algos_chapter_5/build-hm.svg' | relative_url }}){: .center-img }


So after this, anytime you want to check any price that's already on the hash map its `O(1)`.

There is also an important concept besids the consistency and the fact that in this case all our values map to different indexes in the array, which is that our hash function is producing valid indexes all the time.


Caches have a lot of use cases, to name a few:

1. Modeling relationships one-to-one as we just did with the warehouse items/price example.
2. Filtering out of duplicates, its easy to find if any item already exists in a hashmap by just computing its hash function and read if there is a value.
3. Caching stuff that is resource heavy (not caching is a complex topic, here is just a basic example of a user requesting the content of a page or triggering a payload for something that is static).


## Collisions

Collisions happen when you have a key (the input passed to the function) and then it maps to the same key, this is terrible but we're lucky most of the hash maps implementations we use take care of this.

Imagine we wanted to use only the first word of the key and add it... So `A` is index `0`, `B` is `1` and so on... You map airaplane so `f("a") = 0`, and then store the price at the index `0` but then you have to add the price, so you can access fast later on, for alarm clock, so `f("a") = 0`. But wait! There is already a price for airplane there, if you overwrite and need to access it you will be getting the alarm clock price! This is a collision, and it could lead to pretty catastrophic consequences.

![hashmap collision]({{ '/assets/images/grokking_algos_chapter_5/hm-colli.svg' | relative_url }}){: .center-img }

So next time someone wants to buy an airplane and someone asks for the price, we'd say its `15$` instead of `1k$`!


A possible solution would be that whenever there is a collision, remember you can check if something exists in the cell that your hash function outputs, you maybe create a linked list? But based on the previous image, imagine we keep adding items on `A`, going through linked lists is not that fast anymore and we might pay for performance.

Two important things are:

1. Your hash function is key, not only to avoid collisions but how your data is going to be distributed in the key space.
2. If we go for the LL solution, having an unbalanced hash map (a lot of values go into a key), will be expensive.

## Performance

Hash maps are quite cool, for search, insertion and deletion take `O(1)` on its average, which is constant time, but `O(n)` on the worst case, but still compared to other structures:

```markdown
| Operation | Hash Table (Avg) | Hash Table (Worst) | Array | Linked List |
|-----------|------------------|--------------------|-------|-------------|
| Search    | O(1)             | O(n)               | O(1)  | O(n)        |
| Insert    | O(1)             | O(n)               | O(n)  | O(1)        |
| Delete    | O(1)             | O(n)               | O(n)  | O(1)        |
```

So most of the time, on average, they will have the best of arrays and LL, but worst case they are worse in some points, so we need to pick:
- A low load factor (distribution of keys).
- A good hash function.

The load factor which we just mentioned is the number of used slots in an array, so for our last example of the collisions at the last step we had 3/5 slots, because `A`, `B` and `C` were taken but `D` and `E` not, no product was added that started for those two letters. You should never reach a high load factor, and when you approach 1 you probably want to resize, which is expensive(still averages to `O(1)`) but the alternative is worse!


## Closing up

Hash functions are a very powerful structure that comes given for free in most (if not all?) the most known and used programming languages, it's somewhat simple if you think its just a hash function and an array, but as we have seen some concepts such as collisions and laod factors can appear.

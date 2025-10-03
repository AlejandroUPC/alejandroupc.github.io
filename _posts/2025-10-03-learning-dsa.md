---
layout: post
title: Learning DSA
date: 2024-10-03
tags: [cs, dsa, data-structures]
permalink: /blog/2025/10/03/learning-dsa
---

Am I too old to learn DSA? I dont know, been programming for a while and I feel like I just know the basics, would really like to master this topic, find it interesting and its an underlying foundation for a lot of stuff used daily.

Tried to half LLM a learning plan to be somewhat competent w/ DSA, I plan to do one post with every phase or chapter, depending how long it takes and go from there, hopefully you can learn something.

Each chapter contains:
1. **Core concepts** (what to learn)  
2. **Resources** (videos, articles, books)  
3. **Practice** (LeetCode problems)  
4. **Goal** (what you should master that)  

Should structure the learning by making a post per chater or phase not sure.

Additional disclaimer, ignore the problems and its #, I just added leetcode because I know the website but this might change in the future and probably will add a link to every problem I do.

---

##  Phase 1: Core Data Structures

### Chapter 1: Arrays & Strings
- **Concepts:** indexing, iteration, two pointers, sliding window, prefix/suffix sums.  
- **Resources:**  
  - [Neetcode Arrays & Hashing](https://www.youtube.com/watch?v=GrV3MTR_Uk0)  
  - [LeetCode Array & String Explore](https://leetcode.com/explore/learn/card/array-and-string/)  
  - CLRS, Ch. 2 (*Getting Started*)  
- **Goal:** confidently apply sliding window and two-pointer patterns.  

---

### Chapter 2: Hashing (Sets & Maps)
- **Concepts:** hashmaps, sets, frequency counting, collisions, when to trade space for time.  
- **Resources:**  
  - [Neetcode Hashing](https://www.youtube.com/watch?v=shs0KM3wKv8)  
  - [Hashing article (GeeksforGeeks)](https://www.geeksforgeeks.org/hashing-data-structure/)  
  - CLRS, Ch. 11 (*Hash Tables*)  
- **Goal:** solve grouping, frequency, and uniqueness problems with hashing.  

---

### Chapter 3: Linked Lists, Stacks, Queues
- **Concepts:** pointers, reversal, cycle detection, monotonic stack, BFS with queue.  
- **Resources:**  
  - [Neetcode Linked List](https://www.youtube.com/watch?v=Hj_rA0dhr2I)  
  - [Neetcode Stack](https://www.youtube.com/watch?v=wjI1WNcIntg)  
  - [Neetcode Queue](https://www.youtube.com/watch?v=okr-XE8yTO8)  
  - [LeetCode Linked List Explore](https://leetcode.com/explore/learn/card/linked-list/)  
  - CLRS, Ch. 10 (*Elementary Data Structures*)  
- **Goal:** quickly implement stack/queue logic, spot monotonic stack problems.  

---

##  Phase 2: Trees & Graphs

### Chapter 4: Binary Trees
- **Concepts:** recursion, traversals (pre/in/post-order), height, diameter, LCA.  
- **Resources:**  
  - [Neetcode Trees](https://www.youtube.com/watch?v=fAAZixBzIAI)  
  - [LeetCode Tree Explore](https://leetcode.com/explore/learn/card/data-structure-tree/)  
  - CLRS, Ch. 12 (*Binary Search Trees*)  
- **Goal:** reduce problems into recursive traversals.  

---

### Chapter 5: BSTs & Heaps
- **Concepts:** BST invariants, balanced BSTs, min-heap/max-heap, priority queues.  
- **Resources:**  
  - [Neetcode BST](https://www.youtube.com/watch?v=K0u_JqrbJeY)  
  - [Neetcode Heap](https://www.youtube.com/watch?v=HqPJF2L5h9U)  
  - CLRS, Ch. 6 (*Heapsort*)  
- **Goal:** recognize when to use heap for “top-k” and scheduling problems.  

---

### Chapter 6: Graph Traversals
- **Concepts:** adjacency list/matrix, BFS, DFS, connected components, topological sort.  
- **Resources:**  
  - [Neetcode Graphs](https://www.youtube.com/watch?v=tWVWeAqZ0WU)  
  - [LeetCode Graph Explore](https://leetcode.com/explore/learn/card/graph/)  
  - CLRS, Ch. 22 (*Graph Algorithms*)  
- **Goal:** map graph problems to BFS/DFS patterns.  

---

### Chapter 7: Shortest Paths & Union-Find
- **Concepts:** Dijkstra, Bellman-Ford, Kruskal/Prim MST, disjoint sets (union-find).  
- **Resources:**  
  - [Neetcode Dijkstra](https://www.youtube.com/watch?v=EFg3u_E6eHU)  
  - [Union-Find (GeeksforGeeks)](https://www.geeksforgeeks.org/union-find/)  
  - CLRS, Ch. 23–24 (*MST & Shortest Paths*)  
- **Goal:** solve weighted graphs & connectivity efficiently.  

---

##  Phase 3: Sorting & Searching

### Chapter 8: Sorting
- **Concepts:** merge sort, quicksort, heap sort, interval scheduling, greedy sort.  
- **Resources:**  
  - [Neetcode Sorting](https://www.youtube.com/watch?v=PgBzjlCcFvc)  
  - CLRS, Ch. 7 (*Quicksort*)  
- **Goal:** identify sorting as preprocessing step.  

---

### Chapter 9: Binary Search
- **Concepts:** binary search patterns, lower/upper bound, search-on-answer.  
- **Resources:**  
  - [Neetcode Binary Search](https://www.youtube.com/watch?v=U4yPae3GEO0)  
  - [LeetCode Binary Search Explore](https://leetcode.com/explore/learn/card/binary-search/)  
- **Goal:** master binary search variants.  

---

## Phase 4: Dynamic Programming 

### Chapter 10: 1D DP
- **Concepts:** recursion → memoization → tabulation.  
- **Resources:** [Neetcode DP Intro](https://www.youtube.com/watch?v=tyB0ztf0DNY)  

### Chapter 11: 2D DP
- **Concepts:** grids, subsequences, edit distance.  
- **Resources:** [Neetcode 2D DP](https://www.youtube.com/watch?v=h6FmiyYDjmk)  

### Chapter 12: Knapsack & Variants
- **Resources:** CLRS, Ch. 15 (*Dynamic Programming*)  

### Chapter 13: State Compression

### Chapter 14: Mixed Hard DP
- Practice: hard DP problems from LeetCode  

**Goal:** reduce recursion → memoization → tabulation; solve any DP-tagged LC problem.  

---

## Phase 5: Advanced Topics

- **Greedy:** 55, 45, 135  
- **Backtracking:** 78, 46, 51  
- **Segment Trees / Fenwick Trees:** 307, 315  
- **Strings (KMP, Trie):** 208, 211, 14, 28  
- **Bit Manipulation:** 136, 191, 338  

---

## Learn workflow
1. **Theory (1–2h):** watch videos, skim article/book chapter.  
2. **Notes (30m):** write definitions, time complexities, patterns.  
3. **Practice (5–10 problems):** solve progressively harder LC.  
4. **Reflection (15m):** tag problem → data structure/algorithm used.  

---

## Endgame
- Build a **mental classifier**: given a problem, know immediately → “Graph BFS,” “Heap top-k,” “DP table,” etc.  
- Write **concise, clean code**.  
- Compete at **LeetCode contest level**, but with deep DS/Algo knowledge.  




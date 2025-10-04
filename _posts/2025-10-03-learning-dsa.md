---
layout: post
title: Master DSA — Pattern-First Study Plan
date: 2025-10-04
description: A structured roadmap to master Data Structures and Algorithms using the Tech Interview Handbook and LeetCode Explore cards.
tags: [cs, dsa, algorithms, study-plan]
redirect_from:
  - /blog/2025/10/04/master-dsa
---

I’ve been programming for a while but always felt like my data structures and algorithms knowledge was surface-level.  
This is my attempt to *master DSA properly* — pattern-first, following the  
[Tech Interview Handbook Study Cheatsheet](https://www.techinterviewhandbook.org/algorithms/study-cheatsheet/)  
as the main structure, and pairing it with **LeetCode Explore Cards** for applied practice.

Each chapter follows the same format:
1. **Core concepts** – what to learn  
2. **Resources** – best explanations (videos, articles, or books)  
3. **Practice** – LeetCode Explore cards and curated problems  
4. **Goal** – what you should master before moving on  

---

## Phase 1: Core Data Structures

### Chapter 1: Arrays & Strings
- **Concepts:** indexing, iteration, two pointers, sliding window, prefix/suffix sums.  
- **Resources:**  
  - [Tech Interview Handbook – Array Techniques](https://www.techinterviewhandbook.org/algorithms/array/)  
  - [LeetCode Arrays 101](https://leetcode.com/explore/featured/card/fun-with-arrays/)  
  - [LeetCode Array & String Explore](https://leetcode.com/explore/learn/card/array-and-string/)  
  - CLRS, Ch. 2 (*Getting Started*)  
- **Foundations:**  
  - Array cost model (index O(1), mid insert/delete O(n)); Python list operations.  
  - Two pointers (forward compaction, backward write).  
  - Sliding window (fixed vs dynamic).  
  - Prefix sums for range calculations.  
- **After Learning:** review the LeetCode Array and String Explore cards.  
- **Goal:** confidently apply sliding window and two-pointer patterns.  

---

### Chapter 2: Hashing (Sets & Maps)
- **Concepts:** hashmaps, sets, frequency counting, collisions, and space-time tradeoffs.  
- **Resources:**  
  - [Tech Interview Handbook – Hashmaps](https://www.techinterviewhandbook.org/algorithms/array/)  
  - [NeetCode Hashing](https://www.youtube.com/watch?v=shs0KM3wKv8)  
  - [GeeksforGeeks – Hashing](https://www.geeksforgeeks.org/hashing-data-structure/)  
  - CLRS, Ch. 11 (*Hash Tables*)  
- **Foundations:**  
  - Hash functions, collisions, load factor.  
  - Average-case O(1) map/set operations.  
  - Frequency maps, grouping, and lookup caching.  
- **After Learning:** [LeetCode Hash Table Explore Card](https://leetcode.com/explore/learn/card/hash-table/)  
- **Goal:** solve grouping, frequency, and uniqueness problems using hashing.  

---

### Chapter 3: Linked Lists, Stacks, and Queues
- **Concepts:** pointers, reversal, cycle detection, monotonic stacks, BFS queues.  
- **Resources:**  
  - [Tech Interview Handbook – Linked Lists](https://www.techinterviewhandbook.org/algorithms/linked-list/)  
  - [NeetCode Linked List](https://www.youtube.com/watch?v=Hj_rA0dhr2I)  
  - [NeetCode Stack](https://www.youtube.com/watch?v=wjI1WNcIntg)  
  - [LeetCode Linked List Explore](https://leetcode.com/explore/learn/card/linked-list/)  
  - CLRS, Ch. 10 (*Elementary Data Structures*)  
- **Foundations:**  
  - Pointer mechanics; head/tail operations and complexities.  
  - Fast/slow pointer and cycle detection invariants.  
  - LIFO/FIFO models and amortized analysis.  
- **After Learning:** [LeetCode Queue & Stack Explore Card](https://leetcode.com/explore/learn/card/queue-stack/)  
- **Goal:** quickly implement linked list operations and identify monotonic stack problems.  

---

## Phase 2: Trees & Graphs

### Chapter 4: Binary Trees
- **Concepts:** recursion, traversals (pre/in/post-order), height, diameter, LCA.  
- **Resources:**  
  - [Tech Interview Handbook – Trees](https://www.techinterviewhandbook.org/algorithms/tree/)  
  - [NeetCode Trees](https://www.youtube.com/watch?v=fAAZixBzIAI)  
  - [LeetCode Tree Explore](https://leetcode.com/explore/learn/card/data-structure-tree/)  
  - CLRS, Ch. 12 (*Binary Search Trees*)  
- **Foundations:**  
  - Recursion and call stack intuition.  
  - Traversal orders and subtree invariants.  
  - Height/balance properties.  
- **After Learning:** [LeetCode Binary Tree Explore](https://leetcode.com/explore/learn/card/data-structure-tree/)  
- **Goal:** reduce problems into recursive traversals.  

---

### Chapter 5: BSTs & Heaps
- **Concepts:** BST invariants, heaps, and priority queues.  
- **Resources:**  
  - [Tech Interview Handbook – Heaps](https://www.techinterviewhandbook.org/algorithms/heap/)  
  - [NeetCode Heap](https://www.youtube.com/watch?v=HqPJF2L5h9U)  
  - CLRS, Ch. 6 (*Heapsort*)  
- **Foundations:**  
  - BST inorder properties and balance.  
  - Heap property, array representation, push/pop complexities.  
  - Priority queues for top-k, scheduling, and streaming.  
- **After Learning:** [LeetCode Heap Explore](https://leetcode.com/explore/learn/card/heap/)  
- **Goal:** recognize heap-based top-k and scheduling patterns.  

---

### Chapter 6: Graphs
- **Concepts:** adjacency list/matrix, BFS/DFS, connected components, topological sort.  
- **Resources:**  
  - [Tech Interview Handbook – Graphs](https://www.techinterviewhandbook.org/algorithms/graph/)  
  - [NeetCode Graphs](https://www.youtube.com/watch?v=tWVWeAqZ0WU)  
  - [LeetCode Graph Explore](https://leetcode.com/explore/learn/card/graph/)  
  - CLRS, Ch. 22–24 (*Graph Algorithms*)  
- **Foundations:**  
  - Graph representations and traversal invariants.  
  - Shortest path (unweighted) via BFS.  
  - Topological sort and DAG detection.  
- **After Learning:** [LeetCode Graph Explore Card](https://leetcode.com/explore/learn/card/graph/)  
- **Goal:** classify graph problems by traversal and connectivity type.  

---

## Phase 3: Searching & Sorting

### Chapter 7: Sorting
- **Concepts:** merge, quick, counting; sorting + scanning patterns.  
- **Resources:**  
  - [Tech Interview Handbook – Sorting](https://www.techinterviewhandbook.org/algorithms/sorting/)  
  - [NeetCode Sorting](https://www.youtube.com/watch?v=PgBzjlCcFvc)  
  - CLRS, Ch. 7 (*Quicksort*)  
- **Foundations:**  
  - Stable vs unstable sorts; partitioning and merging.  
  - Sorting as preprocessing (two-pointer / greedy).  
- **After Learning:** [LeetCode Sorting Explore](https://leetcode.com/explore/learn/card/sorting/)  
- **Goal:** identify sorting as a preprocessing or optimization step.  

---

### Chapter 8: Binary Search
- **Concepts:** binary search on index, range, or answer space.  
- **Resources:**  
  - [Tech Interview Handbook – Binary Search](https://www.techinterviewhandbook.org/algorithms/binary-search/)  
  - [NeetCode Binary Search](https://www.youtube.com/watch?v=U4yPae3GEO0)  
  - [LeetCode Binary Search Explore](https://leetcode.com/explore/learn/card/binary-search/)  
- **Foundations:**  
  - Invariant on bounds and termination.  
  - Lower/upper bound, monotonic predicate design.  
- **Goal:** master binary search variants.  

---

### Chapter 9: Greedy Algorithms
- **Concepts:** local vs global optimum, sorting-based selection.  
- **Resources:**  
  - [Tech Interview Handbook – Greedy](https://www.techinterviewhandbook.org/algorithms/greedy/)  
  - [NeetCode Greedy](https://www.youtube.com/watch?v=H9bfqozjoqs)  
  - CLRS, Ch. 16 (*Greedy Algorithms*)  
- **Foundations:**  
  - Proving correctness with exchange arguments.  
  - Interval scheduling, coin change, and meeting problems.  
- **Goal:** identify when a local choice leads to global optimality.  

---

## Phase 4: Dynamic Programming

### Chapter 10: 1D DP
- **Concepts:** recursion → memoization → tabulation.  
- **Resources:**  
  - [Tech Interview Handbook – Dynamic Programming](https://www.techinterviewhandbook.org/algorithms/dynamic-programming/)  
  - [NeetCode DP Intro](https://www.youtube.com/watch?v=tyB0ztf0DNY)  
- **After Learning:** [LeetCode DP Explore](https://leetcode.com/explore/learn/card/dynamic-programming/)  
- **Goal:** design states and transitions for linear recurrence problems.  

---

### Chapter 11: 2D DP and Knapsack
- **Concepts:** grids, subsequences, edit distance, 0/1 knapsack.  
- **Resources:**  
  - [NeetCode 2D DP](https://www.youtube.com/watch?v=h6FmiyYDjmk)  
  - CLRS, Ch. 15 (*Dynamic Programming*)  
- **Goal:** handle table-based DP and subproblem dependencies.  

---

### Chapter 12: State Compression and Hard DP
- **Concepts:** bitmasking, space optimization, combinatorial DP.  
- **Resources:**  
  - [CP-Algorithms Bitmask DP](https://cp-algorithms.com/dynamic_programming/state_compression.html)  
  - [LeetCode Hard DP Problems](https://leetcode.com/tag/dynamic-programming/)  
- **Goal:** optimize memory and state transitions for large-state DP.  

---

## Phase 5: Advanced Topics

### Chapter 13: Backtracking and Recursion
- **Concepts:** permutation generation, constraint satisfaction.  
- **Resources:**  
  - [NeetCode Backtracking](https://www.youtube.com/watch?v=s7AvT7cGdSo)  
  - [LeetCode Recursion II](https://leetcode.com/explore/learn/card/recursion-ii/)  
- **Goal:** model search problems as recursion trees.  

---

### Chapter 14: Tries and String Algorithms
- **Concepts:** prefix trees, substring search (KMP).  
- **Resources:**  
  - [Tech Interview Handbook – Trie](https://www.techinterviewhandbook.org/algorithms/trie/)  
  - [NeetCode Tries](https://www.youtube.com/watch?v=oObQBwA4MTI)  
  - [LeetCode String Explore](https://leetcode.com/explore/learn/card/string/)  
- **Goal:** implement trie-based lookups and prefix pattern matching.  

---

### Chapter 15: Segment Trees and Fenwick Trees
- **Concepts:** range queries and point updates.  
- **Resources:**  
  - [CP-Algorithms Segment Tree](https://cp-algorithms.com/data_structures/segment_tree.html)  
  - CLRS, Ch. 14 (*Augmenting Data Structures*)  
- **Goal:** design efficient range-query data structures.  

---

### Chapter 16: Bit Manipulation and Math
- **Concepts:** bitmask enumeration, parity, modular arithmetic.  
- **Resources:**  
  - [Tech Interview Handbook – Bit Manipulation](https://www.techinterviewhandbook.org/algorithms/bit-manipulation/)  
  - [LeetCode Bit Manipulation Explore](https://leetcode.com/explore/learn/card/bit-manipulation/)  
  - [CP-Algorithms Math Section](https://cp-algorithms.com/)  
- **Goal:** handle parity checks, subsets, and optimization via bit tricks.  

---

## Study Workflow
1. **Theory (1–2h):** read or watch resources for the chapter.  
2. **Notes (30m):** summarize patterns, invariants, and complexity.  
3. **Practice (5–10 problems):** focus on the pattern, not the solution.  
4. **Reflection (15m):** note pattern triggers and edge cases.  

---

## Next Step: Apply the Patterns

After finishing each chapter, use the **LeetCode Explore cards** to reinforce pattern recognition.

| Phase | Chapter | Explore Card |
|:------|:---------|:--------------|
| 1 | Arrays & Strings | [Arrays 101](https://leetcode.com/explore/featured/card/fun-with-arrays/) |
| 1 | Hashing | [Hash Table](https://leetcode.com/explore/learn/card/hash-table/) |
| 1 | Linked Lists | [Linked List](https://leetcode.com/explore/learn/card/linked-list/) |
| 2 | Trees | [Binary Tree](https://leetcode.com/explore/learn/card/data-structure-tree/) |
| 2 | BSTs & Heaps | [Heap](https://leetcode.com/explore/learn/card/heap/) |
| 2 | Graphs | [Graph](https://leetcode.com/explore/learn/card/graph/) |
| 3 | Sorting | [Sorting Algorithms](https://leetcode.com/explore/learn/card/sorting/) |
| 3 | Binary Search | [Binary Search](https://leetcode.com/explore/learn/card/binary-search/) |
| 4 | Dynamic Programming | [Dynamic Programming](https://leetcode.com/explore/learn/card/dynamic-programming/) |
| 5 | Backtracking | [Recursion II](https://leetcode.com/explore/learn/card/recursion-ii/) |
| 5 | Bit Manipulation | [Bit Manipulation](https://leetcode.com/explore/learn/card/bit-manipulation/) |

---

## Endgame
- Build a **pattern classifier**: instantly identify the right strategy for any problem.  
- Write **clean, efficient code** with strong reasoning.  
- Compete at **LeetCode contest level** with deep DS/Algo intuition.  

"Mastery isn’t memorizing solutions — it’s recognizing the pattern before you start."

## Resources
- [Big-O Cheat Sheet](https://www.bigocheatsheet.com/)
- [TechInterviewHandook](https://www.techinterviewhandbook.org/algorithms/study-cheatsheet/)

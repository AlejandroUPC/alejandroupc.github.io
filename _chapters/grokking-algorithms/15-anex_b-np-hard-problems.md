---
title: Anex B - NP-hard problems
order: 15
date: 2026-07-21
tags: [books, algorithms, grokking-algorithms]
---

## Chapter intro

Some problems are easy to solve with an algorithm that scales well, while other problems become impossible to compute exactly when the input gets bigger. This appendix is about those problems and what we can do when brute force is too slow.

- What NP, NP-hard, and NP-complete mean.

- Why some problems grow exponentially when we try every possibility.

- How approximation algorithms and heuristics can give us useful answers.

## Easy problems vs hard problems

When we talk about an algorithm being fast or slow, we usually care about how its running time grows with the input size. An `O(n)` or `O(n log n)` algorithm can usually handle much larger inputs than an `O(2^n)` algorithm.

For example, if we have a set of stations and want the smallest combination that covers every state, the most direct solution is to check every possible subset of stations. With `n` stations, there are `2^n` subsets:

```text
1 station:   2 subsets
2 stations:  4 subsets
3 stations:  8 subsets
...
n stations:  2^n subsets
```

This is fine for a very small number of stations, but the number of possibilities grows extremely quickly. Adding one more station doubles the search space.

## What does NP mean?

NP is a group of problems where, if somebody gives us a possible solution, we can check that solution quickly. The difficult part may be finding the solution in the first place.

For example, imagine somebody gives us a route through several cities and says it visits every city exactly once. We can check the route relatively quickly. Finding the shortest possible route may be much harder because there are many possible routes to compare.

NP does not mean that a problem is automatically impossible or that every NP problem takes exponential time. It is a way of describing the relationship between finding and checking solutions.

## NP-hard problems

An NP-hard problem is at least as hard as the hardest problems in NP. If we found a fast exact algorithm for one of these problems, it could be used to solve many other difficult problems quickly too.

Some examples include:

- The traveling salesperson problem: find the shortest route that visits every   city and returns to the start.

- The 0/1 knapsack problem: choose items with the highest value without exceeding   the capacity.

- The set-covering problem: cover every state with the smallest number of   stations.

- Scheduling problems where we need to assign many jobs while minimizing time or   cost.

NP-hard does not mean that we cannot solve an individual example. Small examples can often be solved with brute force or dynamic programming. The problem is that the exact approach may not scale as the input grows.

## NP-complete problems

NP-complete problems are both:

1. In NP: a proposed solution can be checked quickly. 2. NP-hard: finding a solution is at least as difficult as the hardest problems    in NP.

The difference between NP-hard and NP-complete is that NP-complete problems are also decision problems, where the answer is usually yes or no.

For example:

- Optimization version: what is the shortest route through all cities? - Decision version: is there a route through all cities with a total length below   100 kilometers?

The decision version can be NP-complete, while the optimization version is often described as NP-hard.

## Why brute force stops working

Brute force tries every possible solution and keeps the best one. This is useful because it is simple and guarantees the optimal answer, but the number of possible solutions can grow exponentially.

For example, with 30 items in a 0/1 knapsack, there are:

```text
2^30 = 1,073,741,824 combinations
```

That is already more than one billion combinations. With 100 items, the number is so large that checking every combination is not practical.

Dynamic programming can help when the problem has overlapping subproblems and optimal substructure. It does not solve every NP-hard problem efficiently, but it can turn some exponential-looking problems into more manageable algorithms when the numeric constraints are small enough.

## Approximation algorithms

When finding the exact optimum is too expensive, we can use an approximation algorithm. It gives us a solution that may not be perfect, but is usually much faster to compute.

We should ask two questions:

1. How fast can we find the answer? 2. How close is the answer to the optimal solution?

For the set-covering problem, the greedy algorithm repeatedly chooses the station that covers the most states that are still uncovered. It does not always find the smallest possible set of stations, but it gives a useful result much faster than checking every subset.

Some approximation algorithms also have a mathematical guarantee. For example, the standard greedy set-cover algorithm has a logarithmic approximation guarantee: its result is within roughly a factor of `O(log n)` of the optimal solution.

## Heuristics

A heuristic is a practical rule that tries to find a good answer without guaranteeing that the answer is optimal.

For example, in a route problem we could:

1. Start at the first city. 2. Go to the closest city that we have not visited. 3. Repeat until every city is visited.

This nearest-neighbor approach is fast and often gives a reasonable route, but a choice that looks best now can lead to a bad final route. It is a heuristic, not a proof that we found the shortest route.

Other heuristics can improve an initial answer by making small changes, such as swapping two cities in the route and keeping the change if it makes the route shorter.

## What should we do in practice?

When a problem looks NP-hard, we do not immediately give up. We can check a few options:

- Is the input small enough for brute force?

- Does dynamic programming work because the problem has useful overlapping states?

- Can we simplify the problem or accept an approximate answer?

- Is there a greedy strategy with a known guarantee?

- Would a heuristic give a good answer quickly enough?

- Can we use an existing solver designed for this type of problem?

The right answer depends on whether we need a guaranteed optimum, how large the input is, and how much error is acceptable.

## Closing up

- NP problems have solutions that can be checked quickly when a solution is given.

- NP-hard problems are at least as hard as the hardest problems in NP.

- NP-complete problems are both in NP and NP-hard.

- Brute force guarantees the best answer but can become exponential.

- Dynamic programming can help with some problems when overlapping subproblems   and manageable states exist.

- Approximation algorithms trade perfect answers for speed.

- Heuristics can find useful answers quickly but do not guarantee optimality.

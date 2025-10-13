---
layout: post
title: Big O Notation for Time Complexity
date: 2025-10-13
description: Understanding how algorithm runtime grows as input size increases.
tags: [cs, dsa, algorithms, big-o]
image: /assets/images/post_big_o_notation/big_o_comparison.png
image_alt: "Comparison chart of common Big O time complexities"
twitter:
  card: summary_large_image
redirect_from:
  - /blog/2025/10/13/master-dsa
---

Big O notation helps us understand, approximately, how expensive it is for an algorithm to run. I first encountered this in [CS50 Week 3](https://github.com/AlejandroUPC/cs-vault/blob/main/01_Core_Foundations/CS50/03_Week.md). This post combines my notes from several resources.

![Big O header]({{ '/assets/images/post_big_o_notation/big-o-blog-header.webp' | relative_url }}){: .center-img }

**Disclaimer:** I’m still learning — feel free to open a merge request if you spot a mistake.

## Introduction

A computer can only execute as many instructions as its clock allows — its **clock speed**, usually measured in GHz.  
For example, 3GHz ≈ 3 billion cycles per second.

The *same algorithm* can run fast on a modern CPU and slow on an old one. So instead of measuring absolute time, we measure **how many steps** it takes as a function of input size `n`.

Many other factors affect performance (compiler optimizations, caching, hardware parallelism), but Big O ignores these and focuses purely on how runtime grows with input size.

## Reading complexity from code

You’ll see in this post several claims that a function or algorithm has a certain time complexity.  
This small (and simple) introduction explains *how* those claims are made — in the form of: **when you see X, expect Y.**

1. Two sequential loops (not nested) → `O(n) + O(n)` simplifies to **O(n)** asymptotically, since constants are ignored.  
2. Two nested loops → the total work is the product of both loop sizes, `O(n) * O(m)` = **O(n·m)** (and **O(n²)** if `n == m`).  
3. A recursive function that halves its input each call → **O(log n)** (as in the phonebook example).  
4. An operation that halves its input but must be repeated `n` times → **O(n log n)**.  
5. Some Python built-ins, such as sorting (`list.sort()`), are not `O(1)` but **O(n log n)**.

A common rule of thumb: when you see a simple for-loop iterating over all elements, it’s usually **O(n)**.

For `while` loops, it helps to focus on *how the controlling variable changes each iteration*.  
For example, this loop runs in **O(log n)** because `k` halves every time:

```python
k = n
while k > 1:
    k //= 2
```

Each iteration reduces the problem size by a constant factor, leading to logarithmic growth.

## O(1) — Constant Time

Also known as constant time, when an algorithm’s runtime doesn’t depend on the input size. No matter how large the array or dataset, the operation always takes the same number of steps.

Common examples:

1. Accessing an array element by its index.  
2. Accessing a dictionary key in Python.

If you see no loops, recursion, or size-dependent iteration, it’s usually O(1).  
Be careful though — a function like `array.sort()` looks simple but runs in **O(n log n)** under the hood.

## O(n) — Linear Time

Runtime grows linearly with the input size.

```python
def find_in_array(arr: list[int], target: int) -> bool:
    for idx, item in enumerate(arr):
        if item == target:
            print(f"Found {target=} in {idx} steps.")
            return True
    return False
```

Even though finding the first element is fast, Big O considers the **worst case** — scanning the entire array → **O(n)**.

Imagine a doctor taking 30 minutes per patient — doubling the patients doubles the total time.

![O(1)]({{ '/assets/images/post_big_o_notation/big_o_O1.png' | relative_url }}){: .center-img }

## O(log n) — Logarithmic Time

A classic example is **binary search** — repeatedly splitting a sorted list in half until you find the target.

If a phonebook has 100 pages:

```
100 → 50 → 25 → 12 → 6 → 3 → 1
```

That’s about 7 steps ≈ log₂(100) ≈ 6.6.  
Each step halves the remaining search space → **O(log n)**.

This only works if the input is **sorted**.

![O(log n)]({{ '/assets/images/post_big_o_notation/big_o_Ologn.png' | relative_url }}){: .center-img }

## O(n log n) — Log-Linear Time

This appears in efficient sorting algorithms like **merge sort** or **quicksort**.

Example:

```python
def merge_sort(arr):
    if len(arr) <= 1:
        return arr
    mid = len(arr)//2
    left = merge_sort(arr[:mid])
    right = merge_sort(arr[mid:])
    return merge(left, right)

def merge(left, right):
    i = j = 0
    merged = []
    while i < len(left) and j < len(right):
        if left[i] <= right[j]:
            merged.append(left[i]); i += 1
        else:
            merged.append(right[j]); j += 1
    merged.extend(left[i:]); merged.extend(right[j:])
    return merged
```

Each merge handles `n` elements, and there are log₂(n) recursive levels → **O(n log n)**.

![O(n log n)]({{ '/assets/images/post_big_o_notation/big_o_Onlogn.png' | relative_url }}){: .center-img }

## O(n²) — Quadratic Time

When every element is compared or combined with every other element.

```python
def print_pairs(arr: list[int]) -> None:
    for i in arr:
        for j in arr:
            print(i, j)
```

Two nested loops = n × n iterations → **O(n²)**.  
Three nested loops → **O(n³)**.

![O(n²)]({{ '/assets/images/post_big_o_notation/big_o_On2.png' | relative_url }}){: .center-img }

## O(2ⁿ) — Exponential Time

When work **doubles** with each additional input — e.g., the naive recursive Fibonacci.

```python
def fib(n):
    if n <= 1:
        return n
    return fib(n - 1) + fib(n - 2)
```

Each call spawns two new calls until n ≤ 1 → about 2ⁿ total calls.

```
fib(5)
├── fib(4)
│   ├── fib(3)
│   └── fib(2)
└── fib(3)
```

A huge amount of repeated work! Memoization can reduce this to O(n).

![O(2ⁿ)]({{ '/assets/images/post_big_o_notation/big_o_O2n.png' | relative_url }}){: .center-img }

## O(n!) — Factorial Time

Factorial time happens when an algorithm generates **all possible permutations**, e.g., brute-forcing the traveling salesman problem.

For n = 10 → 10! = 3.6 million possibilities.  
For n = 20 → 2.4 quintillion!

![O(n!)]({{ '/assets/images/post_big_o_notation/big_o_On_fact.png' | relative_url }}){: .center-img }

## Conclusions

Big O helps compare algorithms abstractly — not by how fast they run, but by *how their runtime grows*.

Most real-world algorithms fall between **O(1)** and **O(n log n)**.  
Beyond **O(n²)**, performance becomes impractical for large inputs.

There are deeper topics worth exploring — **amortized analysis** (balancing expensive vs cheap operations) and **space complexity** (memory usage).

Here’s a simple comparison chart of time complexities:

![Comparison]({{ '/assets/images/post_big_o_notation/big_o_comparison.png' | relative_url }}){: .center-img }


## Questionnaire — Test Your Understanding

A few questions to check what was just explained, some of the q's are explained as exmaples so try to answer without loooking.

---

```python
for i in range(n):
    print(i)
for j in range(n):
    print(j)
```

**Tip:** Two separate loops, not nested.

<details>
Each loop runs `n` times → total `n + n = 2n` → **O(n)** (constants are ignored).
</details>

---

```python
for i in range(n):
    for j in range(n):
        print(i, j)
```

**Tip:** Inner loop runs fully each outer iteration.

<details>
`n * n = n²` → **O(n²)**, quadratic growth.
</details>


---

```python
k = n
while k > 0:
    print(k)
    k //= 2
```

**Tip:** The variable `k` halves every time.

<details>
The number of iterations is log₂(n) → **O(log n)**.
</details>

---


```python
k = n
while k > 0:
    for i in range(k):
        print(i)
    k //= 2
```

**Tip:** Inner loop shrinks each round.

<details>
Total work = n + n/2 + n/4 + … = 2n → **O(n)**.
</details>

---


```python
def foo(n):
    if n <= 1:
        return
    foo(n//2)
    foo(n//2)
```

**Tip:** Each call splits the input in half and recurses twice.

<details>
`T(n) = 2T(n/2) + O(1)` → **O(n)** (like traversing all nodes in a binary tree).
</details>


---


```python
def fib(n):
    if n <= 1:
        return n
    return fib(n - 1) + fib(n - 2)
```

**Tip:** Each call spawns two recursive calls.

<details>
Each level doubles the number of calls → **O(2ⁿ)**.
</details>


## Resources

- [Big O Notation - Full Course (YouTube)](https://www.youtube.com/watch?v=Mo4vesaut8g).
- [Big O Cheat Sheet](https://www.bigocheatsheet.com/).
- [Neetcode cheatsheet](https://neetcode.io/courses/lessons/big-o-notation).

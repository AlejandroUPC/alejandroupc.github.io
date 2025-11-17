---
title: Chapter 1 - Introduction to algorithms
order: 1
date: 2025-11-16
tags: [books, algorithms, grokking-algorithms]
---


## What is an algorithm?

Set of instructions to accomplish a task, one that will be studied is binary search.


## Searching

Given a sorted set of data starts looking in the middle and splitting the search space by the half every time. For example, if you want to look for someone in a phone book you could easily start in the middle, and check the letter from the alphabet you are at and 
split recursively.

For example, you are looking for the name of `Aaaron` and you happen to open in the middle, which is `M`, then we know that `A` is before `M` so we can split whatever is from `M` to `Z` (current page to the end) and open that half, and so recursively.

If we find the value we return `True`, else if its not there we return `None`.

Binary search is a very good algoirthm to search, for example, imagine we want to guess a number in a sorted array that goes from 1 to 100, we ask ourselves, which is the most 
number of steps we need to find a number (aka worse case scenario)?

If we go linear search is checking item by item and the item being in the last place, so if we wanted to guess the number in the position it would be a total of 100 steps.

Now if we used binary search, how many steps would we need until we find the number (or check all possible values?)

![binary search space]({{ '/assets/images/grokking_algos_chapter_1/binary_search_diagram.svg' | relative_url }}){: .center-img }

This is much faster than linear search, which we need to at least have `n` steps where `n` is the size of the space we are searching so we are talking about `100` steps vs `7`, but the numbers are much better with bigger values:

```plaintext
| n (elements) | Linear Search Steps (≈n) | Binary Search Steps (≈log₂ n) |
|--------------|---------------------------|-------------------------------|
| 10           | ~10                       | ~4                           |
| 100          | ~100                      | ~7                           |
| 1,000        | ~1,000                    | ~10                          |
| 1,000,000    | ~1,000,000                | ~20                          |
```

The pseudocode for binary search would look something like this:

```python
from typing import Any, Optional


def binary_search(arr: list[Any], target: Any) -> Optional[int]:
    """
    Perform binary search on a sorted array.

    Returns:
        The index of `target` if found, otherwise None.
    """
    lower_bound = 0
    upper_bound = len(arr) - 1
    iteration = 1

    print(f"Starting binary search on array: {arr}")

    while lower_bound <= upper_bound:
        mid = (lower_bound + upper_bound) // 2

        print(f"\n########## Iteration {iteration} ##########")
        print(f"Search window: arr[{lower_bound}:{upper_bound + 1}] = {arr[lower_bound:upper_bound + 1]}")
        print(f"mid = {mid}, arr[mid] = {arr[mid]}")

        if arr[mid] == target:
            print(f"Found target {target} at index {mid}")
            return mid

        elif target > arr[mid]:
            print(f"{target} > {arr[mid]} → move lower_bound to mid + 1")
            lower_bound = mid + 1

        else:  # target < arr[mid]
            print(f"{target} < {arr[mid]} → move upper_bound to mid - 1")
            upper_bound = mid - 1

        iteration += 1

    print(f"\nTarget {target} not found in array.")
    return None

```

And here is a visual representation to help you understand:


![binary search diagram]({{ '/assets/images/grokking_algos_chapter_1/binary_search_excali.svg' | relative_url }}){: .center-img }




## Big O Notation

We have alredy a thread on big O notation in [this post]({{ '/blog/2025/10/13/big-o-notation/' | relative_url }}), TLDR is that is the number of steps for an input of size `n` in the worse case scenario, check the blog.


## Closing up

1. Binary search is much faster than linear search, the size sample on each step of the algorithm is reduced considerably.
2. `O(log(n))` from binary search is much faster than `O(n)`.
3. The speed of an algorithm on big O notation is computed as the number of steps (not speed) for the worse case scenario.

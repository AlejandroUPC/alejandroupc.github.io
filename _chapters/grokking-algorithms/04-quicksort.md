---
title: Chapter 4 - Quicksort
order: 5
date: 2025-12-07
tags: [books, algorithms, grokking-algorithms]
---


## Chapter intro

Things start comming together, we get introduced to a new way of solving problems called divide and conquery, which uses recursive calls and take a glance at quicksort, a sorting algorithm that implements D&C.

## Divide and conquer

It can be initially slightly hard to grasp but you can think of a way of dividing problems into smaller parts, whose outputs of those smaller parts can be combined, there are mainly two steps as recursive problems:

1. Find out the base case, simplest case where the algorithm stops running.
2. Divide your problem until it leads to a base case.


So based on step 2 most of the time the D&C problems will be a function that calls itself into two(or more) smaller parts(divide) and is somehow able to combine the output of those two recursive calls.

Now imagine we have an array telling us the height of all of the buildings in a city and we want to find the tallest one using D&C.

The base case would be if there is a single item in the array, so some pseudo code:

```text
if only 1 building 'A' -> tallest building is 'A'
```

Then the reecursive case, while there is more than one item in the array would be:

```text
if buildings are 'A', 'B', 'C', 'D' -> Split in two groups, pick the max for each group and then the max of both, until a single item.
```

In code, it would look like:

```python
def quicksort_max(heights: list[int]) -> int:
    if len(heights) == 1:
        return heights[0]
    mid = len(heights) // 2
    left_half = quicksort_max(heights[:mid])
    right_half = quicksort_max(heights[mid:])
    return max(left_half, right_half)

```

So in the example above we break the example in smaller parts every iteration and keep calling it recursively until we reach the solution, or base case.


## Quicksort

One interesting sorting algorith, much faster than selection sort, that uses D&C.

Let's start small with the base cases, what is the simplest array to solve? Either an empty one or one with one element, we have literally nothing to do there.

```python
def quicksort(arr: list[int]):
    if len(arr) < 2:
        return arr
```

Now, for two elements, let's start drawing:


That was easy right? Now with three elements we introduce the element of the pivot, a value in the list used to split it in two sub arrays based if the value is higher or lower (picking this value right is important! talking about this later on); the process of splitting is called partitioning:

![quicksort 2]({{ '/assets/images/grokking_algos_chapter_4/qs_2_items.svg' | relative_url }}){: .center-img }


So we managed to sort the array, the steps, after setting the base case are clear then, we keep writting our algorithm:

```python
def quicksort(arr: list[int]):
    if len(arr) < 2:
        return arr
    pivot = arr[0] # pivot here is just the first item
    # improtant, WE MUST EXCLUDE PIVOT from the arrays we filter
    left_pivot = list(filter(lambda x: x < pivot, arr[1:]))
    right_pivot = list(filter(lambda x: x >= pivot, arr[1:]))
```

Now, in order to understand the recursive call we increase the size of the array in the example to three:

![quicksort 3]({{ '/assets/images/grokking_algos_chapter_4/qs_3_items.svg' | relative_url }}){: .center-img }


Finally we increase the size of the array to five to be more representative:

![quicksort 5]({{ '/assets/images/grokking_algos_chapter_4/qs_5_items.svg' | relative_url }}){: .center-img }

Basically what quicksort is doing is picking a pivot and splitting the array from a point, knowing that all the items from the left are smaller (but are not sorted yet!), so it's sorting around a pivot on every iteration until the there is nothing else to sort; the full implementation could be something like:

```python
def quicksort(arr: list):
    """O(n log n) # depends on pivot"""
    if len(arr) < 2:
        return arr
    pivot = arr[0]
    bigger_than_pivot = list(filter(lambda x: x > pivot, arr[1:]))
    smaller_than_pivot = list(filter(lambda x: x <= pivot, arr[1:]))
    print(
        f"Pivot is {pivot} bigger than {bigger_than_pivot} smaller than {smaller_than_pivot}."
    )
    return quicksort(smaller_than_pivot) + [pivot] + quicksort(bigger_than_pivot)

```

Those are the results of calling it with the prints and some array sample:

```python
if __name__ == "__main__":
    print(quicksort([1, 4, 5, 7, 9, 2]))

Pivot is 1 bigger than [4, 5, 7, 9, 2] smaller than [].
Pivot is 4 bigger than [5, 7, 9] smaller than [2].
Pivot is 5 bigger than [7, 9] smaller than [].
Pivot is 7 bigger than [9] smaller than [].
[1, 2, 4, 5, 7, 9]
```

When reviewing `O(n)` quicksort can be as bad as `O(n^2)` but as average it is `O(nlogn)`, which is not bad, but there is a better algorithm called merge sort which is always `O(nlogn)`, so why would use then quicksort?


## Merge sort vs quick sort

Most of the time we have said that when computing big O times constants do not make a difference and we remove the, specially if we are comparing different times (e.g `O(n)` vs `O(logn)`).

But in some cases constants make a difference, specially if looking at same runtimes, because based on how quicksort is implemented it can be `O(nlogn)`.Even if they are both the same time, quicksort is faster, and this is because it hits the average case more often than the worst case.

If you pick the worst case on quicksort, where the pivot is always the first element and you call an array that is already sorted, quicksort won't check if its already sorted and will still need `n`  steps to solve it, see for example:

![innefective_qs]({{ '/assets/images/grokking_algos_chapter_4/ineffective_qs.svg' | relative_url }}){: .center-img }


So above we just had a small array that was empty all the time on the left, and a big one on the right most of the time, does not look ideal does it? Now if we decided to pick the middle:


![efective_qs]({{ '/assets/images/grokking_algos_chapter_4/effective_qs.svg' | relative_url }}){: .center-img }

We hit now a much better number of calls `O(logn)`.

What is important here? In both implementations of the algorithm, on every step we need to go through all the items when we partition around the pivot, the differece here is then the number of calls in the stack (aka stack height), so we can say for the first case:

1. `O(n) * O(n) = steps * height_stack = O(n^2)` is the worst run time.
2. `O(n) * O(logn) = steps * height_stack = O(nlogn)` is the best case.



## Closing up

D&C is a nice way to tackle problems, similarly to recursion is a way to break problems into smaller problems and call it recursively.

When doing quicksort, pick a random element as pivot as the average time will be `O(nlogn)`.

It is important that when algorithm have same runtimes, one can be constantly faster than the other so its important to know which, that's why quicksort is faster than merge sort.

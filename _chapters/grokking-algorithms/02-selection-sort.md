---
title: Chapter 2 - Selection Sort
order: 2
date: 2025-11-17
tags: [books, algorithms, grokking-algorithms]
---


## How does memory work?

You could imagin the memory of your computer to work as a literal grid, where you have this two ideas:

1. You can access the cells by their address, e.g if excel-like grid like A:1 would access the first row and first column.
2. You can access only the value of those cells by using the address above.

Usually programs will be reading and writting to memory, and specially for the first sometimes its important to understand how the data lives in memory to make sure we effectively read it, or whats the best way to write it.



### Arrays and Linked Lists

**Arrays**

One of the first data structures and most simple to learn, all the values of an array live contiguously in memory, literally one after the other. So whenever you create an array in your code the program will reserve a set of slots which you expect you will be needing.
And here lies one of the big questions, how many slots would you reserve for your array? again this need of needing in advance is because it will live contiguous in memory, so you can either:

1. Fall short: You wont be able to add more items until you find a set of contiguous memory slots where the array + new items fit, and moving it around often can be expensive.
2. Reserve too much: You will be oversizing some of the resources and other memory needed by your program might not be allocable.

**Note** in Python arrays are barely used and most of times lists are used, which is kinda of an array that support for multiple data types and reseres some space in the beggining and end but this is for another post, here is what an array could look in memory:

![example memory cells array]({{ '/assets/images/grokking_algos_chapter_2/memory_cells_example_00.svg' | relative_url }}){: .center-img }

So given the image above, could you create a new array of size 3? Yes, but could you append it two more items? No, beceause the most amount of available memory that is contiguous is 4.

One of the good things of array is the random acces, where we can compute any position's value by having the initial address (think as of position 0) + the number in the array we want to access.

**Linked Lists**

Initially linked list solve one of the array's main issue, the constraint of contiguous cell. The items can be anywhere in the memory, we just need to add a new concept of a pointer, that tells us where our next element in the list is, you could imagine where each cell instead of a value has now a value and the cell (counting as a 0 index array) of where the next item is:

![example ll]({{ '/assets/images/grokking_algos_chapter_2/chapter_2_ll_simple.svg' | relative_url }}){: .center-img }

So now you have a three item linked list where the first item has a value of `1` and is telling you its next item is on the cell `3`, and then the next item will be on the cell `5` and there is no longer a next item pointer on that cell because its the end of our list, where the values do not need to be in contiguous memory positions!

Now, imagine that you have a very big list of `100` items, and you want to get the value of the last item? You'd need to go from the start and keep asking each node until you raech one that has no "next node" pointer, so it looks like linked lists might not be as good to just jump around vs arrays, that allow for random access.

Now do the exercise, if we wanted to add an item at the middle of any of those data structures, which one would be better?

**DISCLAIMER** For the LL example the fact that the picture does not represent their position in memory (they could be anywhere) also for this example it might seem its almost the same but at scale arrays would be much more work.

For an array we'd need to find the middle and then have to swap all of the elements one by one:

![insert mid arr]({{ '/assets/images/grokking_algos_chapter_2/add_mid_array.svg' | relative_url }}){: .center-img }

While for the linked list, simply finding the middle, adding a new node and have the previous one point to the new would be enough:

![insert mid ll]({{ '/assets/images/grokking_algos_chapter_2/add_mid_ll.svg' | relative_url }}){: .center-img }


For deletions in the middle specially, not start/end, we run into a similar problem where arrays have to shuffle everything to have it contiguous, while in linked lists its justrearranging some pointers:


```markdown
| OP              | Array           | LL              |
| --------------- | --------------- | --------------- |
| Reading         |       O(1)      |       O(n)      |
| Insertion       |       O(n)      |       O(1)      |
| Deletion        |       O(n)      |       O(1)      |

```


#### So which is used more?

Arrays (or some of its variations) are used more, given that they are better at reading by providing random access.
There are two types of accesses:

1. Random access: You can jump directly to the `nth` element.
2. Sequential access: You have to read the elements one by one, like linked lists.

An additional adventage of arrays is caching, you can just read an entire block of cells (contiguous, the array), instead of having to sequentially read it. So when you cache something and you iterate over an array, even the sequential access is faster.

We also argued that arrays were less memory efficient because of how they need to take memory in advance to avoid relocations, the thing is if you take into consideration also that linked lists need additional memory per item (value + address), the differences might be not so big regarding the memory allocation.


## Selection Sort

Quite simple sorting algorithm, we are going to iterate over a list, find the max/min value, remove it from current list and add it in to the new one, and keep appending over until the initial list, then we will have an descending/ascending otrdered list.

Now in terms of big O notation we know that we will be iterating a list of `O(n)` items, but its not enough with a single iteration so we will have to iterate it also `n` items (**NOTE** we ignore that on every iteration the array will decrease as `n-1`, explanation later on), so we have `n * O(n) = O(n*n)=O(n^2)`.

This is then not a really fast algorithm, although there are worse!

**NOTE** We are not considering how the array reduces over time because the serie `n-1` ends up `1/2 * n`, but in big O notation we remove constants so it ends up being `n * O(n * 1/2) = O(n * 1/2 * n) = O(n^2)`.

The pseudo code woudl be something like:

```python
def _get_min_idx(arr) -> int:
    min_idx = 0
    for idx, item in enumerate(arr):
        if item < arr[min_idx]:
            min_idx = idx
    return min_idx


def selection_sort(arr: list) -> list:
    """O(n^2)"""
    new_arr: list[int | float] = []
    _arr = list(arr)
    for item in arr:
        min_idx: int = _get_min_idx(_arr)
        new_arr.append(_arr.pop(min_idx))
    return new_arr

```

## Closing up

1. Basic understanding of how a computer memory works, and when storing somehow related elements it make sense to use an array or LL.
2. Pros/cons arrays and LL; arrays allow fast reads LL allow fast inserts and deletions.
3. A basic implementation of selection sort.


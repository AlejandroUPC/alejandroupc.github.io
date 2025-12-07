---
title: Chapter 3 - Recursion
order: 3
date: 2025-11-27
tags: [books, algorithms, grokking-algorithms]
---


## Chapter intro

Recrusion is a way to approach problems that involves a function calling itself. Although it can be highly effective it's been always a controversial topic, some people hate it or others love it (even some companies like NASA apparently [do not allow recursion](https://craftofcoding.wordpress.com/2021/03/08/why-does-nasa-not-allow-recursion/)).

## Recursion

The book explains an example of boxes inside boxes (nested boxes, e.g think of recursion when thinking of nested structures), imagine we have a big box and inside  this box there are more boxes, with more boxes inside and so on... You need to open boxes until you find a key, so you could go like:

1. Make a pile of all the boxes
2. Open first box
3. If there is a key you are done.
4. If there is a box, put it on the pile so you can look into it later.
5. Grab next box on the pile.

You would do this until you find the key, or you run out of boxes; so there are two key events here:
    1. Find the key: The base case where the algorithm ends.
    2. You are out of boxes: There was no key at all (aka base case)

Those two events have to stop the algorithm from running infinitely.

When decomposing recursive functions we can identify two parts:
    1. Base case: As mentioned before, when the function stops running.
    2. Recursive case: When the function has to stop calling itself.

A very way to ilustrate this:

```python
def countdown_to_zero(start:int):
    if start <= 0:
        raise ValueError(f"To count from 0 the value has to be greater than 0.")
    print(f"We are at {start}.")
    if start <= = 0:
        return # we stop, base case because we want to count until 0.
    else:
       return countdown_to_sero(start-1)
```

## The stack

So we might have those functions that keep calling themselves, e.g for the example `countdown_to_zero` you could imagine something like this:

![call function stack]({{ '/assets/images/grokking_algos_chapter_3/call_function_stack.svg' | relative_url }}){: .center-img }

Stack is another data structure where you can add items (push) and remove them **only from the top** (pop), so kinda LIFO (Last In First Out).
Your computer uses a call stack for functions, and although its not exclusively a recursion concept its key to understand recursion.

One of the most common examples besides Fibonacci series is factorial, which can easily be desribed as recursive function where you can keep multiplying numbers until reaching one, e.g `5! = 5 * 4 * 3 * 2 * 1`, so it looks like it could be written as:

```python
def factorial(i:int):
    if i < 0:
        raise ValueError(f"Only accepting values bigger than 0, got {i=}.")
    if i == 1:
        return 1
    else:
        return i * factorial(i -1)
```

Almost identical to the countdown example we keep adding stuff to the call stack until we hit a return, which only should happen when the base case is reached, so when `i == 1`.

What happens then? The stack starts to hit pop (again remove item from top and act), this picture might help better understand:

![call function stack decomposed]({{ '/assets/images/grokking_algos_chapter_3/call_function_stack_decomp.svg' | relative_url }}){: .center-img }


We have to be careful, as this stack also takes memory and having a lot of recursion (some programming languages have its own way to control it, such as Python [Reecursion Error](https://realpython.com/ref/builtin-exceptions/recursionerror/)).

You can always work around recursion by:

1. Using a loop.
2. Using tail recursion, when supported.

## Closing up

A recursion is when a function calls itself, and is ultimately split into base and recursive caes.

All those calls go into a LIFO stack, where things are pushed and poped + evaluated, we have to be careful with the stack as it takes memory.

My personal opinion is that even if very elegant, it can get complicated really quick so it should only be used in clear contexts where you are pretty sure the stack will not get out of control.


---
title: Chapter 11 - Dynamic Programming
order: 11
date: 2026-07-19
tags: [books, algorithms, grokking-algorithms]
---

## Chapter intro


- Thecnique consisting of breaking a big problem into subproblems that we solve first.


## Revisintg the knapsack problem

Remember the greedy approach was not optimal for the knapsack problem? Looking at the items individually and picking the most expensive was not the best approach, as having a very expensive item could give a lower value than the combination of two with lower value but that fit less space.

Imagine the capacity now is 4 LB and the items as follow:

```markdown
| item | cost | weight |
|------|------|--------|
|stereo| 3000 |   4    | 
|laptop| 2000 |   3    |
|guitar| 1500 |   1    |
```

### Simples solution

One of the simplest solution would be to check all possible combinations, without repetition and including empty, and then discard the ones that don't fit, so that would be `O(2^n)`:

```markdown
|    combination       | value | weight |
|----------------------|-------|--------|
|        nothing       |   0   |    0   |
|        guitar        |  1500 |    1   |
|        stereo        |  3000 |    4   |
|        laptop        |  2000 |    3   |
|    guiter + stereo   |  4500 |    5   |
|    guitar + laptop   |  3500 |    4   |
|    stereo + laptop   |  4500 |    7   |
| guitar+stereo+laptop |  6500 |    8   |
```


So what's the biggest value we can keep at or below 4 LB? Seems to be guitar + laptop for a total of 3500.


So instead of using a greedy algorithm to compute the approximate solution, what can we do to get the actual optimal?


## Dynamic programming

We are going to start by solving smaller versions of the problems, let's break up the current knapsack into smaller knapscack and lal of the potential items so we end up with such a matrix/table:


```markdown
| _  | 1 | 2 | 3 | 4 |
|----|---|---|---|---|
| guitar| | | | |
| stereo| | | | |
| laptop| | | | |
```

For the first row we try to fit a guitar on each of the knapsack and add the value, because its just 1lb, we can fit in em all

```markdown
| _  | 1 | 2 | 3 | 4 |
|----|---|---|---|---|
| guitar| 1500 | 1500 | 1500 |  1500 |
| stereo| | | | |
| laptop| | | | |
```

Now for the stereo it stargs to get interesting:


```markdown
| _  | 1 | 2 | 3 | 4 |
|----|---|---|---|---|
| guitar| 1500 g| 1500 g| 1500 g| 1500 g|
| stereo| 1500 g| 1500 g| 1500 g| 3000 sup|
| laptop| | | | |
```

The row is looking exactly the same until we can fit the stereo (removing the guitar!) for the 4lb capacity knapsack, so it's our current bet, we move to the laptop:

```markdown
| _  | 1 | 2 | 3 | 4 |
|----|---|---|---|---|
| guitar| 1500 g| 1500 g| 1500 g| 1500 g|
| stereo| 1500 g| 1500 g| 1500 g| 3000 sup|
| laptop| 1500 g| 1500 g| 2000 l | 3500 g + l|
```

Similarly, for all the columns below 3 the guitar is the best option, but on 3 we can see that we can finally fit a laptopt for 2000! That's an improvement, but what happens for the 4 LB knapsack is that for the first time we can combine two items in one, and its even better than our previous max!

So as expected, the col 4 (the knapsack) contains the best results (meaning that it can fit more items), and the best combination happens to be two items that beat the stereo itself!

Actually, when you think of a matrix you can think of a formula, and this algorithm could be thought as, for every cell in row `i` and column `j`, pick the max between:

1. previous highest value in same col `dp[i-1][j]`
2. currenet item + whatever you can fit in the remaining space: `value[i] + dp[i-1][j-weight[i]]`.

If the current item does not fit, we just use the value from the row above.


## Knapsack problem FAQ

It's very interesting how you can use the table/matrix to compute fast, imagine the last case, before we come up to the max:

```markdown
| _  | 1 | 2 | 3 | 4 |
|----|---|---|---|---|
| guitar| 1500 g| 1500 g| 1500 g| 1500 g|
| stereo| 1500 g| 1500 g| 1500 g| 3000 sup|
| laptop| 1500 g| 1500 g| 2000 l | 3500 g + l|
```


For the laptop, with a cost of 2000 we are met two choices (1 and listed before), we can either pick the previous max (2000) or check what is the best knapsack for the current capacity - taken already, which is 4 - 1 = 3; so if we go to col 1 (which we computed already), whats the best we can do? Yeah 1500 so its a total of 3500.

Also its clear that as we go down, all the values will be increasing , we store the best value possible.

There's a similar example, optimizing a travel itirenary.

Some other stuff:

- You can't take portions of items (e.g half gutiar or if its a weight less tham the item that the table represents), either you take it all or not.

- Order of the rows should not matter 

- Dynamic programming works best when the problem can be split into smaller problems that overlap, and the best solution to a big problem can be built from the best solutions to its smaller problems. The items in this example do not depend on each other, but that is not a requirement for every dynamic programming problem.

- For this example, best solution does not necessarely mean the knapsack has to be full (e.g small item with lots of value).

Given a set of placs you want to visit, you have a capacity (time) and rating. You only have two days (limit < sum of capacity), so you want to optimize for having a high rating minimising (not hitting the limit) time.

If we have this table then:

```markdown
| place | time | rating |
|-|-|-|
|Westminister Abbey | 0.5 | 7 |
| Globe Theater | 0.5 | 6 |
|National Gallery | 1 | 9 |
|British Museum | 2 | 9|
|St. Paul's Cathedral|0.5 | 8|
```

So we can build the following table/matrix:



```markdown
|  - | 0.5 | 1 | 1.5 | 2 |
|-|-|-|-|-|
|Westminister Abbey | | | |  |
| Globe Theater |  | | | |
|National Gallery | | | | |
|British Museum |  | | | |
|St. Paul's Cathedral| | | | |
```


Note that the table is built from min(value) to max(value) with min(value) increments, let's start then:


```markdown
|  - | 0.5 | 1 | 1.5 | 2 |
|-|-|-|-|-|
|Westminister Abbey |  7  W | 7 W | 7 W | 7 W |
| Globe Theater | 7 W | 13 W + G| 13 W +G | 13 W + G|
|National Gallery | 7 W | 13 W + G | 16 N + W | 22 W + N + G|
|British Museum | 7 W |  13 W + G | 16 W + G | 22 W +N + G |
|St. Paul's Cathedral| 8 S | 15 W + S| 21 W + G + S|  24 W + N + S|
```

## Longest Common Substring

It's hard to understand when to use dynamic programming, some general tips:

- Useful to use a grid to approach the problem.

- You try to optimize the values inside the cells of the grid (e.g knapsack was value)

- Each cell can be thiought as a single problem you can sub divide, defined by the axis (e.g weight in the knapsack).

One clear case is finding the number common substrings into a string, this is a clear example of someone making a typo.

Imagine someone writes to you (without context) a single word, `HISH`, did you mean `FISH` or `VISTA`? Seems clear to be `FISH` but they both share a few words in common:

```markdown
H I S H

F I S H 
  1+1+1 = 3

V I S T A
  1+1     = 2
```

But let's approach this as a dp problem. We are trying to maximize the longest substring that two words share, but we need to answer some questions:

- What are the values of the cells? Instead of the knapsack problem, where it was the aggregated of any item, here there is an additional condition that the words have to be together, its a substring, so the value should be the amount of words in common we have carried until that point.

- How do you divide this problem into subproblems? Well instead of comparing the entire word, we can keep comparing from `1 to len(n)` (both strings are same length) all the words in common for longest substrings, so the axes will most be likely the wrods.

So we have the following grid:


```markdown
 H I S H
F
I
S
H
```

Again the "hard" or different part here, is that we have to keep the state to see if the previous word (as we check longer strings) was also the same, so we need to keep some kind of state (e.kg in knapsack this constraint was simpler, was just a look up to get the value/weight of each item).

So the formula could be something like:

```python
if row[i] == row[j]:
    row[i][j] = row[i-1][j-1]+1
else:
    row[i][j] = 0
```


So if we compare both

```markdown
  H I S H  |  H I S H
F 0 0 0 0  |V 0 0 0 0
I 0 1 0 0  |I 0 1 0 0
S 0 0 2 0  |S 0 0 2 0
H 1 0 0 3  |T 0 0 0 0
           |A 0 0 0 0
```


Looks like we got a `3` in `FISH`, so that is indeed the most similar word, because it has three letters creating a word(substring), vs two.


Note that the last cell (bottom right), must not be the solution, e.g imagine we compare `FISH` to `HISHO` , bottom right will be 0 but we still have a 3.

### Longest common subsequence

Now imagine the word typed is `FOSH`, we are not sure, is it either `FORT` or `FISH`?

Let's write the tables again:

```markdown
  F O S H    |  F O S H
F 1 0 0 0    |F 1 0 0 0
O 0 2 0 0    |I 0 0 0 0
R 0 0 0 0    |S 0 0 1 0
T 0 0 0 0    |H 0 0 0 2

```

So its a tie! But is it really? But agian `FOSH` has three common letters vs `FORT`, which has just two, the issue that the longest substring is both of length 2.

The deal here is we need to compare the longest common subsequence, which is the latters in the sequence that two words have in common, so our previous approach did not work.

The important distinction is that a substring has to be contiguous, while a subsequence only needs to preserve the order and can skip letters. For example, `FSH` can be a subsequence of `FISH`, but it is not a substring.

Here, it seems, that whenever we find a common word we need to keep expanding it through the matrix so its not lost when a word does not match, how to do so? Well maybe we can:

```python
if row[i] == row[j]:
    row[i][j] = row[i-1][j-1]+1
else:
    row[i][j] = max([row[i-1][j], row[i][j-1]) # here we check left and top right neighbour, so if its 1 we spread the value
```

Some of the cases of DP are:

- Biologist to find similarties in DNA strands.

- Commands like diff use dp to show diff between two files.

- Levensthein's distance does that to check for similarity on strings, from spell checking to check if user is uploading copyright data.


## Closing up

- DP is a way to solve problems when you are trying to optimize given a constraint, and the problem can be broken into discrete problems.

- Every DP involves thinking with terms of a grid, where the values is what we try to optimize and the edges the constraints.

- Each cell can be thought of a sub problem.

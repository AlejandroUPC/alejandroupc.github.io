---
title: Chapter 10 - Greedy Algorithms
order: 10
date: 2026-07-18
tags: [books, algorithms, grokking-algorithms]
---

## Chapter intro

- Greedy strategy, simple and sometimes the most effective (NP-hard problems).

- Approximation algos, again for NP-hard problems.


## The classroom scheduling problem


Changing the example slightly, instead of a classroom imagine you are renting a garage to repair bikes. Each bike takes about one hour to repair, and your agenda contains overlapping appointments. The appointments are listed in order of starting time:


```markdown
| bike | start | end |
|------|-------|-----|
|  b1  |  9:00 |10:00|
|  b2  |  9:30 |10:30|
|  b3  | 10:00 |11:00|
|  b4  | 10:30 |11:30| 
|  b5  | 11:00 |12:00|
```
Some appointments overlap and will have to be cancelled. How can you schedule the maximum number of repairs?

Well you can think of an easy algorithm:

1. Pick the repair that ends the soonest.
2. Pick the next compatible repair: one that starts at or after the previous repair ends, and that ends the soonest.

Here is code:


```python
import math


def find_closest_repair(
    schedule: dict[str, tuple[float, float]],
    processed_repairs: set[str],
) -> str | None:
    next_repair_end: float = math.inf
    next_repair: str | None = None

    for repair, time_window in schedule.items():
        if repair in processed_repairs:
            continue

        _, end = time_window

        if end < next_repair_end:
            next_repair_end = end
            next_repair = repair

    return next_repair


def is_overlapping(
    prev_repair: tuple[float, float],
    next_repair: tuple[float, float],
) -> bool:
    _, end_prev = prev_repair
    start_next, _ = next_repair

    return start_next < end_prev


def main(schedule: dict[str, tuple[float, float]]) -> None:
    processed_repairs: set[str] = set()
    scheduled_repairs: list[str] = []

    current_repair = find_closest_repair(
        schedule=schedule,
        processed_repairs=processed_repairs,
    )

    while current_repair is not None:
        processed_repairs.add(current_repair)

        if not scheduled_repairs:
            scheduled_repairs.append(current_repair)
        else:
            previous_repair = scheduled_repairs[-1] # if not scheduled the prev is the last we added

            if not is_overlapping(
                prev_repair=schedule[previous_repair],
                next_repair=schedule[current_repair],
            ):
                scheduled_repairs.append(current_repair)

        current_repair = find_closest_repair(
            schedule=schedule,
            processed_repairs=processed_repairs,
        )

    print(scheduled_repairs)


if __name__ == "__main__":
    schedule = {
        "b1": (9, 10),
        "b2": (9.5, 10.5),
        "b3": (10, 11),
        "b4": (10.5, 11.5),
        "b5": (11, 12),
    }
    main(schedule=schedule)
```


This is the interval scheduling problem. Its greedy rule is to always choose the compatible repair with the earliest finishing time. That local choice is guaranteed to produce a schedule with the maximum possible number of repairs.

## The knapsack problem

Here, using the same example from the book, we have a container with a capacity of 35 pounds. We want to maximize the total value without exceeding the weight limit.

There are two different versions of the knapsack problem:

- In the 0/1 version, each item must be taken whole or left behind. A greedy strategy is not guaranteed to find the best solution; dynamic programming is a common approach instead.
- In the fractional version, items can be divided. Greedy works here: repeatedly take as much as possible of the item with the highest value-to-weight ratio.

Imagine you had the following items to steal from:


```markdown
| item | cost | weight |
|------|------|--------|
|stereo| 3000 |   30   |
|laptop| 2000 |   20   |
|guitar| 1500 |   15   |
```

For the 0/1 version, a tempting greedy strategy is to pick the most valuable item that fits:

1. Stereo, 3000$, 30 lbs.

We'd have 5 pounds left and no item below that, so we'd be done.

But, what if we picked 20+15 pounds, aka 35 and racked 3500 $ picking the laptop and guitar?
 
This greedy strategy chooses the stereo for 3000 dollars, but choosing the laptop and guitar gives 3500 dollars. It is therefore not optimal for 0/1 knapsack. This illustrates why we cannot assume that a locally best choice will produce the globally best result.


## The set-covering problem

Now a compute-hard problem is presented, where we imagine we ar edoing a radio show and we want to maximize the states we reach out, but we are limited by the stations. We are given a set of stations and the states we reach:

![set covering example]({{ '/assets/images/grokking_algos_chapter_10/set-ex.svg' | relative_url }}){: .center-img }

As you can see there are some clear insights, like why pick radio station 4 when 1 covers the same states + ID, but focusing in the problem what we ideally want to do (in a kind of brutte force manner) is to maximize the amount of states while keeping (imagine they have a cost) the number of statios.

In order to do so, the easiest way is to list every possible subset of stations, this is called a power set and it has a total of `2^n` supersets. This means that something as little as 5 stations takes (assuming you can calculate 10 subsets per second), that is `2^5/10=32/10=3.2 seconds`, but if you go to 100, `2^100/10)=4x10^21` years.

### Approximation algorithms

Here is a greedy approximation for the set-covering problem:

1. Pick the station that covers the most states that have not been covered yet. Only consider a station if it adds at least one new state.
2. Repeat until all states are covered or no station can cover any remaining state.


This is an approximation algorithm, which are a good consideration based on:

- How fast they are
- How close to the optimal solution they land

For set cover specifically, the standard greedy algorithm has a logarithmic approximation guarantee: its solution is within roughly a factor of `O(log n)` of the optimal number of stations.

In this case, the greedy version runs on `O(n^2)`, which still could be considered expensive but much cheaper than `O(2^n)`

Here is some self written code on how to approach this:


```python
def main() -> None:
    states = {"WA", "MT", "ID", "OR", "CA", "NV", "UT", "AZ"}
    stations = {
        1: {"ID", "NV", "UT"},
        2: {"WA", "MT", "ID"},
        3: {"OR", "CA", "NV"},
        4: {"NV", "UT"},
        5: {"CA", "AZ"},
    }
    covered_states = set()
    stations_picked = set()

    while covered_states != states:
        best_station = None
        best_coverage = set()
        for station_name, statione_coverage in stations.items():
            new_coverage = statione_coverage - covered_states
            if len(new_coverage) > len(best_coverage):
                best_station = station_name
                best_coverage = new_coverage

        if best_station is None:
            print("Could not map all states.")
            return
        stations_picked.add(best_station)
        covered_states |= best_coverage
        print(f"Added {best_station} with states = {best_coverage}.")
    print(f"Used stations {stations_picked}.")


if __name__ == "__main__":
    main()

```

Note how we set a goal, to map all staets in the while clause and for each while iteration we re-run all the states. We are OK here iteration over all stations all the time (we dont need a visited object, as repeated sets will add nothing new to `new_coverage`) because the sample is small.

This code prints:


```makrkdown
Added 1 with states = {'NV', 'UT', 'ID'}.
Added 2 with states = {'MT', 'WA'}.
Added 3 with states = {'OR', 'CA'}.
Added 5 with states = {'AZ'}.
Used stations {1, 2, 3, 5}.
```


If we added an ew state, that is not covered into for any station so it becomes `states = {"WA", "MT", "ID", "OR", "CA", "NV", "UT", "AZ","FOO"}`, we get:

```markdown
Added 1 with states = {'ID', 'NV', 'UT'}.
Added 2 with states = {'MT', 'WA'}.
Added 3 with states = {'OR', 'CA'}.
Added 5 with states = {'AZ'}.
Could not map all states.
```

We could actually print unmapped states too.



## Closing up


- Greedy algorithms optimize locally and hope to end up with a global optimum.

- Approximation algorithms are often useful for NP-hard optimization problems, where finding the exact optimum may be too expensive.

- Greedy are easy to write and fast to run, good approximators.

- Greedy algorithms answer "Can I just pick what seems the best choice right now and ignore the future?"

They kind of follow a template:

```python
init_state

while problem not solved:
    best_choice = None
    for candidate in candidates:
        best_choice = find_best_choice(candidate)
        if candidate is better:
            best_choice = candidate

    commit(best_choice)
    update_state(best_choice)
```


- Usually approximation algorithms are `O(n^2)` because we iterate over space (ideally `n` times), rather than doing the combinatory for all space.

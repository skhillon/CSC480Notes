# CSC 480 - Lecture 3
Friday, September 28, 2018

*Note: Casey does not support exiting early from a loop. No `break`, `continue`, or `return` within a loop. To work around, just use a boolean flag.*

-----

## Notes
### Admissable Heuristics
- A heuristic `h(n)` is admissible if for every node `n`: `h(n) <= h*(n)`, where `h*(n)` is the true cost to reach goal state from `n`.
- An admissible heuristic never overestimates the cost to reach goal.
- Is always "optimistic", like the straight line distance to calculate driving distance.

### Dominance
- As stated before, you can often find admissible heuristics by relaxing certain constraints (`h1` = let tiles slide over each other to calculate Manhattan distance, or `h2` = number of tiles displaced.)
- If one is clearly more accurate than the other, and both heuristics are admissible, then you want to pick the most accurate one every time.
- The more accurate heuristic is said to **dominate**.

### Combining Heuristics
- Usually heuristics are not *always* better than others, so you can take the `max` of all available heuristics and combine them.

----- 
## Whiteboard Stuff / Project Discussion
- In shuffle, you're trying to create the most complex puzzle as possible. To do that, you maximize `h(n) = ManhattanDistance + LinearConflict` which is meant to approximate `NumMovesToSolvePuzzle`.
- If you get to a point where, no matter what, every node on the `frontier` has h(n) <= current, then you've probably found the max complexity. That's not entirely true--you may have an even more complex configuration later--but it's a good approximation.
- At the beginning, how do you create a shuffled puzzle that is solvable? Just shuffle them and continue checking `is_solvable`. Then after that, keep trying to maximize `h(n)`.
- You could also just keep generating random numbers up to `x` random configurations.
- `is_solvable()` should almost definitly implement mergesort because you can easily count the number of inversions in the list with `merge()`.
    -   Recall `mergesort(list)`:
        - Base case
        - left = MS(left half)
        - right = MS(right half)
        - Merge(left, right)
            - Whenever left < right, we append that onto the new list, otherwise we append right.
            - **Whenever right < left, you have an inversion. So, just keep a counter and increment that to find `NumInversions`.**
            - Don't count inversions involving 0. You can just use the index of 0 as its NumInversions because 0 would be greater than every element on its left, then subtract that from `NumInversions`.

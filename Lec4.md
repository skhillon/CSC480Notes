# CSC 480 - Lecture 4
Monday, October 1, 2018

- The Manhattan distance of a new state on the frontier will differ only by 1, so you don't have to recompute every time.

## A* Memory Optimizations
You don't need to put these in the project, but the things on the slide are good to know.

## Local Search Algorithms
- Focused on where we are at some point in time.
- Once we get a value that we think is good, we don't know if it's necessarily the best one.
- We don't keep our path in memory, we only choose the best successor in the current state (forget where we came from). It *can* be like Greedy based on how you choose a successor. There are no frontier and explored sets. How do you know you won't go in circles if there's no explored set?
- Put mechanism to avoid creating loops. So you might want to keep parent in memory.

### State Space Landscape
- You have some sort of evaluation function. Using this function, we can draw out a state space that shows us the local "best" and "worst." You don't necessarily know if that is the best one, though.
- You know you won't get trapped in a loop because you always choose something better, so your current node is better than your parent node and there's no way you'll choose the parent again.

### Hill Climbing Search
- Tiledriver has uniform cost, so this isn't so useful for the project, but if costs aren't always the same, you can use this to find a local maximum.
- Part 2 might use Random Restart Hill Climb.

### Simulated Annealing Search
- It's a way to do a search without throwing away a previous solution.
- Assume you want a global minimum. If you're already at a local minimum, then you choose a direction based on rate of schedule, and the probability that you find the global minimum approaches 1 and the rate approaches 0.

### Local Beam Search
- All other searches had 1 current state at a given time. 
- This one keeps `k` random current states in memory, and each iteration generates every successor.
- Out of the generated `kb` successors, where `b` is branch factor, you pick the `k` best states. Repeat for each `k`.
- Problem: You might skip over the path to the best, and you might cluster towards a local optimum instead of a global optimum.
- "Diversity" is extremely important in Genetic Algorithms, which he will cover next time.

## Something about `conflict_tiles(tiles)`.
- Most moves that you make are not going to change the number of linear conflicts. So there's going to be lots of plateaus.
- This means that most moves you make are not going to have any impact on linear conflicts. As you go into higher and higher n-values, the width of plateaus decrease.
- Why is this important? Consider the 3 main algorithms we studied today.
- Local beam would perform poorly because of its randomness. If all start at 0 and then one finds a 1, then they all flock to 1 even though the best might be a stair of 5s.
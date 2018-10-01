# CSC 480 - Lecture 1
Monday, September 24, 2018

**THESE NOTES ARE SUPPLEMENTARY TO THE LECTURE SLIDES**

*Note: Review Manhattan distance, see `tiledriver.c` from CSC 225*

## Problem Descriptions
### Sliding Tile Puzzle
The reason why puzzle has `num_squares! / 2` configurations:
- Factorial makes sense, you take one spot and now there's `(num_squares - 1)!` remaining.
- Half of the configurations that you generate are unsolvable. For example, if you take a solved puzzle and swap two tiles, it's unsolvable.

### N-Queens
- The goal in an 8-queens problem would be to place all 8 queens on the board such that none are attacking each other.
- `(n^2)!` configurations.

### Knuth's Problem
- Def: Reach any integer `n` using only factorial, square root, or floor.
- Infinite configurations.

### Romania Travel Problem
Objective is to find shortest route.


## Problem Solving

### Components of Problem Solving

**Initial State**
- Need some way of representing what a state is. Ex: In Romania travel problem, the initial state is Arad.
- Ex initial state known in advance: Chess
- Ex initial state not known in advance: Minesweeper, Sliding Tile Puzzle, Rubix Cube
- Ex initial state not known at present: Finding your way out of a maze, also Minesweeper again.

**Action State**
- Ex always the same: Rock paper scissors
- Ex never the same: Tic tac toe

**Transition Model**
- Represent model as a function that takes state and action for that particular state and returns a new state: `Result(s, a) = s'`

**Goal Test**
- Lets us know if we've gotten to the end. We may apply the goal test at different times, such as when we see the state on the frontier or we've arrived at the target state.
- Ex goal test is computationally expensive: Shortest or most optimal anything.
- Ex multiple goals exist: Maze with multiple exits.

**Path Cost**
- How expensive it is to get from the initial state to the goal.
- Ex path cost irrelevant to solution: When end state is independent of process (if goal is just winning at chess, doesn't matter how you did it), or solving Knuth's problem, or N Queens problem you only care where the queens are at the very end but you don't care how you solved them.
- Ex cost is difficult to calculate: Optimization problems.

### State Space
- **Explored States**: Program has already checked this state, or has already generated its frontier.
- **Frontier States**: Unexplored states adjacent to at least one explored state.
- With tiledriver, you will want to represent these two states in a certain way to maximize efficiency, using a set. You only need to check if you've explored them, you don't care about when or how. For membership checking, set membership checking is constant time.
- For frontier, use a priority queue for greedy pathfinding. Use `heapq` priority queue module.

### Search
Searches may be *informed* or *uninformed*. Definitions next time.

For next time: Go over the two types of searches, graph traversals and A* pathfinding.
# CSC 480 - Lecture 2
Wednesday, September 26, 2018

## Pre-class misc.
For `tiledriver`, you might want a State object like so:

```python
class State:

    def __init__(self, tiles, path, distance_to_goal, cost):
        self.tiles = tiles
        self.path = path
        self.distance_to_goal = distance_to_goal
        self.cost = path + distance_to_goal

    def __hash__(self):
        # You'll have to implement this method so you can add State objects to a set.
        pass

    def __eq__(self):
        pass # This method too.
```
- For frontier states, you want to use a `heapq` (priority queue).
- For explored states, you want to use a `set`.

## Ways to measure graph performance:**
- `b`: Maximum number of branches from a given node.
- `d`: Shallowest depth of goal(s).
- `m`: Maximum depth of graph.

## Uninformed Searches
- We only know what the cost of solving the search was, or how far we've searched so far, but we don't know how close we are to the goal.
- You can solve any problem this way, it just might be super inefficient.
- Ex where uninformed search is necessary: Maze
- Ex where uninformed search is impossible, even with infinite computing power: Theoretically a DFS on a number line.

### Breadth First Search: See slide.
### Depth First Search: See slide.

### Iterative Deepening DFS (ID-DFS)
- Works on the notion of limits on the height of a search.
- On each iteration, increases limit of DFS; a standard DFS has a limit of infinity.
- Sort of a depth-breadth-first-search.


### Uniform Cost Search (UCS)
- All we know is how far we've come up until now.
- This is essentially Djikstra's algorithm. 
- Not greedy! Greedy algorithms look at distance to each frontier and choose smallest. This one looks at which direction would minimize cost so far instead of additional cost.

### Bidirectional Search
- You have two BFS searches running. One starts at your "starting point", the other starts at your goal.
- You're done when both searches have a common node in their frontiers.
- This goal could be used to test a pathfinding algorithm, not a "solving" algorithm. Ex: Romania Problem.
- This would be impossible if you don't know what you're looking for. Ex: N Queens problem.

## Informed Searches
- Since you know where the goal is, you can evaluate the quality of your frontier states using a **search heuristic**, or a *reasonable* estimated distance to the goal.
- Ex of a problem where Informed Search is only sometimes possible: Romania travel problem, given excellent weather and no traffic, you can come up with an estimate of how long it might take. If there's an accident, then your estimate is no longer accurate. Some variables are random.
- Ex of a problem where an Informed Search is never possible, even with infinite computing power: In driving, if you don't have a map or GPS, you don't know how far things are so you can't calculate a heuristic.

### Defining Heuristics
- In sliding tile puzzle, you calculate Manhattan distance by assuming you could slide a tile over others (even though the problem doesn't allow that). By relaxing that constraint, you come up with a heuristic that's much easier to calculate than another solution to that problem.
- Ex of an even simpler heuristic for Sliding Tile Puzzle than Manhattan Distance: How many tiles are not in the correct place.
- Ex heuristic for Romanian Travel Problem: Euclidian distance.
- Ex heuristic for N-queens: How many queens left to go.

### Greedy Search
- Always select node with lowest `h(n) = estimated distance to goal from state n`
- Not necessarily complete, depends on the following factors:
    - Requires finite state space, or no loops. Imagine if you were following a path to a goal and the shortest path is always smaller than the correct path and you form a spiral.
        - This is possible because greedy doesn't keep track of how far you've gone, it only cares about what's the next shortest path.

### A* Search
- A* just combines 2 things together: Uniform Cost Search + Greedy Search.
- `g(n)` = cost from start to current node, which is UCS.
- `h(n)` = cost from current node to goal, which is Greedy.
- `f(n) = g(n) + h(n)`
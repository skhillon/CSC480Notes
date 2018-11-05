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

# CSC 480 - Lecture 5
Friday, October 5, 2018

*See biogimmickry on GDrive*
- Starting values are all 0.
- You cannot assume that the program starts at index 0 of the array.

## Genetic Algorithms: Intuition
- Might seem like ML, but is firmly in the family of searches.
- Like a Beam Search, we have some set of initially random states, and we want to generate successors from those states in a way that solves the convergence problem from Local Beam Search, something to "inject" diversity into our candidates.
- Assume some initial population of values (slide shows N Queens problem, each number represents a row on the board that contains a queen).
- Take 2 states (the shaded ones) and perform a **crossover step** where you randomly choose one partition from each sequence and combine them together into a new state.
    - Each crossover results in 2 children.
- The **fitness function** is an evaluation function that chooses which 2 states to cross over. The probability of being selected for crossover is directly proportional to its fitness score.
- Once you create 2 children, there is a final chance of **mutating** the sequence (meant to help increase diversity)
- The idea is that, if a candidate is good, its probability of being selected (or a chunk of it being selected) increases due to its high score. Pretty much like reproduction in evolution.

## Terminology
See slides

## Terminating Conditions
- You could use time or number of iterations, or when you find an individual with a certain fitness score. For this project, you will use the Euclidian distance between your produced array and the given solution, and once it hits 0 then you're done.
- One remaining (only applicable if probability decreases with each generation)
- Convergence where all individuals start to look very similar. Once it crosses a certain similarity threshold that you define, you could terminate.

## Fitness Functions
- Are unique to the problem that you're trying to solve. Every other step is pretty much boilerplate.
- Tend to be the bottleneck of the algorithm runtime, may not be linear or even polynomial in runtime.
- Because it can be applied on to individuals, it doesn't need to be done on the same thread. So you could distribute your fitness systems across different cores or systems to make it much faster.

## Selection
- Individuals are selected for crossover based on fitness function.
- Given a random numeric threshold `R`, after all members of the population are scored, you add all the scores to a running `total` until `total >= R`.
- The individual whose score pushes `total` over `R` is selected. This doesn't guarantee the "best" individual, but it does guarantee diversity.


## Crossover
- Randomly partition somewhere in the parent (make sure it's not 0 or the end, because then there's effectively no partition). Generate 2 children.

## Mutation
- Mutate the child to keep the population diverse. This prevents stagnation in a local optimum.
- It allows a small chance that a crossover child will haev its elements altered by either an add, remove, or edit.
- Protip: For project, just don't mutate a loop.
- Protip: Make the probability of add slightly less than remove.

## Generational Complexity of a Genetic Algorithm
- For a particular generation, our fitness function will run in `O(F * N)` time.
- For each new individual, selection and crossover is `O(N)` and mutation is constant time.
- Therefore, each generation `G` runs in `O(N^2 + F * N)`.
- This is best case, fitness function over linear is very common and will end up dominating the runtime.

## Question: What is this used for?
- In a machine learning problem, you might have a vector of values. You won't necessarily know if each number represents the feature of a problem. You don't know how much weight to give each feature.
- Let's say each value was a grayscale value in an image, and you're trying to classify a cat or a dog.
- A lot of pixels will not contribute a whole lot (like the ones in the corner/background), so you want to disregard them.
- You apply a new vector of normalized weights to the original values.
- To find an ideal set of weights, you use a genetic algorithm (in a supervised problem) and keep changing them. This is literally the learning in supervised learning.

# CSC 480 - Lecture 6
Monday, October 8, 2018


Now that we know the concepts of genetic algorithms, we're going to move to creating adversarial (game) AIs.

## Defining Intelligent Agents [1/2]
- Ex: Humanity & Thought (Internal): Psychology
- Ex: Rationality & Thought (Internal): Philosophy and logic.
- Ex: Humanity & Action (External): Sociology, Turing Test.
- Ex: Rationality & Action (External): Artificial Intelligence.

*Point is, we're not trying to create humanoid, thinking robots; we're trying to create **agents** that are good at what they do*.

## Agent Model: See Slide
- Easy to extend agency to a robot, but we can even take this down to the software level:
    - Environment would be its state space, for some software that's the entire internet. Ex: Web Crawler.

## Percept Sequence: See Slide
- see slide
- This is not an algorithm, it's a way of thinking about AI.

## Rational Agent Requirements
- Need to do the "right thing" under **uncertainty**. If an agent had infinite knowledge, then it would be trivial to know what to do at any point in time. You wouldn't *need* AI.
- It needs to choose the *best* action given what it knows. This is dependent on the problem space.
- This is really about maximizing whatever our performance measure is. In `tiledriver` this is heuristic and speed.

## Vacuum Cleaner World (also in textbook)
- Given rooms A and B, along with a bot, the vacuum is looking to gather dirt.
- Each room can be either clean or dirty.

Vacuum Possible Actions:
- Move Left
- Move Right
- Suck

## Performance Measure [1/2]
- This is how we judge whether the agent is being "rational"
- It's based on the state of the environment, not the state of the agent.

**Possible Performance Measures for Vacuum Bot:**
- Amount of dirt left in room.
- Amount of rooms that are dirty.
- Amount of time taken to get rid of all dirt.

**Why is Amount of dirt cleared a bad performance measure?**
- It doesn't take into account how much dirt is in the room. You could specify a threshold, and the bot would clean the same regardless of how dirty or clean the room was. 
- Doesn't take into account efficiency. 
- If there's no priority to the rooms, then it might clean the dirtiest room that no one actually goes in.
- The agent could also do something sneaky: It could keep vacuum out dirt and then dump it out, effectively cleaning up `x` times the desired amount, where `x` is the number of times it dumps out dirt.


# CSC 480 - Lecture 7
Wednesday, October 10, 2018

Fun fact: If you just want to see what your score is without submitting, use Casey with `-n`.

## First Writing Assignment - Chatbots
Going to be using the Mitsuku chatbot. My professor is forcing me to become a weeb. Due Friday of next week.

# CSC 480 - Lecture 8
Friday, October 12, 2018

We've covered chapters 1-4 in the book so far.

## PEAS

yeah nothing much today.

# CSC 480 - Lecture 9
Monday, October 15, 2018

## Adversarial Search
- Ex 2 players, not zero sum: Prisoner Dilemna
- Ex 3+ players, is zero sum: Racing


# CSC 480 - Lecture 10 - Nothing

# Lecture 11
Friday, October 19, 2018

## Project Notes
### Crossover Test Cases
- Crossed over do not equal parents
- Combined length should equal combined length of parents
- If it's possible to have a loop in both, both should have it. ???

### Program Length
- Max steps wont be longer than 10_000
- Simple programs -> 10-20 seconds, max 1 minute
- Iterative -> slightly longer for him, max 2 minutes


- If you're at either end and you try to go forward/back, then it should not do anything.

## EXTRA CREDIT: SHOW "I VOTED" STICKER

## Writing Prompt
- Can also address something else that you might want.

## Game Trees
- Each level is a "ply" in a 2-player turn-based game, alternates between players/objectives.
- One is trying to max utility, other is trying to min. Returns 1 or -1, 0 if tie.
- Results in **Minimax Algorithm**

## Minimax Algorithm
- Performs exhaustive DFS so that, at worst, player will tie the game.
- Problem is that it only works for very simple games.
- Look up alpha-beta pruning.

## Transposition tables 
- Map collection of states to **one** utility value.
- Need to be careful with how you rotate them; you have to rotate ALL of them the same way
- Can also reflect (flip upside-down) the whole cube. Is the easiest transposition, can simply `reverse(board_list)`. REQUIRED, up to 16.

## Estimating Utility
- When exploring large game trees, you search a portion and use a heuristic.
- First decision of tic tac toe (2d) will be very slow, then subsequent ones will be faster.

## Monte Carlo Tree Search (MCTS)
- Since heuristics are necessarily game-dependent and not generalized, we use this.
- MCTS finds probable winning move sequences.
    - "Simulates" playing by making random moves by current state and recording wins and losses per path.
    - With enough stats and moves, it can start selecting "intelligently".
        - Without sufficient information, turns into a random search. Ex, if you do 500 out of 1000 total allowed moves, you might find that one is already "starting" to seem better. At some point, you will stop random selection and make smarter choices (**UCB**)
    - When time limit reached, chooses move with best win-to-loss ratio.
- Monte Carlos family of searches basically get more effective as `n -> inf`.

## Upper Confidence Bound (UCB)
- Given a set of slot machines, you want to choose the one with the biggest payoff.
- If there's no one else to watch, you have to do the experiments on your own to find that one.
- You might pick one out of 3 that you won with, and then you keep losing after that.
- At some point, you get "sick" of trying that first selection, and you go back to one of the other 2. At some point, the last one will look like it's worth exploring.
- This is **exploitation versus exploration** at play. When you believe that one is good, you want to exploit (play) that. But, at some point, you're going to start exploring other options to see if they turn out to be better than they seemed. You use UCB for this.
- See slide for formula.
- Can generalize UCB for any turn-based game.
- Why did AlphaGo lose in the 4th game? There's a chance to have narrow paths to victory or loss, and AlphaGo overlooked those conditions. New version (AlphaGo Zero) is trained off of Neural Nets where it trains against itself.
- Recommend creating the driver so you can play against it.

# Lecture 12 - Quiz day

# Lecture 13
Wednesday, October 24, 2018

## Sensorless Agent
- Instead of using actual states, agents can use **belief states** which are composed of actual states.
- Given some state the agent is in, even though we don't know what that is, it could be one of `n` possibilities. Those possibilities make up the belief state.

1. For `N` actual states, how many belief states are possible? 
    - Ans: 2^n (Each state is either in or not in belief state, and there's `n` of them).
2. What are the possible actions available?
    - Guaranteed: Intersection of all actions in belief state.
    - Possible: The difference of the union of each action state and `guaranteed`.
3. How does the agent know if it has achieved its goal?
    - O

## Sensorless Vacuum Agent
**Belief State Space**: 

**Known Goal States**: 7, 8

# Lecture 14 - Nothing

# Lecture 15
Monday, October 29, 2018

## BoardStupid discussion
- Tests going to give boards that are almost complete. No empty boards.
- If you've got multiple choices, you have very little data on moves (starting) you can do random playthrough, backtrack data up, giving data points as you go.

## Final Project Discussion
**OPTION 1: PRESENTATION**
- What he did last time was everyone chose a topic of interest, AI related, and they gave a 10 minute presentation on that.
- What's been attempted in the past, the algorithm, case studies, how it has practical value, etc.
- Peer evaluation
PROS: no implementation
CONS: some people don't like presentation.

**OPTION 2: OpenAI Gym**
- Potentially scarier, has the chance to be more fun.
- Use Open.AI Gym that has collected a bunch of different environments to experiment with reinforcement learning techniques.
- Interface with other libraries like Tensorflow and Keras to "Teach" an agent how to act.
- Because there's a lot that you'd have to learn and pick up, this would be done in small groups (probably 2 maybe 3)
- **Implement and then give a presentation.**

**We're done with search, now moving to Knowledge Representation**
## Knowledge-Based Agent
- An agent may use a knowledge base, which maintains an internal representation of its environment. Info is represented in its own langauge--we use a Knowledge Base (KB) language.
- Knowledge may derive from initial background knowledge or information gained by exploring the environment.
- KB contains an infinitely expanding collection of sentences.
- **Sentence**: Logical conclusions derived from existing sentences or **Axioms** which are not derived from other sentences.
- Knowledge can be accessed and updated by **telling** KB new sentences and **Asking** about existing sentences.

## Propositional Logic
- KB sentences can be composed of **propositions**, which are joined together by operators.
ex) `is_cs_major and taking_480`
- Just remember 480, operators + truth tables.

## The Wumpus World: PEAS
- There's an adventurer that dies if it touches a wumpus or falls in a pit.
- If you are 1 step away from a wumpus in any direction you can stell it (stench), if one step away from pit then you feel a breeze.
- Can't move diagonally. Adventurer gets one arrow to kill a wumpus, gold is somewhere in the cave. Wumpus can be in a pit.
- 1, 1 must always be clear.
- When gold is beneath you, you see a glitter.
- When you get back to 1, 1 you win.

### P - Performance Measure
- We want to penalize death of course, and wasting your arrow by not hitting a wumpus.
- Number of moves, want to minimize.
- Reward for gold and maybe for killing a wumpus.

### E - Environment
- Partially observable - can only see your percept on a particular square.
- Static: Wumpuses and squares don't move.
- No determinism other than generating environment.
- Discrete, sequential, single agent (mostly because wumpus does not move, it is not adversarial, think of it like poison that you can kill).

### A - Actuators
- Can move forward or turn 90 degrees left or right (so you can aim in a direction with your arrow)
- Grab gold, shoot arrow, climb ladder for entrance or exit.

### S - Sensors
- Feel breeze, smell stench, hear wumpus scream, see glitter for gold, feel wall bump (so you didn't keep walking into a wall, avoid infinite loop).

### Understanding Find best move
- Check if a node is a transposition of another
- Transpositions frozenset, calculate wins/total ratio.
- If a is a transposition of b, then check the result in the table which stores the wins/total ratio for that state.
- State = empty node.

# Lectures 16/17 - Nothing

# Lecture 18 (also quiz day)
Monday, November 5, 2018

## Entailment
- A way we can determine that something else is true from previous knowledge.
- Often used with respect to the KB.
- Often implemented with sets, differences, unions, etc.

## Logical Inference
- Entailment allows you to generate new sentences based on inferences.
- Given alpha = "All men are mortal" and b = "Socrates is a man", then beta = "Socrates is a man".

## Inference by Enumeration
- Effectively the "exhaustive search" approach.
- If we want to see whether some sentence alpha is the case, we can enumerate through every model and check if KB implies alpha.
- Has time complexity 2^n. *Not* the method we usually use; that will be introduced next time.
- Project will involve implementing a resolution prover.

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
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
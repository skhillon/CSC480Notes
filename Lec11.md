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
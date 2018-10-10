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



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


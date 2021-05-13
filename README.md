# EECS738_RL_projcect

## Project 4 - Treasure Hunters Inc.

## project pipeline
1. Set up a new git repository in your GitHub account
2. Think up a map-like environment with treasure, obstacles and opponents
3. Choose a programming language (Python, C/C++, Java)
4. Formulate ideas on how reinforcement learning can be used to find treasure efficiently while avoiding obstacles and opponents
5. Build one or more reinforcement policies to model situational assessments, actions and rewards programmatically
6. Document your process and results
7. Commit your source code, documentation and other supporting files to the git repository in GitHub

## Initial configuration

Treasure map detail analog:

- map dimension 5x5
- hunter starts at (0,0)
- there are four potential actions to move: up/down/left/right or stay
- the forbidden sign in cells indicates obstacle which hunter can pass through
- the blue devil is a monster which walking into it's territory(single cell) would cause harm(negative reward)
- the treasure box is teh end goal of the hunt

![WeChat 截圖_20210512233658](https://user-images.githubusercontent.com/42806161/118079804-6ebc0880-b37e-11eb-9c37-4b93e3bb442c.png)

Reward system:
- agent would be roled as the hunter starting from (0,0)
- random moves will be initiated and exploration would lead at the first few steps
- rewards are weighted given difference moves, rewards for the hunter incountering obstacles,monster,treasture,valid cell and invalid cell are respectively -5, -10, +10, +1 and 0
- the rewards system will help the agent learn about the environment and maximize the rewards.

As shown in the ouput reward_table below, a 25x5 matrix is used to represent all possible [action] rewards in each [state]:

![image](https://user-images.githubusercontent.com/42806161/118081060-40d7c380-b380-11eb-915a-d96b43757bf8.png)

The initial q-table would be set to same dimension matrix of zeros.

Similarly, transition table would be initiated. The transition table lays out all the possible movements to another state given the current state. The invalid move is marked as -1 and no movement is marked as the corresponding state number. The transition table will be referred to for tracing out the path to treasure chest through the maze where value represents positions of the map.

![image](https://user-images.githubusercontent.com/42806161/118081279-a035d380-b380-11eb-8907-d7f1731d5ac5.png)

last, action table is initialized given possible [actions] on each [state]: The action table represents the valid actions that can be taken by the agent at each position(0-24 in columns). Here 'up, down, left, right and no action' are encoded as 0, 1, 2, 3 and 4 respectively. this table should be checked before each action.

![image](https://user-images.githubusercontent.com/42806161/118081338-c8253700-b380-11eb-8da6-a899d83856e2.png)


## Reinforcement Learning basis
For the treasure hunt project, we are not digging into deep learning, so Q-learning would be the best algorithm to go with. 
Quick recap on Q-learning, it attempts to learn the value of being in a given state, and taking a specific action. The core of Q-learning is updating the q-table which contains [state,action].

Bellman Equation is well defines for the update algorithom:
Q(s,a) = r + γ(max(Q(ś,á))

Updated Q(state,action)←(1−α)Q(state,action)+α(reward γmaxaQ(next state,all actions))

' α ' is the learning rate. ‘ s ’ is current state, ‘ a ’ is the action the agent takes from the current state, ‘ ś ’ represents the state resulting from the action. ‘ r ’ is the reward for taking the action and ‘ γ ’ is the discount factor. So, the Q value for the state ‘ ś ’ taking the action ‘ á ’ is the sum of the instant reward and the discounted future reward (value of the resulting state). The discount factor ‘ γ ’ determines the importance of future rewards.

to sum up the process of Q-learning:

* Initialize the Q-table by all zeros.
* Start exploring actions: For each state, select any one among all possible actions for the current state (S).
* Travel to the next state (S') as a result of that action (a).
* For all possible actions from the state (S') select the one with the highest Q-value.
* Update Q-table values using the equation.
* Set the next state as the current state.
* If goal state is reached, then end and repeat the process.

A iterration of 500 is set to this Q-learning run, we get a best q-table as below:

![image](https://user-images.githubusercontent.com/42806161/118081647-644f3e00-b381-11eb-8662-9794a3ae73b0.png)

From the updated q-table we then can come up with the best route the agent had during the RL process, as shown in the analog route.

![image](https://user-images.githubusercontent.com/42806161/118082762-5d292f80-b383-11eb-985f-b8a39e5b0c64.png)

## Ref:
https://www.learndatasci.com/tutorials/reinforcement-q-learning-scratch-python-openai-gym/

https://www.freecodecamp.org/news/an-introduction-to-q-learning-reinforcement-learning-14ac0b4493cc/

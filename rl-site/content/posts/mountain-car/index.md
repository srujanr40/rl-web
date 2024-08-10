+++
title = 'Mountain Car Problem'
date = 2024-06-21T16:01:00-07:00
draft = false
tags = ['article']
+++

{{< katex >}}

## Introduction to the problem

The mountain car problem involves a car positioned on a hill with a goal at the top of the hill. The car is affected by gravity. The agent can fully observe the state of the environment at any given time step. The agent can take one of three actions:

- move the car to the left
- move the car to the right
- do nothing

>An environment for this problem is generously available in the OpenAI Gym library through the CartPole-v1 environment. This library provides all the necessary tools to set up the problem and interact with the environment, allowing us to focus on a solution of the problem instead of the details of the environment. It also allows us to visualize the mountain car, and easily observe how the actions of the agent affect the positions of the car on the hill.

*Demo of the problem with the agent taking random actions*

![image gif of mountain car with the agent taking random actions, the car does not make its way to the goal](./images/mountain_car_random.gif)

## The Goal

**The goal of the problem is to get the agent to move the cart to the goal at the top of the hill in as few time steps as possible**. To do so, the agent needs to perform actions that move the car to the left or right just the right amount at each time step such that the car reaches the goal in the least amount of time.

An intuitve solution that we might consider is to constantly move the car in the direction of the goal. However, as we will soon see, this solution is not optimal.

## What can the agent observe?

The agent can observe the **position of the car** on the x-axis, and the **velocity of the car**. The mountain car starts randomly at a position on the x-axis in the interval [-0.6, 0.4]

## Approach

### Q-Learning

The agent will need to understand that it must move the car to the goal optimally. To do so, we can discourage the agent from taking too many actions using penalties. The agent is penalized by -1 at each time step until the agent reaches the goal.

To teach the agent to get the car up the mountain, we can use Q-Learning utilizing Bellmans' equations. Q-Learning is an algorithm that learns from the environment by updating the Q-Values of the state-action pairs. By repeatedly training the agent on the same environment, the agent can take actions and then observe the outcome of the action and the reward it obtains from said action. We can use this to build a state-action pair table with values that correspond to the probability of maximizing rewards based on the action taken.

> **Bellman's Optimality Equation for Q-values**
$$
 Q[s_t, a_t] = r[s_t, a_t] + \gamma \max_a Q[s_{t+1}, a]
$$

> **Q-Learning Algorithm to update Q-values for state-action pairs**
$$
 Q[s, a] \leftarrow Q[s,a] + \alpha((r + \gamma \max_{a'}Q[s',a']) - Q[s,a])
$$

#### Gamma
- Gamma (\\(\gamma\\)) is the *discount factor*, and it influences how future rewards are valued immediate to current rewards. It is bounded between 0 and 1.
  - A value of 0 represents valuing only immediate rewards with no concern for future rewards.
  - A value of 1 represents valuing future rewards equally as much as immediate rewards.

#### Alpha
- Alpha (\\(\alpha\\)) is the *learning rate*, and it influences the extent to which the new information overrides the information learned in earlier iterations. It is bounded between 0 and 1.
  - With an (\\(\alpha\\)) closer to 1, the agent learns quicker because it heavily weighs new information. However, it can lead to instability as the agent may overly adjust to recent experiences.
  - With an (\\(\alpha\\)) closer to 0, the agent learns slowly as it relies more on the existing Q-values. This can lead to more stable learning but may also cause the agent to take a long time to adapt to changes in the environment.
  - For this problem we will run 2 versions of Q-learning: one with alpha set to a constant 0.1, and another with a decaying alpha where alpha is set to 1/k where k is the number of times an (s, a) pair has been seen.


### Epsilon Greedy Policy

Q-learning can create a state-action pair table, but does not tell the agent what action to take at what state.

To be able to exploit this data, we can use an epsilon-greedy policy. We want the agent to take more random steps at the beginning of training as it encourages exploration, allowing the agent to explore all possible actions and paths that may lead to the highest overall rewards rather than pigeonholing it into one series of actions. However, with enough iterations and enough exploration, we would like the agent to get greedier and take more action where the Q-value is higher so it gets better at refining the optimal series of actions for maximum rewards.

With the epsilon-greedy policy, we define a parameter (\\(\epsilon\\)) that determines the chance of taking a random action. The agent selects the greedy action with probability 1 - (\\(\epsilon\\)), and selects a random action otherwise. The greedy action is the action that maximizes the Q-value. We can start with a high epislon value to encourage the agent to explore. By decreasing the value of epsilon over time, we can get the agent to exploit instead of explore.

> **Epsilon-Greedy Policy**
> ``` python3
> x = random(0, 1)
> 
> if x < epsilon:
>     do random action
> else:
>     do greedy action [argmax(Q(s,a) for the given state)]
> ```


## Results

With both an alpha value of 0.1 and a decaying alpha, the Q-learning algorithm was run for 20000 episodes with 5000 steps per episode. With both versions of the Q-learning algorithm, the agent was able to learn the optimal policy to get the car up the mountain in as few time steps as possible.

We observe here that the optimal policy is not to simply to constantly move the cart towards the goal at every time step. Instead, it is to leverage gravity to swing the cart back and forth and use the momentum to get the cart up the mountain.

![image gif of car getting up the mountain optimally](./images/mountain_car_optimal.gif)
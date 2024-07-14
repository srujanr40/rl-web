+++
title = 'Cartpole Problem'
date = 2024-06-21T16:01:00-07:00
draft = false
tags = ['article']
+++

## Introduction to the problem

The cartpole problem involves a movable cart on a track with a pole attached to its center. The only actions the agent can perform are to move the cart a step to the left, or a step to the right. The agent can fully observe the state of the environment at any given time step. The pole on the cart can rotate freely and is weighed down by gravity.

An environment for this problem is generously available in the OpenAI Gym library through the CartPole-v1 environment. This library provides all the necessary tools to set up the problem and interact with the environment, allowing us to focus on a solution of the problem instead of the details of the environment. It also allows us to visualize the cartpole, and easily observe how the actions of the agent affect the positions of the cart and pole.

*Demo of the problem with the agent taking random actions*

{{include image here}}

## The Goal

The goal of the problem is to get the agent to keep the pole on the cart upright. To do so, the agent needs to move the cart to the left or right just the right amount at each time step such that the movement of the cart balances the movement of the pole against gravity.

## Approach

To approach this problem, we will use the OpenAI Gym library to create an environment for the problem. We will use the CartPole-v1 environment. This provides a 

## heading 3
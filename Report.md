# Report: Project 2 - Continuous Controls

## Method: DDPG
In this project, the an adaption of the DDPG algorithm discussed during the course is applied. The DDPG algorithm has been adapted to work with 20 agents in parallel to collect experiences. As pointed out in the benchmark of the project, the actor anc critic networks are updated after 20 timesteps, 10 times. Inputs are sample randomly from the replay buffer filled from all 20 agents.

### Hyper Parameters:
- replay buffer size: 1e6
- minibatch size: 1024
- discount factor gamma: 0.99
- tau for soft update of target parameters: 1e-3
- learning rate of the actor: 1.5e-3
- learning rate of the critic: 1e-3
- L2 weight decay: 0
- Maximum number of timesteps: 10000

### Neural Network Architecture
#### Actor:
- Input: state-size (33x1)
- Fully-connected layer with RELU: 256 nodes
- Fully-connected layer with RELU: 128 nodes
- Output layer with tanh: 4 nodes

#### Critic:
- Input: state-size (33x1)
- Fully-connected layer with RELU: 256 nodes
- Fully-connected layer with RELU: 128 nodes
- Fully connect layer: 1 node

# Plotting Rewards
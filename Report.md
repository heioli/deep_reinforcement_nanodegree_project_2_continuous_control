# Report: Project 2 - Continuous Controls

## Method: DDPG
### Learning Algorithm
In this project, an adaption of the **Deep Deterministic Policy Gradient** (DDPG) algorithm described in this [paper](https://arxiv.org/pdf/1509.02971.pdf) is used. DDPG is a *model-free and off-policy* **actor-critic** algorithm. Deep function approximators (in the form of neural networks) are used to learn policies in **high-dimensional** and **continuous** action spaces.

Two neural networks are used in the setup: The **actor** network is used to approximate the optimal policy **deterministically** or in other words the *best believed action* shall be outputted for any given state. The **critic** network on the other hand learns to evaluate the *optimal action value function* by using the *best believed action* of the actor. To train the network a **replay buffer**. Additionally, **soft updates** are applied, which help to slowly mix the most up-to-date **regular** network used during traing with the **target** network used for prediction. This allows more stable training and faster convergence.

### Adaption
The following adjustments are pointed out compared to the standard implementation discussed during the course:
- The DDPG algorithm has been adapted to work with 20 agents in parallel to collect experiences. This has lead to an adjusted step function: step_multi. As pointed out at the benchmark implementation of the project, the actor anc critic networks are updated after 20 timesteps, 10 times. Inputs are sampled randomly from the replay buffer which is filled from experiences of all 20 agents.
- To stabilize the code and increase learning stability, I implemented additionally:
    - Epsilon decay has been added to reduce the added noise in the act function over time. This turned out to stabilize the learning additionally over time.
    - Batch normalization layers in Neural Network
    - Gradient clipping at critic agent

### Hyper Parameters:
The following hyperparamters turned out to lead to an efficient learning:
- replay buffer size: 1e6
- minibatch size: 256
- discount factor gamma: 0.99
- tau for soft update of target parameters: 1e-3
- learning rate of the actor: 1.5e-4
- learning rate of the critic: 1e-3
- L2 weight decay: 0
- Maximum number of timesteps: 1000
- Epsilon Decay factor: 0.9999

### Neural Network Architecture
The neural network consists of two separate neural networks with the following setup:
#### Actor:
- Input: state-size (33x1)
- Fully-connected layer with RELU: 400 nodes
- Batch normalization layer 1D
- Fully-connected layer with RELU: 300 nodes
- Output layer with tanh: 4 nodes

#### Critic:
- Input: state-size (33x1)
- Fully-connected layer with RELU: 400 nodes
- Batch normalization layer 1D
- Fully-connected layer with RELU: 300 nodes
- Fully connect layer: 1 node

## Rewards Plot - The solved environment
Criterion for solving the environment: Agents must get an average score of +30 (over 100 consecutive episodes, and over all agents). As pointed out in the project instructions this means specifically:
>- After each episode, we add up the rewards that each agent received (without discounting), to get a score for each agent. This yields 20 (potentially different) scores. We then take the average of these 20 scores.
>- This yields an average score for each episode (where the average is over all 20 agents).

This environment has been resolved in: **109 episodes**. The following plot shows the averaged rewards (20 agents) obtained during the training. 



![Rewards Plot](Plots/Average_scores.png)

Results run in Udacity GPU environment can be found in : 
- Main file with run results: *'Continuous_Controls_v2_trained.ipynb'*. 
- *.pth contain stored checkpoint files. Checkpoint_actor_final.pth and checkpoint_critic_final.pth contain final version of neural network weights.
- ddpg_agent.py and model.py contain the adapted agent and model code.
- workspace_utils.py provides the active_session function to allow long training on the Udacity GPU workspace (provided by Udacity support).

## Ideas for future work
- Alternative neural network architectures for agent and critic
- Alternative actor-critic implementations as e.g. D4PG

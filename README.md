# Q-vs-DQN-vs-ModelRL-FrozenLake
Comparing the performance of Q-Learning, DQN, and Model RL (w. value iteration) on simple Frozen Lake.

### Q-Learning
- **Algorithm Implementation**
  - Implemented the Q-Learning algorithm.
  - Initialized Q-table with zeros.
  - Defined hyperparameters: learning rate, discount factor, and exploration rate.
- **Training Process**
  - Iteratively updated Q-values based on agent's experiences.
  - Used an epsilon-greedy policy for exploration and exploitation.
  - Decayed exploration rate over time.
- **Performance Evaluation**
  - Recorded the number of episodes required to reach the goal.
  - Calculated average rewards over multiple runs.

### Deep Q-Network (DQN)
- **Algorithm Implementation**
  - Built a neural network to approximate Q-values.
  - Defined network architecture: input layer, hidden layers, and output layer.
  - Implemented experience replay buffer to store agent experiences.
- **Training Process**
  - Trained the neural network using mini-batches sampled from the replay buffer.
  - Used a target network to stabilize training.
  - Updated network weights using the Bellman equation.
- **Performance Evaluation**
  - Measured the agent's ability to learn optimal policies over time.
  - Calculated average rewards over multiple runs.

### Model-Based RL with Value Iteration
- **Algorithm Implementation**
  - Constructed a model of the environment's dynamics (transition probabilities and rewards).
  - Implemented value iteration algorithm to compute optimal state values.
  - Used state values to derive an optimal policy.
- **Training Process**
  - Iteratively updated state values based on the Bellman optimality equation.
  - Improved the policy by choosing actions that maximize expected rewards.
- **Performance Evaluation**
  - Evaluated the convergence of state values.
  - Calculated average rewards over multiple runs.

### General Analysis
#### (Q Learning): **Explain the impact of the  exploration probability epsilon on the cumulative discounted rewards per episode earned during training as well as the resulting Q-values and policy**.
To answer the question, it appears that the higher the value of epsilon, the lower the cumulated discounted rewards per episode and in general. This implies that the model is well-explored already by a lower epsilon. In addition, we see that a higher epsilon leads to a more stable 'cdr' as our Q-values converge, due to the lower probability of taking non-optimal actions. However, at the beginning, a higher epsilon seems to 'learn faster' likely due to more aggressive exploration.

In terms of epsilon's affect on the resulting Q-values and policy, we can see that having a high epsilon results in more Q-values being explored as the agent learns the environment. In contrast, using a low epsilon results in what are likely less-accurate Q-values for non-optimal actions. Related to policy, a high epsilon results in more comprehensive policy (i.e. ability to choose best action regardless of start state) but in the long run suffers from learning slower compared to lower epsilons.

<img width="577" alt="Screenshot 2024-07-11 at 4 18 51 PM" src="https://github.com/shuh2002/Q-vs-DQN-vs-ModelRL-FrozenLake/assets/40676497/259c5f3b-28f0-4c61-95a3-f0dc08b48ff2">

#### (Q Learning vs Model-based RL): **How does model-based RL with value iteration compare with Q-learning in terms of data efficiency?**
In terms of data efficency, model-based RL (w. value iteration) seems to be more data efficent than Q-learning. We can observb this by noting that it reaches a reasonable cdr in fewer episodes than Q-learning. This is possibly due to it being model-based over model-free, where we learn an explicit model faster than trying to solve using many samples (i.e. Q-learning).

<img width="577" alt="Screenshot 2024-07-11 at 4 19 43 PM" src="https://github.com/shuh2002/Q-vs-DQN-vs-ModelRL-FrozenLake/assets/40676497/1ffa600d-d743-44ab-a133-5a5c1c52f8d5">

#### (DQN):
The more often we update the target network, the faster we see the target network follow the main network. That is, the quicker we 'learn' or apply information we've seen. However, if this information is bad data, updating too frequently results in sometimes moving the weights in a non-optimal direction. We can see this in the high-variance our red line results in after 300 episodes. In contrast, updating less frequently results in slower learning, but often more stable. We see this with the green line in the grpah above, that takes more than 150 episodes before progress is made, but maintains a relatively consistent upwards trajectory as it converges.

Both the target network and value iteration are attempting to solve for a value function, with value iteration operating as a model-based method and DQN operating on a model-free method. Value iteration can be thought of as finding the best reward (and thus action) of a given state, given the **defintion** of the model. In contrast, DQN is an extension of Q-learning that finds the best reward-action pair of a given state, given a set of **experiences** (and then sampling from them) from an unknown model.

#### (DQN):
From the results below, it appears that the batch size is heavily correlated to the learning rate of the network. That is, we see when we only take one sample and update on that, we learn extremely slowly (see red line) and only on a limited number of experiences. In contrast, when we sample on a large number of experiences (green line) we see extremely fast growth. Also in theory, a small mini batch-size would be better for exploration (avoid convergence to non-optimal), so more episodes might result in a mini-batch of 10 outperforming a mini-batch of 30.

The main reason why we use a replay buffer over exact gradient descent is avoiding 'temporal connections'. That is, we don't want the agent to learn only on sequential actions as it may lead to unstable training and highly-correalated updates. Another reason is in being efficent with a limited number of experiences. That is, we can re-use previous experiences multiple times to train our agent as opposed to once when we see it. This can greatly speed up learning as a result.

<img width="577" alt="Screenshot 2024-07-11 at 4 21 05 PM" src="https://github.com/shuh2002/Q-vs-DQN-vs-ModelRL-FrozenLake/assets/40676497/f56e961b-81dd-40d3-8b3a-67b00e950a81">




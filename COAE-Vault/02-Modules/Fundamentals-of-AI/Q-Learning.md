---
module: fundamentals-of-ai
category: general
tags: [ml, reinforcement-learning, rl, q-learning, q-table, bellman, epsilon-greedy, model-free]
tools: [PyTorch, NumPy]
attack-type: 
exam-relevance: core
---

# Q-Learning

## Summary

A **model-free RL** algorithm that learns an optimal policy by estimating **Q-values** — the expected cumulative reward for taking a specific action in a given state and then following the optimal policy. The agent learns entirely through trial and error with no prior model of the environment.

See [[Reinforcement-Learning-Algorithms]] for the core RL concepts (state, action, reward, policy, discount factor γ).

## The Q-Table

A lookup table storing Q-values for every state-action pair. The agent consults it to decide which action to take.

| State / Action | Up | Down | Left | Right |
|---------------|----|------|------|-------|
| S1 | -1.0 | 0.0 | -0.5 | 0.2 |
| S2 | 0.0 | 1.0 | 0.0 | -0.3 |
| S3 | 0.5 | -0.5 | 1.0 | 0.0 |
| S4 | -0.2 | 0.0 | -0.3 | 1.0 |

Rows = states, Columns = actions, Cells = Q-values.

![[assets/02 - Q-Learning_0.png]]

## Q-Value Update Rule (Bellman Equation)

```python
Q(s, a) = Q(s, a) + α * [r + γ * max(Q(s', a')) - Q(s, a)]
```

| Symbol | Meaning |
|--------|---------|
| `Q(s, a)` | Current Q-value for action a in state s |
| `α` (alpha) | Learning rate — weight given to new information |
| `r` | Immediate reward received after taking action a |
| `γ` (gamma) | Discount factor — importance of future rewards (see [[Reinforcement-Learning-Algorithms]]) |
| `max(Q(s', a'))` | Best Q-value available from the next state s' |

**The update rule pulls Q(s,a) toward the target `r + γ * max(Q(s', a'))`, scaled by α.**

### Worked Example

- State: S1, Action: Right → new state S2, reward r = 0.5
- α = 0.1, γ = 0.9, max(Q(S2, *)) = 1.0

```python
Q(S1, Right) = 0.2 + 0.1 * [0.5 + 0.9 * 1.0 - 0.2]
             = 0.2 + 0.1 * 1.2
             = 0.32
```

Q(S1, Right) updated from 0.2 → 0.32.

## Algorithm Steps

1. **Initialize** Q-table (typically all zeros)
2. **Choose action** — balance exploration vs exploitation (see epsilon-greedy below)
3. **Take action, observe** new state s' and reward r
4. **Update Q-value** using the Bellman update rule
5. **Update state** — set current state to s'
6. **Iterate** steps 2–5 until Q-values converge or stopping condition met

## Exploration-Exploitation Trade-off

The agent must balance:
- **Exploration** — try new/untested actions to discover better strategies
- **Exploitation** — use current best-known action to maximize immediate reward

### Epsilon-Greedy Strategy

The standard approach. With probability ε → explore (random action). With probability 1-ε → exploit (highest Q-value action).

| ε value | Behavior |
|---------|---------|
| High (e.g. 0.9) | Heavy exploration — agent tries many random actions early on |
| Low (e.g. 0.1) | Heavy exploitation — agent mostly uses what it has learned |

**Common practice:** start with high ε and decay it over time (exploration → exploitation as learning matures).

## Data Assumptions

| Assumption | Detail |
|-----------|--------|
| **Markov property** | Next state depends only on current state and action — not the full history |
| **Stationary environment** | Transition probabilities and reward functions don't change over time |

## Related Notes

- [[Reinforcement-Learning-Algorithms]] — core RL concepts: agent, environment, state, action, reward, policy, γ, V(s), Q(s,a)
- [[Deep-Learning-Fundamentals]] — Deep Q-Networks (DQN) replace the Q-table with a neural network for large/continuous state spaces

## Sources / References

- COAE Course Module: Fundamentals of AI — Q-Learning

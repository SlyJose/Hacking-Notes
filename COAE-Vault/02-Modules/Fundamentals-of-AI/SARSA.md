---
module: fundamentals-of-ai
category: general
tags: [ml, reinforcement-learning, rl, sarsa, on-policy, epsilon-greedy, softmax, model-free]
tools: [PyTorch, NumPy]
attack-type: 
exam-relevance: core
---

# SARSA (State-Action-Reward-State-Action)
![[assets/sarsa.png]]
## Summary

A **model-free, on-policy RL** algorithm that updates Q-values using the actual next action taken under the current policy — not the theoretical maximum. The name encodes the five elements of its update tuple: **s, a, r, s', a'**.

Compare with [[Q-Learning]], which is off-policy and always bootstraps off `max(Q(s', *))` regardless of what the agent actually does next.

See [[Reinforcement-Learning-Algorithms]] for core RL concepts (state, action, reward, policy, discount factor γ).
## Update Rule

```python
Q(s, a) = Q(s, a) + α * (r + γ * Q(s', a') - Q(s, a))
```

| Symbol | Meaning |
|--------|---------|
| `Q(s, a)` | Current Q-value for action a in state s |
| `α` (alpha) | Learning rate — weight given to new information |
| `r` | Immediate reward after taking action a |
| `γ` (gamma) | Discount factor — importance of future rewards |
| `Q(s', a')` | Q-value of the **actual next action** taken in next state s' |

**Key distinction from Q-learning:** `Q(s', a')` uses the action the policy *actually chose* in s', not the greedy maximum.

## Algorithm Steps

1. **Initialize** Q-table (typically all zeros) for every state-action pair
2. **Choose action** `a` in state `s` using the current policy (e.g. epsilon-greedy)
3. **Take action** `a`, observe next state `s'` and reward `r`
4. **Choose next action** `a'` in state `s'` using the **same policy** — this is the on-policy step
5. **Update Q-value** using the SARSA rule above
6. **Update state and action:** `s ← s'`, `a ← a'`
7. **Iterate** steps 2–6 until convergence or max iterations reached

## On-Policy vs Off-Policy

![[assets/03 - SARSA (State-Action-Reward-State-Action)_0.png]]

| Property | SARSA (on-policy) | Q-Learning (off-policy) |
|----------|-------------------|------------------------|
| Learns value of | Current policy (including exploration) | Optimal policy (regardless of current policy) |
| Next-state bootstrap | `Q(s', a')` — actual action taken | `max Q(s', *)` — best possible action |
| Safety | More conservative — avoids risky exploratory actions | More exploratory — can learn optimal routes faster |
| Use case | Safety-critical environments, policy stability required | Faster convergence to optimal in unconstrained settings |

Because SARSA accounts for exploration actions in its updates, it learns a policy that is aware of its own exploratory behavior. This makes it safer in environments where a wrong exploratory action could be costly.

## Exploration-Exploitation Strategies

### Epsilon-Greedy

![[assets/epsilon greedy.png]]

With probability ε → random action (exploration). With probability 1-ε → highest Q-value action (exploitation).

SARSA's on-policy nature makes epsilon-greedy more cautious: exploratory actions in s' feed directly into the Q-update, so the agent learns to account for them rather than assuming it will always act greedily.

### Softmax

![[assets/softmax.png]]

Assigns action probabilities proportional to Q-values — higher Q → higher probability, but all actions retain some selection chance. Produces smoother, more nuanced exploration than epsilon-greedy.

In SARSA, softmax encourages the agent to consider moderately promising actions, potentially yielding better long-term outcomes than pure epsilon-greedy in complex environments.

## Convergence and Parameter Tuning

| Parameter | Effect |
|-----------|--------|
| **Learning rate α** (high) | Faster updates, risk of instability and oscillation |
| **Learning rate α** (low) | Stable convergence, slower learning |
| **Discount factor γ** (close to 1) | Emphasises long-term rewards |
| **Discount factor γ** (close to 0) | Prioritises immediate rewards |

SARSA is guaranteed to converge to the optimal policy when:
- α decays over time (satisfying Robbins-Monro conditions)
- Exploration ensures all state-action pairs are visited infinitely often

## Data Assumptions

| Assumption | Detail |
|-----------|--------|
| **Markov property** | Next state depends only on current state and action — not full history |
| **Stationary environment** | Transition probabilities and reward functions don't change over time |

## Related Notes

- [[Q-Learning]] — off-policy counterpart; uses `max Q(s', *)` instead of actual next action
- [[Reinforcement-Learning-Algorithms]] — core RL concepts: agent, environment, state, action, reward, policy, V(s), Q(s,a)
- [[Deep-Learning-Fundamentals]] — Deep RL extensions replace Q-tables with neural networks

## Sources / References

- COAE Course Module: Fundamentals of AI — SARSA

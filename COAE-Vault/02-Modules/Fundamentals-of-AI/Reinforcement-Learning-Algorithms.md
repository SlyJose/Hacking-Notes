---
module: fundamentals-of-ai
category: general
tags: [ml, reinforcement-learning, rl, policy, reward, value-function, agent, environment]
tools: [PyTorch, TensorFlow]
attack-type: 
exam-relevance: core
---

# Reinforcement Learning Algorithms

## Summary

RL agents learn by **interacting with an environment** through trial and error, guided by rewards and penalties. Unlike supervised or unsupervised learning, there are no labels — the agent discovers optimal behavior by maximizing cumulative reward over time.

High-level coverage in [[ML-Learning-Paradigms]]. This note covers the deep mechanics and core concepts.

## Two Algorithm Categories

| Category | How it works | Analogy |
|----------|-------------|---------|
| **Model-Based RL** | Agent builds an internal model of the environment to plan actions | Navigating a maze with a map |
| **Model-Free RL** | Agent learns directly from experience without modeling the environment | Navigating a maze without a map — pure trial and error |

## Core Concepts

### Agent
The learner and decision-maker. Takes actions, observes consequences, and aims to learn a policy that maximizes cumulative reward.

*Examples: robot navigating a maze, chess engine, self-driving car.*

### Environment
Everything outside the agent — the physical world, simulator, or game — that responds to actions and provides rewards/penalties.

### State
A snapshot of the current situation the agent observes. Contains all information needed to make a decision.

| Example domain | State representation |
|---------------|---------------------|
| Maze robot | Current position + surrounding walls |
| Chess | Current board configuration |
| Self-driving car | Camera feed, sensor data, speed |

### Action
A decision the agent makes that affects the environment. Selected based on the current state and current policy. Transitions the environment to a new state.

### Reward
A scalar feedback signal from the environment:
- **Positive** → reinforces the action
- **Negative (penalty)** → discourages the action
- **Goal:** maximize cumulative reward over time, not just immediate reward

### Policy (π)
The agent's strategy — a mapping from states to actions.

| Policy type | Behavior |
|------------|---------|
| **Deterministic** | Always selects the same action in a given state |
| **Stochastic** | Selects actions with certain probabilities |

The optimal policy maximizes expected cumulative reward.

### Value Function
Estimates the **long-term value** of states or actions — predicts expected cumulative reward from a given state/action onward.

| Type | What it estimates |
|------|------------------|
| **State-value V(s)** | Expected cumulative reward starting from state s under a given policy |
| **Action-value Q(s, a)** | Expected cumulative reward from taking action a in state s, then following policy |

Q-values (action-value) are the foundation of Q-learning and deep RL algorithms.

### Discount Factor (γ)

Controls how much the agent values future rewards vs. immediate rewards:

```python
γ = 0   # only immediate reward matters
γ = 1   # all future rewards weighted equally
```

Typical values: 0.9–0.99. Lower γ → myopic agent. Higher γ → far-sighted agent.

**Why discount?** Future rewards are uncertain; discounting reflects this and ensures convergence in many algorithms.

### Episodic vs. Continuous Tasks

| Type | Definition | Example |
|------|-----------|---------|
| **Episodic** | Interaction ends at a terminal state (an "episode") | Maze navigation, game match |
| **Continuous** | No explicit end — runs indefinitely | Robot arm control, traffic management |

## RL vs Supervised and Unsupervised Learning

| | Supervised | Unsupervised | Reinforcement |
|-|-----------|-------------|--------------|
| Data | Labeled | Unlabeled | Interaction-based |
| Feedback | Ground truth labels | None | Rewards/penalties |
| Goal | Predict labels | Discover structure | Maximize cumulative reward |
| When | Static datasets | Pattern discovery | Sequential decision-making |

## Related Notes

- [[ML-Learning-Paradigms]] — RL in context of all three paradigms
- [[Deep-Learning-Fundamentals]] — deep RL (DQN, PPO) combines RL with neural networks for complex state spaces
- [[Supervised-Learning-Algorithms]] — contrast: explicit labels vs reward signals

## Sources / References

- COAE Course Module: Fundamentals of AI — Reinforcement Learning Algorithms

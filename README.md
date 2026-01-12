# Adaptive Liquidity Provision in Uniswap V3 with Deep Reinforcement Learning  
### Replication and Multi Pool Extension

This repository contains a replication and extension of the paper  
**Adaptive Liquidity Provision in Uniswap V3 with Deep Reinforcement Learning**  
by Zhang, Chen, and Yang (2023).

The project was completed as part of an interview assignment for the  
**Quantum Geometric Intelligence (QGI) Lab**.

---

## Project Overview

Uniswap V3 allows liquidity providers to concentrate liquidity within custom price ranges.  
While this improves capital efficiency, it introduces new risks such as frequent rebalancing, gas costs, and impermanent loss.

The original paper proposes a Deep Reinforcement Learning (DRL) agent that dynamically selects liquidity ranges to improve performance over static strategies.

This project:

- Replicates the main DRL framework proposed by Zhang et al.
- Uses a Dueling Double DQN agent to control liquidity rebalancing
- Extends the original setup by introducing a **multi pool allocation agent**
- Compares performance across five strategies using cumulative normalized PnL

All experiments are conducted using **synthetic data**, generated to simulate Uniswap-like market behavior.

---

## Key Contributions

1. **Replication of the original DRL model**
   - Dueling Double DQN
   - Discrete action space for tick width selection
   - Reward based on fees, gas costs, and impermanent loss

2. **Multi Pool Extension (DRL-MP)**
   - Agent allocates liquidity across multiple pools simultaneously
   - Pools used: ETH/USDC, BTC/USDC, LINK/USDC
   - Action space extended to select both pool and tick width
   - State includes features from all pools and total capital information

3. **Comprehensive comparison**
   - DRL (single pool)
   - DRL Multi Pool (proposed extension)
   - Static liquidity provision
   - Periodic rebalancing strategy
   - Threshold based rebalancing strategy

---

## Data Description

- Data is **synthetically generated** using Geometric Brownian Motion (GBM)
- Hourly price series for each pool
- Pool specific drift and volatility parameters
- Synthetic volume, liquidity, and tick values
- Total simulated horizon: 3000 hours
  - Training: 2400 hours
  - Validation: 300 hours
  - Testing: 300 hours

Synthetic data is used due to limited availability of clean historical Uniswap V3 pool data.

---

## Feature Engineering

For each pool, the agent observes:

- Log returns (1h, 6h, 24h)
- Rolling volatility (6h, 24h)
- Volume ratio relative to 24h moving average
- Price momentum over 12h
- Position features:
  - In range indicator
  - Normalized range width
  - Log portfolio value ratio
  - In range hit ratio

All features are normalized and clipped for stable training.

---

## Reinforcement Learning Model

- **Algorithm:** Dueling Double DQN
- **Network:** Two fully connected hidden layers
- **Optimizer:** Adam
- **Replay Buffer:** 100,000 transitions
- **Discount Factor:** 0.9
- **Exploration:** Epsilon-greedy with decay
- **Target Network:** Soft updates

The same architecture is used for both the single pool and multi pool agents.

---

## Environment Design

### Simplified Uniswap V3 Position
- Approximate liquidity model
- Fee accrual proportional to volume
- Impermanent loss estimated using AMM-style formulas
- Fixed gas cost per rebalance

This is a **simplified proxy**, not a full implementation of Uniswap V3 contract math.

### Multi Pool Environment
- One position per pool
- Capital initially split evenly across pools
- Rewards aggregated across all pools
- Portfolio value tracked over time

---

## Evaluation Metric

Performance is evaluated using cumulative normalized PnL:

Cumulative Normalized PnL = (V_t - V_0) / V_0

where:
- V_t is the total portfolio value at time t
- V_0 is the initial portfolio value

Results are reported for three initial capital values:
- 250 USD
- 500 USD
- 1000 USD

---

## Main Results

- Both DRL models outperform static and rule-based baselines
- The **DRL Multi Pool agent** achieves higher or comparable returns
- Multi pool allocation reduces normalized impermanent loss
- Performance gains are more pronounced for smaller capital sizes
- Results are consistent across multiple synthetic runs



---

## References

- Zhang, H., Chen, X., and Yang, L. F.  
  *Adaptive Liquidity Provision in Uniswap V3 with Deep Reinforcement Learning*  
  https://arxiv.org/pdf/2309.10129

---

## License

This project is provided for academic and research purposes only.

# Cliff Walking: SARSA vs. Q-Learning

This project implements and compares two fundamental Temporal Difference (TD) learning algorithms: **SARSA** (On-Policy) and **Q-Learning** (Off-Policy) using a 4x12 grid environment.

---

## 1. Algorithm Comparison

The primary difference lies in how the $Q$-values (the agent's memory of "good" moves) are updated.

| Feature | **SARSA** | **Q-Learning** |
| :--- | :--- | :--- |
| **Full Name** | State-Action-Reward-State-Action | (Quality Learning TD Control) |
| **Policy Type** | **On-Policy**: Learns from the *actual* next action taken. | **Off-Policy**: Learns from the *best possible* next action. |
| **Update Rule** | $$Q(s, a) \leftarrow Q(s, a) + \alpha [R + \gamma Q(s', a') - Q(s, a)]$$|$$Q(s, a) \leftarrow Q(s, a) + \alpha [R + \gamma \max_{a'} Q(s', a') - Q(s, a)]$$ |
| **Behavior** | **Conservative**: Steers clear of the cliff to avoid accidental falls. | **Aggressive**: Takes the risky shortest path along the cliff edge. |
| **Best Used For** | Real-world systems where mistakes are costly (e.g., Robotics). | Simulations where finding the absolute shortest path is the priority. |



---

## 2. Alpha ($\alpha$) and Gamma ($\gamma$) Relationship

These parameters act as the "knobs" that control the speed and depth of the agent's learning:


- **Alpha ($\alpha$) – Learning Rate:** Controls how much new information updates previously learned values.
  - **High $\alpha$:** Learns quickly but may be unstable or fail to converge.
  - **Low $\alpha$:** Learns more slowly but converges more steadily and reliably.

- **Gamma ($\gamma$) – Discount Factor:** Determines how much future rewards are valued compared to immediate rewards.
  - **High $\gamma$ (close to 1.0):** Emphasizes long-term rewards (e.g., reaching the goal state).
  - **Low $\gamma$ (close to 0):** Focuses mainly on immediate rewards with little emphasis on future outcomes.


**Convergence:** Both algorithms are designed to converge. **Q-Learning** converges to the mathematically optimal policy. **SARSA** converges to a "safe" policy. If the exploration rate ($\epsilon$) is gradually reduced to zero, both will eventually converge to the same optimal path.

---

## 3. Code Explaination

### A. The Environment (`CliffWalkingEnv`)
* **Grid Size:** 4 rows by 12 columns.
* **Rewards:** -1 for every step; -100 for falling off the cliff (bottom row, columns 1–10).
* **Termination:** Episode ends upon reaching the Goal $(3, 11)$ or falling off the Cliff.

### B. Epsilon-Greedy Strategy
To balance **Exploration** and **Exploitation**:
* **$\epsilon$ (Epsilon):** Set to 0.1 (10% random moves).
* **Greedy:** 90% of moves use the highest $Q$-value.

### C. Path Visualization Symbols
* `S`: Start position
* `G`: Goal position
* `C`: Cliff (Danger zone)
* `*`: Final path taken by the trained agent.

---

## 4. Expected Results
1.  **Reward Graph:** SARSA's reward curve is generally "higher" (less negative) during training because it prioritizes safety.
2.  **Pathing:** * **SARSA** walks "the long way around" (up toward the top of the grid) to keep a buffer from the cliff.
    * **Q-Learning** walks directly along the edge of the cliff to minimize steps.

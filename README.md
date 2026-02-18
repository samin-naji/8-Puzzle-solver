## 1. Comparison of Search Methods and Reinforcement Learning Methods

## 1.1 Introduction

The Eight-Puzzle problem can be solved using both **classical search algorithms** and **reinforcement learning (RL) methods**. In this project, we implemented and evaluated three approaches:

* **A*** search with two heuristics (Misplaced Tiles and Manhattan Distance)
* **Tabular Q-Learning**
* **Deep Q-Network (DQN)**

This section compares these methods in terms of their assumptions, computational cost, and solution quality, and analyzes their suitability for this problem.

---

## 1.2 Example Paths: A* vs Q-Learning Policy

To ensure a fair comparison, all methods were tested on the same initial state:

```
Initial State: (1, 3, 6,
                5, 0, 2,
                4, 7, 8)
```

### **A* Solution Path (h1 and h2)**

Both heuristics produced the same optimal path:

```
R U L D L D R R
```

* Path length: 8
* Optimality: Guaranteed optimal

### **Q-Learning Solution Path**

The learned Q-Learning policy produced the following example path:

```
R U L D L D R R
```

* Path length: 8
* Matches the optimal A* solution

This shows that Q-Learning was able to learn an optimal policy for this specific problem instance.

---

## 1.3 Quantitative Comparison of Results

### **Performance Metrics**

| Method                  | Path Length | Expanded Nodes | Execution Time | Success Rate |
| ----------------------- | ----------- | -------------- | -------------- | ------------ |
| A* (Misplaced Tiles)    | 8           | 17             | ~0.000 s       | 100%         |
| A* (Manhattan Distance) | 8           | 13             | ~0.000 s       | 100%         |
| Q-Learning              | 8 (avg)     | 301,388        | 13.50 s        | 100%         |
| DQN                     | 8 (avg)     | 400            | 0.07 s         | 100%         |

---

## 1.4 Comparison of A* with Q-Learning and DQN

### 1. Need for an Environment Model (Transition Model)

* **A*** requires a **complete and accurate transition model**.

  * It must explicitly know how actions change the state.
  * Successor generation is hard-coded.
* **Q-Learning and DQN** do **not require an explicit model**.

  * They learn solely through interaction with the environment.
  * This makes them model-free methods.

**Conclusion:**
A* is model-dependent, while RL methods are model-free.

---

### 2. Computational Cost and Number of Visited States

* **A***:

  * Extremely efficient for this problem.
  * Expanded as few as **13 nodes** using Manhattan distance.
  * Execution time was negligible.
* **Q-Learning**:

  * Required **over 300,000 expanded nodes** during training.
  * Training time was significantly longer (13.5 seconds).
* **DQN**:

  * Required far fewer expanded nodes during evaluation.
  * Training is more expensive than A*, but evaluation is fast.
  * More efficient than tabular Q-Learning due to generalization.

**Conclusion:**
A* is vastly more efficient for small, well-defined problems like the Eight-Puzzle.

---

### 3. Quality of the Obtained Paths (Optimality and Length)

* **A***:

  * Guarantees optimal paths when using admissible heuristics.
  * Always returns the shortest solution.
* **Q-Learning**:

  * Can learn optimal paths, but without guarantees.
  * Convergence depends on exploration and training time.
* **DQN**:

  * Produces near-optimal paths.
  * Does not guarantee optimality due to function approximation.

In this experiment, all methods achieved a **path length of 8**, indicating optimal or near-optimal performance.

---

## 1.5 Advantages and Disadvantages Summary

### **A***

**Advantages**

* Guaranteed optimal solution
* Very low computational cost
* Deterministic and interpretable
* Ideal for small, discrete problems

**Disadvantages**

* Requires a known transition model
* Does not generalize to unseen problems
* Not suitable for large or continuous state spaces

---

### **Q-Learning**

**Advantages**

* Model-free
* Simple to implement
* Learns optimal policies given enough exploration

**Disadvantages**

* Large memory requirements
* Slow convergence
* Poor scalability

---

### **DQN**

**Advantages**

* Generalizes across states
* Scales to larger state spaces
* Efficient inference once trained

**Disadvantages**

* More complex
* Training instability
* No optimality guarantee
* Overkill for small problems

## 2. Comparison of Q-Learning and Deep Q-Network (DQN)

### 2.1 Overview of the Two Approaches

Q-Learning and Deep Q-Networks (DQN) are both value-based reinforcement learning algorithms that aim to learn an optimal action-value function Q(s, a). However, they differ fundamentally in how this function is represented.

* **Q-Learning** uses a **tabular representation**, storing explicit Q-values for each state-action pair.
* **DQN** replaces the Q-table with a **neural network**, which approximates Q(s, a) and generalizes across states.

In this project, both methods were applied to the Eight-Puzzle problem using the same Gym environment and reward structure.

---

### 2.2 Advantages and Disadvantages in This Problem Setting

The Eight-Puzzle has a **relatively small and discrete state space** compared to many modern reinforcement learning problems. This strongly affects the relative performance of the two algorithms.

#### **Q-Learning**

**Advantages**

* Simple and easy to implement
* Guaranteed convergence (under standard assumptions)
* Learns exact Q-values for visited states
* Produces stable and deterministic policies
* No function approximation error

**Disadvantages**

* Requires storing a Q-value for every visited state
* Does not generalize to unseen states
* Becomes inefficient as the state space grows
* Large number of expanded nodes during training

#### **DQN**

**Advantages**

* Can generalize across similar states
* Does not require storing an explicit Q-table
* Scales better to large or continuous state spaces
* Faster inference once trained

**Disadvantages**

* More complex implementation
* Requires careful tuning of hyperparameters
* Training instability due to function approximation
* No guarantee of learning the optimal policy
* Higher computational overhead for small problems

---

### 2.3 Did DQN Show a Significant Advantage Over Q-Learning?

In this particular problem, **DQN did not show a significant advantage over Q-Learning**.

Both methods:

* Achieved a **100% success rate**
* Produced **near-optimal solution paths** (≈ 8 moves)
* Solved the puzzle reliably

However:

* Q-Learning required **significantly more expanded nodes** during training.
* DQN converged to a stable policy more quickly and executed solutions faster during evaluation.

Despite this, the **final policy quality was similar**, and the additional complexity of DQN did not result in substantially better performance.

**Reason:**
The Eight-Puzzle has a **small, fully observable, discrete state space**, which is ideal for tabular methods. In such settings, Q-Learning is sufficient and often preferable.

---

### 2.4 When Would DQN Outperform Q-Learning?

DQN is expected to perform significantly better than Q-Learning in problems with:

* Very **large state spaces**
* **Continuous or high-dimensional observations**
* **Image-based inputs** (e.g., Atari games)
* Environments where **generalization** is required
* Tasks where storing a Q-table is infeasible

Examples include:

* Robotic control
* Autonomous driving
* Video game environments
* Continuous control tasks

In these cases, tabular Q-Learning becomes impractical, while DQN can learn useful representations using neural networks.

---

### 2.5 Final Method Selection

If only **one method** were to be chosen for solving the Eight-Puzzle problem, **Q-Learning** would be the preferred choice.

**Justification:**

* The state space is small and discrete
* Q-Learning is simpler and more transparent
* It provides stable convergence and exact value estimates
* No need for neural networks or replay buffers
* Lower implementation and debugging complexity

DQN, while powerful, introduces unnecessary complexity for this specific task and does not offer a meaningful performance advantage.

---

### 2.6 Summary Table

| Criterion                 | Q-Learning      | DQN                     |
| ------------------------- | --------------- | ----------------------- |
| State space suitability   | Small, discrete | Large, high-dimensional |
| Implementation complexity | Low             | High                    |
| Memory usage              | High (Q-table)  | Low (network weights)   |
| Generalization            | None            | Yes                     |
| Training stability        | High            | Moderate                |
| Optimality guarantee      | Yes (tabular)   | No                      |
| Best choice for 8-Puzzle  | ✅               | ❌                       |

---

### **Conclusion**

For small, deterministic problems like the Eight-Puzzle, tabular Q-Learning remains a strong and reliable choice. DQN becomes advantageous only when the problem scale exceeds the limits of tabular methods.

## 3. Overall Conclusion and Method Selection

If only **one method** had to be chosen to solve the Eight-Puzzle problem, **A*** would be the preferred choice.

### **Reasoning**

* The problem has a **small, discrete, fully known state space**
* An admissible heuristic (Manhattan distance) is available
* A* finds the **optimal solution with minimal computation**
* Reinforcement learning methods require significantly more computation to reach the same result

### **Final Assessment**

* **A*** is the best practical solution for this problem.
* **Q-Learning** demonstrates learning capability but is inefficient.
* **DQN** is powerful but unnecessary for such a small domain.

RL methods become advantageous only when:

* The transition model is unknown
* The state space is very large or continuous
* Generalization across many tasks is required

---

## **Final Remark**

This comparison highlights that the choice of algorithm should be guided by the **problem structure**, not just algorithmic sophistication. While reinforcement learning is powerful, classical search methods remain superior for small, deterministic planning problems such as the Eight-Puzzle.

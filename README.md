# PI_vs_ESL
Python code for a multi-queue, multi-server system with one-period switching delay. Includes an efficient policy-iteration implementation with vectorized mapping, multiprocessing, and sparse CSR/LinearOperator value updates to avoid memory issues. Also ships a 1-server/3-queue policy visualization.

# Multi-Queue Multi-Server Policy Iteration

This repo simulates a multi-queue, multi-server system with one-period switching delay and implements an efficient policy-iteration solver (vectorized mapping, multiprocessing, sparse CSR + LinearOperator to keep memory manageable). A visualization notebook shows the learned policy and a greedy baseline for the 1-server, 3-queue case.

## Setup
- Python 3.10+ recommended.
- Install dependencies: `pip install -r requirements.txt`.
- Open `PI_Modular_parallel.ipynb` (or the modular copies) in Jupyter/Lab/VS Code.

## Model parameters (edit near the top of the notebook)
- `p`: arrival probabilities per queue (each < 1). Adjust to set per-queue arrival rates.
- `R`: number of servers/robots.
- `N`: max queue length (cap); higher values grow the state space.
- `beta`: discount factor.
- `M`: boundary penalty when a queue hits `N`.
All of these live under the `# ---------------- model parameters --------------` section.

## Running policy iteration and simulation
- Run cells in order to build state encodings, admissible actions, transition tables, and then perform policy iteration.
- Simulation section compares the optimal policy vs a greedy longest-queue baseline; arrivals are generated with a fixed RNG seed for reproducibility.
- You can parallelize episodes via the provided `ProcessPoolExecutor` pattern in the notebook.

## Visualization
- The visualization cells plot 2D slices of the learned policy and the greedy policy (for the low-dimensional case), plus a disagreement map to see where they diverge.
- Adjust `pair_configs` or widget controls (if added) to explore different queue pairs.

## Tips
- Larger `Q`, `R`, or `N` can explode the state space; monitor memory when increasing them.
- Keep `beta` in (0,1) for contraction and GMRES stability.
- If using Windows, switch multiprocessing context to `spawn`; on Unix, `fork` is faster.

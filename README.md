# PIKAN vs PINN for Physics-Informed Thermal Modeling (ECE 228, Team 47)

Comparison of an MLP backbone (PINN) and a B-spline Kolmogorov–Arnold Network
backbone (PIKAN) inside an identical physics-informed framework, on three
heat-conduction PDEs. Only the backbone changes; the PDE, the (fixed)
collocation points, the loss, the optimizer, the learning rate, and the epoch
count are held constant, and the trainable-parameter count is matched to within
1.1% (12,737 vs 12,600).

## Problems
- **(a) 1D transient conduction**: `u_t = α u_xx`, α = 0.01, IC `sin(πx)`, zero Dirichlet BC. Closed-form reference `e^{-π²αt} sin(πx)`.
- **(b) 2D steady-state conduction**: `u_xx + u_yy = 0`, BC `u(x,1)=sin(πx)` else 0. Reference `sin(πx) sinh(πy)/sinh(π)`.
- **(c) 2D Gaussian source (manufactured solution)**: `u_xx + u_yy + Q = 0`, zero BC, exact field `sin(πx)sin(πy)·exp(-r²/2σ²)` (σ=0.18), `Q = -(u_xx+u_yy)`.


## Key results (relative L² error, matched ~12.7k params)
| Problem | PINN (MLP) | PIKAN (KAN) | Improvement |
|---|---|---|---|
| (a) 1D transient | 3.89e-3 | 9.28e-5 | 41.9× |
| (b) 2D steady-state | 1.04e-1 | 5.39e-3 | 19.3× |
| (c) 2D Gaussian source | 3.22e-2 | 3.01e-2 | 1.07× |

PIKAN reaches loss `1e-4` in ~310 epochs vs ~1127 for the MLP on (a), at a
4–7× training-time cost. See the report for the full convergence, sensitivity,
and timing analysis.

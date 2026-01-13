# src — Core Algorithm Implementation

> **Note**: Implementation files will be added upon paper acceptance.

This directory will contain the MATLAB implementation of the DMS-SI algorithm.

## Main Files

| File | Description |
|------|-------------|
| `dms_si.m` | Main DMS-SI algorithm entry point |
| `poll.m` | Polling step with positive spanning set |
| `filter_dominated.m` | Pareto dominance filtering |
| `normalize.m` | Coordinate normalization utilities |

## Algorithm Overview

DMS-SI operates in a two-space formulation:

1. **Normalized space** [0,1]^n: All geometric operations (polling, distance computations, direction selection)
2. **Original space** [ℓ,u]: Objective function evaluations via inverse affine map

### Key Components

**Variable Normalization**
```matlab
% Forward: x -> y (original to normalized)
y = (x - lb) ./ (ub - lb);

% Inverse: y -> x (normalized to original)
x = lb + y .* (ub - lb);
```

**Pareto Dominance**

For minimization, point x dominates y (x ≺ y) if:
- F_i(x) ≤ F_i(y) for all i = 1, ..., m
- F_j(x) < F_j(y) for at least one j

The algorithm maintains a list of mutually nondominated points, updated at each iteration using Pareto filtering.

**Polling Step**

Poll directions form a positive spanning set (e.g., D_k = {±e_i}). Trial points y_k + α_k d are evaluated in original coordinates via x = ϕ^{-1}(y).

## Usage

```matlab
% Add to path
addpath('src');

% Basic call (returns Pareto front approximation)
[P, F_vals, output] = dms_si(F, lb, ub);

% With options
opts.alpha0 = 0.25;     % Initial step size
opts.maxevals = 20000;  % Max evaluations
opts.tol = 1e-8;        % Stopping tolerance

[P, F_vals, output] = dms_si(F, lb, ub, opts);
```

## References

- Custódio, A.L., Madeira, J.F.A., Vaz, A.I.F., Vicente, L.N. (2011). Direct Multisearch for Multiobjective Optimization. *SIAM J. Optim.* 21(3), 1109–1140.

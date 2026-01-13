# docs — Documentation

> **Note**: Additional documentation will be added upon paper acceptance.

Additional documentation for DMS-SI.

## Contents

| Document | Description |
|----------|-------------|
| `algorithm_details.md` | Detailed algorithm description |
| `convergence_analysis.md` | Summary of convergence guarantees |
| `api_reference.md` | Function API reference |
| `metrics.md` | Performance metrics definitions |

## Quick Reference

### Main Function

```matlab
[P, F_vals, output] = dms_si(F, lb, ub, options)
```

**Inputs:**
- `F` — Objective function handle `F(x)` returning m×1 vector
- `lb` — Lower bounds (n×1 vector)
- `ub` — Upper bounds (n×1 vector)
- `options` — (Optional) Structure with algorithm parameters

**Outputs:**
- `P` — Pareto front approximation (n×k matrix, k nondominated points)
- `F_vals` — Objective values at Pareto points (m×k matrix)
- `output` — Structure with optimization history

### Options Structure

```matlab
options.alpha0    = 0.25;   % Initial step size (normalized space)
options.beta      = 0.5;    % Step contraction factor
options.gamma     = 2.0;    % Step expansion factor
options.tol       = 1e-8;   % Stopping tolerance
options.maxevals  = 20000;  % Maximum evaluations
options.verbose   = false;  % Display progress
```

### Performance Metrics

| Metric | Description | Better |
|--------|-------------|--------|
| Hypervolume | Volume dominated by Pareto front | Higher |
| Purity | Fraction of nondominated points | Higher |
| Spread (Δ) | Distribution uniformity | Lower |
| IGD | Distance to reference front | Lower |

## Related Resources

- **Paper**: Madeira (2025), "DMS-SI: Direct Multisearch for Multiobjective Optimization — A Scale-Invariant Approach" (Submitted)
- **Original DMS**: [https://www.mat.uc.pt/dms](https://www.mat.uc.pt/dms)
- **Benchmark Suite**: [https://github.com/aguilarmadeira/MOO_Prob_Matlab](https://github.com/aguilarmadeira/MOO_Prob_Matlab)

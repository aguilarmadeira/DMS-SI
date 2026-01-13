# experiments — Reproducibility Scripts

> **Note**: Implementation files will be added upon paper acceptance.

This directory will contain scripts to reproduce the numerical experiments from the DMS-SI paper.

## Files

| File | Description |
|------|-------------|
| `run_experiments.m` | Main script to run all experiments |
| `run_single_strategy.m` | Run experiments for a single scaling strategy |
| `generate_figures.m` | Generate all figures from the paper |
| `compute_hypervolume.m` | Compute Hypervolume indicator |
| `compute_purity.m` | Compute Purity metric |
| `compute_spread.m` | Compute Spread (Δ) metric |
| `compute_igd.m` | Compute Inverted Generational Distance |
| `performance_profiles.m` | Generate performance profiles |

## Prerequisites

1. **DMS-SI code**: Ensure `src/` is in your MATLAB path
2. **Benchmark problems**: Download from [MOO_Prob_Matlab](https://github.com/aguilarmadeira/MOO_Prob_Matlab)
3. **Original DMS** (for comparison): Available at https://www.mat.uc.pt/dms

## Running Experiments

### Full Experiment Suite

```matlab
% Add paths
addpath('../src');
addpath('../scaling');

% Run all experiments (may take several hours)
run_experiments;
```

### Single Scaling Strategy

```matlab
% Run only baseline experiments
run_single_strategy('baseline');

% Run extreme scaling experiments
run_single_strategy('extreme');
```

### Generate Figures

```matlab
% After running experiments, generate paper figures
generate_figures;
% Figures saved to ../figures/
```

## Experimental Setup

As described in the paper:

- **Test set**: 108 multiobjective problems (n = 1 to 30, m = 2 to 5)
- **Scaling strategies**: 7 (baseline + 6 heterogeneous)
- **Stopping criteria**: α < 10^{-8} OR 20,000 evaluations
- **Performance metrics**: Hypervolume, Purity, Spread, IGD
- **Comparisons**: C1 (β=2/3, γ=3/2) and C2 (β=0.5, γ=2.0)

## Output Files

Results are saved to:
- `results/` — Raw experimental data (MAT files)
- `../figures/` — Generated figures (PDF/PNG)

## Expected Results

The experiments should reproduce Tables 4.1 and 4.2 from the paper. Under Comparison 1 (identical parameters β=2/3, γ=3/2):

| Strategy | Contrast | DMS ρ(1.001) | DMS-SI ρ(1.001) |
|----------|----------|--------------|-----------------|
| baseline | 1:1 | 0.72 | 0.73 |
| progressive | 10^6 | 0.62 | 0.77 |
| extreme | 10^8 | 0.57 | 0.71 |
| spatial_thermal | ~9×10^4 | 0.61 | 0.71 |
| sobol_oscillatory | 10^6 | 0.52 | 0.81 |
| sobol_digit_oscillatory | 10^6 | 0.63 | 0.79 |
| halton_oscillatory | 10^6 | 0.49 | 0.84 |

## Notes

- Experiments are deterministic (fixed initialization)
- Total runtime: approximately 8-16 hours on a modern desktop
- Memory usage: < 8 GB RAM

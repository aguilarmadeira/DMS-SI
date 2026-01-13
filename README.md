# DMS-SI

**Direct Multisearch for Multiobjective Optimization: A Scale-Invariant Approach**

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE.md)
[![DOI](https://img.shields.io/badge/DOI-pending-orange.svg)]()

> **Note**: Implementation files will be added upon paper acceptance. The repository structure and documentation are provided for reference.

---

DMS-SI is a scale-invariant reformulation of the [Direct Multisearch (DMS)](https://doi.org/10.1137/100798217) framework for derivative-free multiobjective optimization.

## Key Innovation

DMS-SI addresses the sensitivity of DMS to heterogeneous variable scales through a **two-space formulation**:

- **Geometric operations** (polling, distances, direction selection) are performed in **normalized coordinates** [0,1]^n
- **Objective evaluation** is carried out in the **original variables** via an inverse affine map

This ensures that step sizes have a consistent, dimensionless interpretation independent of physical units and variable ranges. A single algorithmic parameter α_k generates coordinate-specific displacements Δx_i = α_k(u_i − ℓ_i)d_{k,i}, automatically proportional to each variable's range.

## Features

- Scale-invariant geometry for robust performance under heterogeneous variable scales
- Maintains list of nondominated points using Pareto dominance
- Preserves all convergence guarantees of the original DMS framework (Pareto-Clarke stationarity)
- Validated on 108 multiobjective benchmark problems under 7 scaling regimes (contrast up to 10^8)

## Repository Structure

```
DMS-SI/
├── README.md               # This file
├── LICENSE.md              # MIT License
├── CITATION.cff            # Citation metadata
├── src/                    # Core algorithm implementation
│   ├── dms_si.m            # Main DMS-SI algorithm
│   ├── poll.m              # Polling step with positive basis
│   ├── filter_dominated.m  # Pareto dominance filtering
│   └── ...
├── scaling/                # Scaling transformation utilities
│   ├── generate_scale_factors.m
│   ├── generate_scaled_bounds.m
│   └── create_scaled_wrapper.m
├── experiments/            # Scripts to reproduce paper results
│   ├── run_experiments.m
│   └── generate_figures.m
├── figures/                # Figures from the paper
└── docs/                   # Additional documentation
```

## Quick Start

### Basic Usage

```matlab
% Add DMS-SI to path
addpath('src');

% Define your multiobjective problem
F = @(x) [sum(x.^2); sum((x-1).^2)];  % Objective functions (m=2)
lb = [-5; -5];                         % Lower bounds
ub = [5; 5];                           % Upper bounds

% Run DMS-SI
[P_approx, F_approx, output] = dms_si(F, lb, ub);

fprintf('Found %d nondominated points\n', size(P_approx, 2));
```

### With Heterogeneous Scaling

```matlab
% Add scaling utilities
addpath('scaling');

% Original problem
F_orig = @zdt1;
lb_orig = zeros(30, 1);
ub_orig = ones(30, 1);

% Generate scale factors (e.g., extreme scaling with kappa = 1e8)
n = 30;
s = generate_scale_factors(n, 'extreme');

% Create scaled bounds
[lb_scaled, ub_scaled] = generate_scaled_bounds(lb_orig, ub_orig, s);

% Create wrapper that preserves Pareto front structure
F_scaled = create_scaled_wrapper(F_orig, lb_orig, ub_orig, lb_scaled, ub_scaled);

% Run DMS-SI (automatically handles normalized geometry)
[P_approx, F_approx] = dms_si(F_scaled, lb_scaled, ub_scaled);
```

## Algorithm Parameters

| Parameter | Default | Description |
|-----------|---------|-------------|
| `alpha0`  | 0.25    | Initial step size in normalized space |
| `beta`    | 0.5     | Step-size contraction factor |
| `gamma`   | 2.0     | Step-size expansion factor |
| `tol`     | 1e-8    | Step-size stopping tolerance |
| `maxevals`| 20000   | Maximum function evaluations |

## Benchmark Suite

The 108 multiobjective benchmark problems used in the paper are available in a separate repository:

👉 [MOO_Prob_Matlab](https://github.com/aguilarmadeira/MOO_Prob_Matlab)

This includes:
- Problems from ZDT, DTLZ, WFG, MOP, Lovison, Jin families
- Dimensions n = 1 to 30, objectives m = 2 to 5
- MATLAB implementations with bound specifications

## Numerical Results

DMS-SI maintains robust performance across all scaling regimes, while DMS degrades under scale heterogeneity. Hypervolume performance profiles (ρ(1.001)) with identical parameters (β = 2/3, γ = 3/2):

| Scaling Strategy | Contrast (κ) | DMS | DMS-SI |
|------------------|--------------|-----|--------|
| Baseline         | 1            | 0.72 | 0.73  |
| Progressive      | 10^6         | 0.62 | 0.77  |
| Extreme          | 10^8         | 0.57 | 0.71  |
| Spatial-thermal  | ~9×10^4      | 0.61 | 0.71  |
| Halton oscillatory | 10^6       | 0.49 | 0.84  |

## Citation

If you use DMS-SI in your research, please cite:

```bibtex
@article{madeira2025dms_si,
  title={{DMS-SI}: Direct Multisearch for Multiobjective Optimization 
         -- A Scale-Invariant Approach},
  author={Madeira, Jos{\'e} F. A.},
  year={2025},
  note={Submitted}
}
```

For the original DMS framework:

```bibtex
@article{custodio2011dms,
  title={Direct Multisearch for Multiobjective Optimization},
  author={Cust{\'o}dio, A. L. and Madeira, J. F. A. and Vaz, A. I. F. and Vicente, L. N.},
  journal={SIAM Journal on Optimization},
  volume={21},
  number={3},
  pages={1109--1140},
  year={2011},
  doi={10.1137/100798217}
}
```

## Related Work

- **DMS** (original): https://www.mat.uc.pt/dms
- **MOO_Prob_Matlab**: https://github.com/aguilarmadeira/MOO_Prob_Matlab
- **GLODS-SI** (scale-invariant global optimization): https://github.com/aguilarmadeira/GLODS_SI

## License

This project is licensed under the MIT License — see [LICENSE.md](LICENSE.md) for details.

## Author

**José F. A. Madeira**  
IDMEC, Instituto Superior Técnico, Universidade de Lisboa  
ISEL, Instituto Politécnico de Lisboa  
Email: aguilarmadeira@tecnico.ulisboa.pt

## Acknowledgments

This work was supported by Fundação para a Ciência e a Tecnologia (FCT) through LAETA (project [DOI: 10.54499/UID/50022/2025](https://doi.org/10.54499/UID/50022/2025)).

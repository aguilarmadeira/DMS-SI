# scaling — Scaling Transformation Utilities

> **Note**: Implementation files will be added upon paper acceptance.

This directory will contain utilities for applying deterministic scaling transformations to optimization problems.

## Files

| File | Description |
|------|-------------|
| `generate_scale_factors.m` | Generate per-variable scale factors for a given strategy |
| `generate_scaled_bounds.m` | Compute scaled bounds from original bounds and scale factors |
| `create_scaled_wrapper.m` | Create objective function wrapper that preserves Pareto front structure |

## Scaling Strategies

Seven deterministic scaling strategies are implemented:

| Strategy | Contrast (κ) | Description |
|----------|--------------|-------------|
| `baseline` | 1 | No scaling (identity) |
| `progressive` | 10^6 | Cyclic geometric pattern |
| `extreme` | 10^8 | Binary partition (half small, half large) |
| `sobol_oscillatory` | 10^6 | Sobol-sequence driven continuous pattern |
| `sobol_digit_oscillatory` | 10^6 | Sobol digit-based binary assignment |
| `halton_oscillatory` | 10^6 | Halton-sequence driven pattern |
| `spatial_thermal` | ~9×10^4 | Multiphysics-inspired (spatial + thermal) |

## Usage

### Generate Scale Factors

```matlab
n = 10;  % Problem dimension

% Baseline (no scaling)
s = generate_scale_factors(n, 'baseline');

% Extreme scaling (kappa = 1e8)
s = generate_scale_factors(n, 'extreme');

% Progressive scaling (kappa = 1e6)
s = generate_scale_factors(n, 'progressive');
```

### Apply Scaling to Bounds

```matlab
lb_orig = zeros(n, 1);
ub_orig = ones(n, 1);

s = generate_scale_factors(n, 'extreme');
[lb_scaled, ub_scaled] = generate_scaled_bounds(lb_orig, ub_orig, s);
```

### Create Problem Wrapper

```matlab
% Original multiobjective problem
F_orig = @zdt1;
lb_orig = zeros(30, 1);
ub_orig = ones(30, 1);

% Scaling
s = generate_scale_factors(30, 'extreme');
[lb_s, ub_s] = generate_scaled_bounds(lb_orig, ub_orig, s);

% Create wrapper (preserves Pareto front structure)
F_wrapper = create_scaled_wrapper(F_orig, lb_orig, ub_orig, lb_s, ub_s);

% Now use F_wrapper with scaled bounds
x_scaled = rand(30,1) .* (ub_s - lb_s) + lb_s;
f_val = F_wrapper(x_scaled);  % Evaluates at corresponding original point
```

## Coordinate Mapping

The wrapper preserves problem structure through relative coordinate mapping:

```
t = (x_scaled - lb_scaled) ./ (ub_scaled - lb_scaled)   % Relative position [0,1]
x_orig = lb_orig + t .* (ub_orig - lb_orig)             % Map to original space
F = F_original(x_orig)                                   % Evaluate objectives
```

This ensures:
- Pareto front geometry is preserved in objective space
- Global and local Pareto optima maintain their relative positions
- Only the geometric scaling of the search space is modified

## References

See Appendix C of the DMS-SI paper for full mathematical details.

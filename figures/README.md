# figures — Paper Figures

> **Note**: Figures will be added upon paper acceptance.

This directory will contain the figures from the DMS-SI paper.

## Figure List

| Figure | File | Description |
|--------|------|-------------|
| Fig. 1.1 | `scale_heterogeneity.pdf` | Effect of scale heterogeneity on search geometry |
| Fig. 1.2 | `poll_step_comparison.pdf` | Scale-proportional poll steps: DMS vs DMS-SI |
| Fig. 4.1a | `C1_baseline_hv.pdf` | Hypervolume profile: baseline (C1) |
| Fig. 4.1b | `C1_spatial_thermal_hv.pdf` | Hypervolume profile: spatial-thermal (C1) |
| Fig. 4.2a | `C2_baseline_hv.pdf` | Hypervolume profile: baseline (C2) |
| Fig. 4.2b | `C2_spatial_thermal_hv.pdf` | Hypervolume profile: spatial-thermal (C2) |

## Regenerating Figures

To regenerate these figures from experimental data:

```matlab
cd('../experiments');
generate_figures;
```

Figures will be saved in both PDF and PNG formats.

## Figure Format

- Vector format: PDF (for paper submission)
- Raster format: PNG at 300 DPI (for presentations)
- Color scheme: Blue solid (DMS), Red dashed (DMS-SI)

## Usage in Publications

These figures are provided under the MIT license. When using in publications, please cite the DMS-SI paper.

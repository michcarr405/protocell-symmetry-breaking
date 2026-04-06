GitHub-ready code package for the BioSystems manuscript:
**"Variance-dependent inheritance fidelity drives residence-time symmetry breaking in a protocell model."**

This repository contains the paired-noise simulation and analysis code used for the manuscript's
implementation and reproducibility section. All simulations and analyses are implemented in Python
using NumPy and SciPy.

## What is included

- **Model A**: Moran birth-death process with variance-dependent reproduction rate.
- **Model B**: Threshold-triggered division with variance-dependent partition noise.
- **Paired-noise implementations** for exact neutral-vs-coupled comparisons.
- **Automatic neutral-equivalence sanity checks** at zero coupling.
- **Neutral-quantile threshold registration** after burn-in.
- **Grid-based statistical summaries** across scanned parameter grids.
- **Figure-generation routines** for the heatmaps and example plots used in the manuscript.
- **Archived outputs** for the primary manuscript figures and statistics.

## Repository layout

```text
.
├── src/                 # shared statistics and plotting helpers
├── scripts/             # executable sweep scripts for Model A and Model B
├── tests/               # minimal import smoke test
├── outputs/
│   ├── paper/           # archived manuscript-aligned outputs (primary 0.99 threshold)
│   └── robustness/      # archived outputs for 0.95, 0.975, and 0.99 threshold analyses
├── requirements.txt
├── CITATION.cff
└── LICENSE
```

## Installation

```bash
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
```

## Reproduce the main paper sweeps

Run the primary paper-mode sweeps:

```bash
python scripts/model_a_moran_sweep.py --mode paper --out outputs/recomputed/model_a
python scripts/model_b_threshold_division_sweep.py --mode paper --out outputs/recomputed/model_b
```

Or run both with:

```bash
bash scripts/run_all.sh
```

## Parameter settings used in the manuscript

### Shared settings

- `seed = 1`
- `n_cells = 200`
- `n_ticks = 5000`
- `n_reps = 30`
- `burnin_frac = 0.2`
- `theta_mode = neutral_quantile`
- `theta_quantiles = 0.95, 0.975, 0.99`
- `sigma_homo = 1.0`
- `sigma_hetero = 3.0`

### Model A sweep

- `betas = [0.0, 0.002, 0.005, 0.01, 0.02]`
- `etas = [0.005, 0.01, 0.02, 0.05]`

### Model B sweep

- `alphas = [0.0, 0.5, 1.0, 2.0, 3.0]`
- `eta0s = [0.005, 0.01, 0.02, 0.05]`
- `s_thresh = 1.0`
- `g0 = 0.02`
- `sigma_g = 0.03`

## Statistical outputs

Each sweep writes matrices and summary statistics including:

- mean paired tail-occupancy difference (`delta_rho` / `mean_diff`)
- median paired difference
- two-sided paired Wilcoxon signed-rank p-values
- Benjamini-Hochberg FDR-adjusted q-values across the full grid
- paired Wilcoxon rank-biserial effect size
- bootstrap 95% confidence intervals for the median paired difference

## Archived outputs included here

The `outputs/paper/` directory contains the manuscript-aligned archived figures and summary files for
the primary `0.99` neutral-quantile threshold analysis. The `outputs/robustness/` directory contains
archived outputs for the additional `0.95`, `0.975`, and `0.99` threshold analyses.

## Notes

- The scripts use a strict common-random-number design, so at zero coupling (`beta = 0` for Model A;
  `alpha = 0` for Model B) the neutral and variance-dependent trajectories are identical to machine precision.
- The plotting and statistics utilities are intentionally lightweight and kept in `src/` rather than packaged
  as a formal installable module.

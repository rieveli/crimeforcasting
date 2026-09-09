# crimeforcasting
Integrating Harm-Based Severity Scoring into Crime Time-Series Forecasting : Crime Prediction ( How many? )
# Integrating Harm-Based Severity Scoring into Crime Time-Series Forecasting

[![IEEE](https://img.shields.io/badge/IEEE-PacificVis%202026-00629B)](https://ieeexplore.ieee.org/abstract/document/11558807/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

Official code repository for the paper accepted at **IEEE PacificVis 2026**.

> Crime analysis and visualisation systems predominantly rely on incident counts, despite extensive evidence that criminal offences vary substantially in social harm. This project integrates crime severity modelling into visual analytics and forecasting workflows by constructing an incident-level **Crime Severity Score (CSS)** for Cambridge, Massachusetts, using FBI NIBRS offence labels, U.S.-adapted imprisonment-day harm weights, and proportionality adjustments inspired by the UK ONS Crime Severity Score.

📄 **Paper:** [IEEE Xplore](https://ieeexplore.ieee.org/abstract/document/11558807/)

## Authors

- **Veli Oz** ¹ ³ — veli.oz@students.cdu.edu.au
- **Reem E. Mohamed** ¹ ³ — reem.sherif@cdu.edu.au
- **Arman Far** ² — arman.far.far@gmail.com
- **Asif Karim** ¹ — asif.karim@cdu.edu.au
- **Sami Azam** ¹ — sami.azam@cdu.edu.au

¹ Energy and Resources Institute, Faculty of Science and Technology, Charles Darwin University, Australia
² Crown Institute of Higher Education (CIHE Australia), Australia
³ Veli Oz and Reem E. Mohamed contributed equally to this work.

## Overview

Severity is treated as a first-class analytic variable rather than a simple incident count. The pipeline:

1. **NIBRS-based offence labelling** — maps free-text offence descriptions to FBI NIBRS categories via rule-based keyword matching.
2. **Imprisonment-day harm weights** — assigns CCHI-inspired harm weights (in days of imprisonment) adapted to U.S. sentencing practice.
3. **ONS-inspired proportionality adjustment** — applies severity multipliers so that violent offences are correctly ranked above high-frequency, low-harm property crimes.
4. **Normalisation** — log, square-root, and Box–Cox transformations followed by min–max scaling, producing 8 CSS variants (`FBI_RAW`, `FBI_LOG`, `FBI_SQRT`, `FBI_BC`, `FBI_ONS`, `FBI_ONS_LOG`, `FBI_ONS_SQRT`, `FBI_ONS_BC`).
5. **Severity-aware forecasting** — a two-stage architecture that predicts incident-level severity first, then uses aggregated predicted severity as a feature for daily/weekly crime-count forecasting with XGBoost, LightGBM, and CatBoost.

## Key Findings

- **Box–Cox transformations** (λ ≈ 0.32–0.33) achieve the best distributional and forecasting properties, preserving harm-based rank order while minimising skew.
- **Daily forecasting:** temporal features drive ~86% of predictive improvement; severity features add a further ~14%, giving a 6.9% MAE reduction and +28.9% R² over volume-only baselines.
- **Weekly forecasting:** severity features play a stabilising role, recovering 5.3% of R² lost to temporal overfitting and contributing 54% of the total MAE improvement.
- **CatBoost outperforms univariate/multivariate LSTM baselines** on this structured time-series task at this scale.
- Severity-weighted classification of offence categories reaches **99.22% accuracy** vs. 97.85% for a count-based baseline.

## Dataset

Incident-level crime data for the City of Cambridge, Massachusetts (2009–2025), published via the Cambridge Police Department's Annual Crime Reports and open data portal (November 2025) — 106,350 incidents.

## Repository Structure

```
.
├── data/            # Raw / processed crime data (see data/README for source & access)
├── notebooks/        # Exploratory analysis and ablation studies
├── src/
│   ├── severity/      # CSS construction: NIBRS labelling, harm weights, ONS multipliers, transforms
│   ├── forecasting/    # Two-stage severity-aware daily/weekly prediction models
│   └── evaluation/     # Metrics, ablation, and figure generation
├── figures/          # Generated plots (histograms, severity rankings, feature importance, etc.)
└── README.md
```

*(Adjust this structure to match what you actually push.)*

## Citation

If you use this code or the CSS methodology, please cite the paper:

```bibtex
@inproceedings{oz2026harmbased,
  title     = {Integrating Harm-Based Severity Scoring into Crime Time-Series Forecasting},
  author    = {Oz, Veli and Mohamed, Reem E. and Far, Arman and Karim, Asif and Azam, Sami},
  booktitle = {IEEE Pacific Visualization Conference (PacificVis)},
  year      = {2026},
  publisher = {IEEE}
}
```

## License

This repository is licensed under the [MIT License](LICENSE). The license covers the code in this repository only; the published paper text and figures remain © IEEE.

## Contact

For questions about the code or paper, reach out to Veli Oz (veli.oz@students.cdu.edu.au) or Reem E. Mohamed (reem.sherif@cdu.edu.au).

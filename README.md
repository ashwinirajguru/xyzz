# Controlled-Release Kinetics

Python tool for fitting and comparing drug-release models for implants and hydrogels.

## Overview

Controlled-release systems deliver a drug at a predictable rate over hours to months. This project fits cumulative release data (Mt/M∞ vs. time) to classic kinetic models, compares how well each fits, and identifies the likely release mechanism.

## Models

| Model | Equation | Typical use |
|---|---|---|
| Zero-order | F = k₀t | Reservoir implants, osmotic pumps |
| First-order | F = 1 − e^(−k₁t) | Water-soluble drugs in porous matrices |
| Higuchi | F = k_H√t | Diffusion from a planar matrix |
| Korsmeyer-Peppas | F = ktⁿ | Hydrogels and polymers |

For Korsmeyer-Peppas (thin film), n ≈ 0.5 indicates Fickian diffusion, 0.5 < n < 1 anomalous transport (diffusion plus swelling/relaxation), and n ≈ 1 Case-II transport.

## Features

- Nonlinear least-squares fitting with `scipy.optimize.curve_fit`
- Fits only the first 60% of release, where Korsmeyer-Peppas is valid
- Reports parameters, R², and time to 50% release (t₅₀) for each model
- Interprets the release mechanism from the exponent n
- Saves a model-comparison plot (`release_kinetics.png`)

## Installation

```bash
git clone https://github.com/<your-username>/controlled-release-kinetics.git
cd controlled-release-kinetics
pip install numpy scipy matplotlib
```

## Usage

```bash
python controlled_release.py
```

The script runs on synthetic data by default. To use your own measurements, replace `t_data` (hours) and `F_data` (fraction released, 0 to 1) in the script.

## Example output

```
Model             Parameters                       R^2   t50 (h)
----------------------------------------------------------------
Zero-order        0.0396                        0.8500      12.6
First-order       0.0556                        0.9488      12.5
Higuchi           0.1277                        0.9621      15.3
Korsmeyer-Peppas  0.0959, 0.6330                0.9925      13.6

Korsmeyer-Peppas exponent n = 0.63 -> anomalous transport
```

## Limitations

- A high R² does not prove a mechanism, since several models can fit the same data.
- Burst release may need a biphasic model.
- The n cutoffs differ for cylinders and spheres.
- In vitro release often differs from in vivo behavior.

## Roadmap

- [ ] Hixson-Crowell model for erosion-controlled release
- [ ] AIC-based model comparison
- [ ] Numerical solution of Fick's second law
- [ ] Biphasic (burst + sustained) model

## License

MIT

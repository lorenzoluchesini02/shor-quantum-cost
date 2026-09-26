# The Space Cost of Shor's Algorithm

How large must the Quantum Phase Estimation (QPE) register be to reliably factor a number with Shor's algorithm? This project measures it empirically.

Developed as the final project for the **Quantum Computing** course (MSc in AI for Science and Technology — Prof. Prati), A.Y. 2025/26.

## Overview

Shor's algorithm reduces integer factoring to order-finding via QPE. The **target register**, holding the number `N`, needs only `n = ⌈log2 N⌉` qubits — but the **counting register**, which reads the eigenphase `θ = k/r`, must be substantially larger to resolve it with enough precision.

By sweeping the counting register size `t` for `N ∈ {15, 21, 35, 105}` (base `a = 2`, PennyLane `default.qubit`, analytic mode) and measuring:

- **P_resolve(t)** — probability the phase is resolved within `1/(2r²)` of the true `k/r` (a purely spatial metric),
- **P_factor(t)** — probability a single shot yields a non-trivial factor of `N` (includes numerical overhead from `k=0`, odd `r`, `gcd(k,r)>1`),

we find that the smallest reliable counting register is **`t* ≈ 1.5–1.6 n`**, comfortably inside the worst-case bound `t ≤ 2n+1`, and that this phase-precision cost — not the number itself — dominates total circuit width (`≈ 3n` qubits vs. `log2 N` bits of useful information).

Full derivation, methodology and results are in [`report/report.pdf`](report/report.pdf); the original slide deck is in [`report/slides.pdf`](report/slides.pdf) / [`report/slides.pptx`](report/slides.pptx).

## Repository structure

```
notebooks/
└── qpe_register_experiment.ipynb   # circuit implementation + t-sweep experiment
report/
├── report.pdf                      # full write-up
├── slides.pdf
└── slides.pptx
```

The notebook validates the circuit on `N=15` (reproducing the IBM Quantum tutorial), factors `N ∈ {15, 21, 35, 105}`, then runs the space-cost sweep and produces every figure discussed in the report.

## Requirements

```
pennylane
numpy
matplotlib
```

Install with:
```bash
pip install -r requirements.txt
```

## Author

**Lorenzo Luchesini** — MSc student, AI for Science and Technology (UniMiB / UniMi / UniPv)
[GitHub](https://github.com/lorenzoluchesini02)

## License

MIT — see [LICENSE](LICENSE).

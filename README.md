# SimASM Stutter Equivalence Project

A research project demonstrating formal behavioral equivalence between discrete-event simulation (DES) formalisms using stutter equivalence theory and the [SimASM](https://github.com/SimASM-Project) framework.

## Overview

This project validates that **different simulation formalisms produce behaviorally equivalent results** when modeling the same system. It compares two classic DES formalisms:

- **Event Graph** (Schruben, 1983) — models systems as directed graphs of event vertices with scheduling edges
- **Activity Cycle Diagram** (Tocher, 1960; Carrie, 1988) — models systems using passive states (queues) and active states (activities) with token flow

Equivalence is verified using **stutter equivalence** (Mokkedem & Mery, 1997): two execution traces are equivalent if their no-stutter traces (consecutive duplicates removed) are identical over a shared observation level.

## Notebooks

| Notebook | Description |
|---|---|
| `eg_littles_law.ipynb` | M/M/5 queue modeled with Event Graph formalism; verifies Little's Law (L = λW) |
| `acd_littles_law.ipynb` | M/M/5 queue modeled with ACD formalism; verifies Little's Law (L = λW) |
| `mm5_verification.ipynb` | Multi-seed stutter equivalence verification of M/M/5 queue across both formalisms |
| `warehouse_verification.ipynb` | Stutter equivalence verification of a complex 6-station warehouse outbound process |

## Key Results

### M/M/5 Queue

- Single-seed: 332 steps (EG) vs 499 steps (ACD) producing **identical 166 no-stutter states**
- 50-seed verification: W-stutter equivalence confirmed across all seeds

### Warehouse Process

- 6 service stations, 4 input streams, 20 atomic propositions
- Raw steps differ by ~71% (EG: 2,979 vs ACD: 1,740), yet both produce **948 identical no-stutter states**
- Formalism choice affects computational overhead but not observable behavior

## Getting Started

### Prerequisites

- Python 3.12+
- Jupyter Notebook or JupyterLab

### Installation

```bash
pip install -r requirements.txt
```

### Running

```bash
jupyter notebook
```

Open any `.ipynb` file and run cells sequentially. Each notebook follows the workflow:

1. **Define** — JSON specification of the system model
2. **Convert** — `%%simasm convert` generates executable SimASM code
3. **Verify** — `%%simasm verify` runs multi-seed stutter equivalence checks
4. **Visualize** — auto-generated plots of trace comparison, convergence, and stutter distribution

## How It Works

SimASM provides Jupyter magic commands for a complete simulation verification pipeline:

- `%%simasm convert` — converts JSON model specifications into executable SimASM (a DSL for DES)
- `%%simasm experiment` — runs statistical experiments with replications
- `%%simasm verify` — performs bounded multi-seed stutter equivalence verification

The **observation level** W defines the set of atomic propositions (e.g., number of busy servers) used to extract observable behavior from raw execution traces. Two models are W-stutter equivalent if, for every shared random seed, their projected no-stutter traces match.

## License

[MIT](LICENSE)

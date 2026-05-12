# Rethinking GNNs and Missing Features: Challenges, Evaluation and a Robust Solution

This repository contains the official codebase for the paper:

> **Rethinking GNNs and Missing Features: Challenges, Evaluation and a Robust Solution**
> Francesco Ferrini, Veronica Lachi, Antonio Longa, Bruno Lepri, Akiyoshi Matono, Andrea Passerini, Xin Liu, Manfred Jaeger.
> *International Conference on Machine Learning (ICML), 2026.*
> [[arXiv]](https://arxiv.org/abs/2601.04855)

The project provides datasets, models, and evaluation protocols to study the robustness of **Graph Neural Networks (GNNs)** under different missing feature mechanisms, and introduces **GNNmim**, a simple yet effective approach for node classification with incomplete features.

> An earlier short version of this work appeared at a workshop; the code is archived at <https://github.com/YOUR_USERNAME/gnnmim-workshop>. **This repository supersedes that one and contains the full method, datasets, and experiments from the ICML paper.**

---

## Installation

We recommend using a fresh Python ≥ 3.10 environment.

```bash
git clone https://github.com/YOUR_USERNAME/gnnmim.git
cd gnnmim
pip install -r requirements.txt
```

The `baselines/graph-sum-product-networks/` directory is required for the GSPN baseline and is added to `sys.path` automatically by `utils.py`.

---

## Usage

Run experiments with:

```bash
python main.py --dataset <dataset> --models <model(s)> --mechanisms <mechanism(s)>
```

Examples:

```bash
# Synthetic dataset, our method, MCAR missingness
python main.py --dataset synthetic --models gnnmim --mechanisms UMCAR

# Compare several models on PubMed under two missingness mechanisms
python main.py --dataset PubMed --models gnnmim fp pcfi --mechanisms SMCAR CDMNAR
```

### Arguments

#### `--dataset`
Dataset to evaluate. All datasets are stored/loaded from `./data/`.

| Option | Description |
|---|---|
| `synthetic` | Synthetic graph with dense, controlled features |
| `Cora`, `Citeseer`, `PubMed` | Citation networks (downloaded automatically via PyG) |
| `electric` | Electric-network dataset (dense features) |
| `air` | Air-quality dataset (dense features) |
| `tadpole` | Alzheimer's disease progression dataset (dense features) |

#### `--models`
One or more models to evaluate, separated by spaces.

| Option | Description |
|---|---|
| `gnnmim` | **Our method** |
| `gcnmi` | Plain GCN with mean imputation |
| `fairac` | FairAC |
| `gcnmf` | GCNmf |
| `fp` | Feature Propagation |
| `pcfi` | PCFI |
| `gspn` | Graph Sum-Product Networks |
| `goodie` | GOODIE |

#### `--mechanisms`
Missingness mechanisms to simulate, separated by spaces.

| Option | Description |
|---|---|
| `UMCAR` | Uniform Missing Completely At Random |
| `SMCAR` | Structured MCAR |
| `LDMCAR` | Label-Dependent MCAR |
| `FDMNAR` | Feature-Dependent Missing Not At Random |
| `CDMNAR` | Class-Dependent Missing Not At Random |

---

## Repository structure

```
.
├── main.py                  # Entry point for experiments
├── models.py                # Model definitions (GNNmim, GCNmf, FairAC, ...)
├── layers.py                # GNN layers used across models
├── utils.py                 # Data loading, missingness mechanisms, evaluation loops
├── filling_strategies.py    # Imputation strategies
├── embedder.py              # Generic embedder base class
├── GOODIE.py                # GOODIE baseline
├── utils_goodie.py          # GOODIE utilities
├── data/                    # Datasets (.pt files)
└── baselines/               # Third-party baseline code (each with its own LICENSE)
    ├── GCNmf/
    ├── feature-propagation/
    ├── graph-sum-product-networks/
    ├── oldie_but_goodie-main/
    └── pcfi/
```

---

## Citation

If you use this code or the datasets in your research, please cite our paper:

```bibtex
@inproceedings{ferrini2026rethinking,
  title     = {Rethinking {GNN}s and Missing Features: Challenges, Evaluation and a Robust Solution},
  author    = {Ferrini, Francesco and Lachi, Veronica and Longa, Antonio and Lepri, Bruno and Matono, Akiyoshi and Passerini, Andrea and Liu, Xin and Jaeger, Manfred},
  booktitle = {Proceedings of the 43rd International Conference on Machine Learning (ICML)},
  year      = {2026},
  url       = {https://openreview.net/forum?id=E4gftzjqoh}
}
```

---

## Acknowledgements

The `baselines/` directory contains code adapted from the original authors of GCNmf, Feature Propagation, PCFI, GSPN, and GOODIE. Each subdirectory retains its original `LICENSE` file. We thank the authors for releasing their code.

## License

This project is released under the MIT License (see `LICENSE`). Code under `baselines/` is governed by the licenses included in each respective subdirectory.

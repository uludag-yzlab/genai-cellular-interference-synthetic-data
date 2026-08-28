# Generative AI-Based Synthetic Data for Cellular Interference Classes

This repository contains synthetic cellular-network interference data generated for the study:

> **Generative AI-Based Synthetic Data Generation for Interference Classes in Heterogeneous Cellular Networks**

The datasets were created to support interference-classification, class-balancing, and synthetic-data research when real operator data cannot be publicly shared.

## Dataset contents

The repository includes synthetic data for three interference classes:

- `External Interference`
- `Hardware/PIM`
- `Inter-Cell Interference (ICI)`

The class label is stored in the `Interference_Tag` column.

Two data representations are provided:

| Representation | Description | Features | Synthetic rows per class |
|---|---|---:|---:|
| Hourly | Hourly cellular-network KPI observations | 31 | 9,013 |
| Sliding window | Mean and standard-deviation features calculated over an 8-hour window | 62 | 4,775 |

The repository contains six CSV files. Each interference class is published in a separate file for the hourly and sliding-window representations.

```text
data/
├── hourly/
│   ├── forestflow_synthetic_ici_hourly_k200.csv
│   ├── forestflow_synthetic_external_hourly_k200.csv
│   └── forestflow_synthetic_hardware_pim_hourly_k200.csv
└── sliding_window/
    ├── forestflow_synthetic_ici_rolling_k200.csv
    ├── forestflow_synthetic_external_rolling_k200.csv
    └── forestflow_synthetic_hardware_pim_rolling_k200.csv
```

## How the data were generated

The synthetic data were generated using the **ForestFlow configuration of ForestDiffusion**. A separate model was trained for each interference class using only the real training records belonging to that class.

The selected generation settings were:

| Parameter | Value |
|---|---|
| `diffusion_type` | `flow` |
| `n_t` | `20` |
| `duplicate_K` | `200` |
| `seed` | `42` |

Before generation, identifying and operational columns such as cell identifiers and timestamps were removed. Only numeric KPI features were used by the generator. Missing numeric values were filled using the median of the corresponding real training class.

The real data were divided into training and test sets. ForestFlow was fitted only on the training portion, and the test data were reserved for evaluation. Therefore, no test samples were used during synthetic-data generation.

The generated data were evaluated using distributional similarity measures, including KS distance, Jensen–Shannon divergence, Wasserstein distance, and correlation difference. Their downstream utility was also examined with Random Forest, KNN, XGBoost, LightGBM, and MLP classifiers.

The original cellular-network records are not included because they contain commercially confidential operator information. Only synthetic records are published in this repository.

## Intended use

These datasets can be used for:

- cellular-interference classification;
- class-imbalance and data-augmentation experiments;
- synthetic tabular-data evaluation;
- comparison of machine-learning classifiers;
- reproducible research without distributing confidential operator data.

The sliding-window rows are independent synthetic observations and do not form a continuous chronological time series. The data should also not be treated as verified measurements from a live network.

## Citation

If you use these datasets, please cite the accompanying study and this repository. Complete citation information will be added after publication.

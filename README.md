# Time-TK

<div align="center">


## Time-TK: A Multi-Offset Temporal Interaction Framework Combining Transformer and Kolmogorov-Arnold Networks for Time Series Forecasting

**The ACM Web Conference 2026 (WWW '26)**

[Paper]([https://doi.org/10.1145/3774904.3792618](https://dl.acm.org/doi/abs/10.1145/3774904.3792618)) | [Code](https://github.com/Cola-Fsm/Time-TK)

</div>

---

## Introduction

Time-TK is a lightweight time series forecasting framework designed to better capture fine-grained temporal dependencies across different time offsets.

Most existing forecasting methods embed individual time steps, temporal patches, or entire sequences as tokens. These strategies may overlook **multi-offset temporal correlations**, especially when modeling long and non-stationary sequences. To address this issue, Time-TK introduces a new multi-offset modeling perspective and combines efficient temporal interaction with Kolmogorov-Arnold Networks (KANs).

The framework contains three main components:

- **Multi-Offset Token Embedding (MOTE):** constructs multiple offset sub-sequences from the original input to preserve temporal patterns at different offsets.
- **Multi-Offset Interactive KAN (MI-KAN):** uses RBF-based KAN layers to learn expressive representations for different offset sub-sequences.
- **Multi-Offset Temporal Interaction (MOTI):** models dependencies within offset sub-sequences and performs global interaction with the original sequence representation.

Extensive experiments on real-world time series benchmarks demonstrate the effectiveness and efficiency of Time-TK for both long-term and short-term forecasting.

---

## Framework

The overall workflow of Time-TK is:

```text
Historical Time Series
        │
        ▼
Instance Normalization
        │
        ▼
Multi-Offset Token Embedding (MOTE)
        │
        ▼
Multi-Offset Interactive KAN (MI-KAN)
        │
        ▼
Multi-Offset Temporal Interaction (MOTI)
        │
        ▼
Global Interaction
        │
        ▼
Prediction Head
        │
        ▼
Future Time Series
```

### Multi-Offset Token Embedding

Given a historical sequence, MOTE divides it into multiple sub-sequences with different temporal offsets. Instead of representing the sequence using only continuous neighboring points, the model explicitly preserves information from different offset positions.

This design helps Time-TK capture temporal patterns at different granularities and improves the utilization of long historical contexts.

### Multi-Offset Interactive KAN

After multi-offset embedding, MI-KAN learns dedicated representations for each offset sub-sequence.

Time-TK adopts an efficient **RBF-based FastKAN** implementation. Compared with conventional MLP mappings, the KAN-based module provides flexible nonlinear modeling for temporal patterns while keeping the architecture lightweight.

### Multi-Offset Temporal Interaction

MOTI first performs self-attention within each offset representation and then introduces a global interaction mechanism between the multi-offset representation and the original sequence representation.

This enables Time-TK to integrate local offset-specific information with the global temporal context.

---

## Repository Structure

```text
Time-TK/
├── data_provider/          # Data loading and preprocessing
├── exp/                    # Experiment pipeline
├── generated_scripts/      # Experiment scripts
├── layers/                 # Basic network layers and FastKAN
├── models/                 # Time-TK and baseline models
├── utils/                  # Utility functions
├── run.py                  # Main entry point
├── requirements.txt        # Python dependencies
└── README.md
```

---

## Requirements

The main dependencies are:

- Python
- PyTorch
- NumPy
- Pandas
- Matplotlib
- Scikit-learn

Install the required packages with:

```bash
git clone https://github.com/Cola-Fsm/Time-TK.git
cd Time-TK

pip install -r requirements.txt
```

---

## Datasets

Time-TK is evaluated on both long-term and short-term forecasting benchmarks.

| Task                        | Datasets                                                     |
| --------------------------- | ------------------------------------------------------------ |
| Long-term forecasting       | ETTh1, ETTh2, ETTm1, ETTm2, Exchange, Weather, Electricity, Solar-Energy, Traffic |
| Short-term forecasting      | PEMS03, PEMS04, PEMS07, PEMS08                               |
| Web transaction forecasting | BTC/USDT                                                     |

For the main long-term forecasting benchmarks, the lookback length is set to **96**, and the prediction horizons are:

```text
{96, 192, 336, 720}
```

For PEMS datasets, the prediction horizons are:

```text
{12, 24, 48, 96}
```

The public datasets can be obtained from the following sources:

- [ETT Dataset](https://github.com/zhouhaoyi/ETDataset)
- [Exchange Rate Dataset](https://github.com/laiguokun/multivariate-time-series-data)
- [Weather Dataset](https://www.bgc-jena.mpg.de/wetter/)
- [Electricity Dataset](https://archive.ics.uci.edu/ml/datasets/ElectricityLoadDiagrams20112014)
- [Solar Energy Dataset](https://www.nrel.gov/grid/solar-power-data.html)
- [PEMS Dataset](https://pems.dot.ca.gov/)
- [BTC/USDT Dataset](https://www.kaggle.com/datasets/shivaverse/btcusdt-5-minute-ohlc-volume-data-2017-2025)

Please organize the downloaded datasets according to the paths specified in the experiment scripts.

---

## Training and Evaluation

Ready-to-use experiment configurations are provided in:

```text
generated_scripts/
```

You can run the corresponding shell script for a dataset and prediction horizon, for example:

```bash
bash generated_scripts/<script_name>.sh
```

Alternatively, experiments can be launched directly through:

```bash
python -u run.py [arguments]
```

Please refer to `run.py` and the scripts under `generated_scripts/` for the complete argument settings.

---

## Main Results

Time-TK achieves strong forecasting performance across long-term and short-term benchmarks.

The experiments show that:

- MOTE improves the utilization of historical information.
- MI-KAN provides effective nonlinear temporal representation.
- MOTI improves interaction across different temporal offsets.
- MOTE can also be integrated into other forecasting architectures such as iTransformer, PatchTST, and TimesNet.
- Time-TK maintains competitive memory efficiency as the input sequence length increases.

Please refer to the paper for the complete forecasting results, ablation studies, statistical significance tests, and efficiency analysis.

---

## Citation

If you find this repository useful, please cite our paper:

```bibtex
@inproceedings{zhang2026timetk,
  author    = {Fan Zhang and Shiming Fan and Hua Wang},
  title     = {Time-TK: A Multi-Offset Temporal Interaction Framework Combining Transformer and Kolmogorov-Arnold Networks for Time Series Forecasting},
  booktitle = {Proceedings of the ACM Web Conference 2026},
  pages     = {7495--7506},
  year      = {2026},
  doi       = {10.1145/3774904.3792618}
}
```

---

## Contact

For questions about the paper or code, please open an issue in this repository.

---

## Acknowledgements

We appreciate the following resources a lot for their valuable code and datasets:

- Time-Series-Library ([https://github.com/thuml/Time-Series-Library](https://github.com/thuml/Time-Series-Library))
- iTransformer ([https://github.com/thuml/iTransformer](https://github.com/thuml/iTransformer))
- BasicTS ([https://github.com/GestaltCogTeam/BasicTS](https://github.com/GestaltCogTeam/BasicTS))
- TFB ([https://github.com/decisionintelligence/TFB](https://github.com/decisionintelligence/TFB))

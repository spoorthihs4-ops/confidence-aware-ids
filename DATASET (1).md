# Dataset: CIC-IDS2017

This project uses the **CIC-IDS2017** intrusion detection evaluation dataset. The data are **not included in this repository**: they belong to their creators, and the CSV files are far larger than GitHub allows for normal uploads. Download them from the sources below.

## Source

| | |
|---|---|
| Name | CIC-IDS2017 — Intrusion Detection Evaluation Dataset |
| Creator | Canadian Institute for Cybersecurity (CIC), University of New Brunswick, Canada |
| Official page | https://www.unb.ca/cic/datasets/ids-2017.html |
| Version used | *MachineLearningCVE*: flow features extracted with CICFlowMeter, 8 CSV files |
| Copy used in this project | Kaggle mirror: https://www.kaggle.com/datasets/chethuhn/network-intrusion-dataset |
| Terms of use | as stated by the creators on the official page; cite the paper below when using the data |

## What the data contain

The data were captured on an isolated testbed over five working days in July 2017. Benign background traffic was generated from profiles of user behaviour, and a separate attacker network executed the attacks. Each row is one **bidirectional network flow**, described by about 80 statistics (packet counts and sizes, inter-arrival times, TCP flags, header lengths) and a label.

The data contain **only flow statistics**: no packet payloads and no IP addresses. No real users or human participants are involved.

### Files

| File | Capture day |
|---|---|
| `Monday-WorkingHours.pcap_ISCX.csv` | Monday (benign only) |
| `Tuesday-WorkingHours.pcap_ISCX.csv` | Tuesday |
| `Wednesday-workingHours.pcap_ISCX.csv` | Wednesday |
| `Thursday-WorkingHours-Morning-WebAttacks.pcap_ISCX.csv` | Thursday morning |
| `Thursday-WorkingHours-Afternoon-Infilteration.pcap_ISCX.csv` | Thursday afternoon |
| `Friday-WorkingHours-Morning.pcap_ISCX.csv` | Friday morning |
| `Friday-WorkingHours-Afternoon-PortScan.pcap_ISCX.csv` | Friday afternoon |
| `Friday-WorkingHours-Afternoon-DDos.pcap_ISCX.csv` | Friday afternoon |

(File names as distributed, including the original spellings.) Notebook 1 reads every file matching `*pcap_ISCX*.csv`.

### Labels

The 15 original labels are grouped into 9 attack families:

| Family | Original labels |
|---|---|
| Benign | BENIGN |
| DoS | DoS Hulk, DoS GoldenEye, DoS slowloris, DoS Slowhttptest |
| DDoS | DDoS |
| PortScan | PortScan |
| BruteForce | FTP-Patator, SSH-Patator |
| WebAttack | Web Attack – Brute Force, Web Attack – XSS, Web Attack – Sql Injection |
| Bot | Bot |
| Infiltration | Infiltration *(held out as a never-seen attack)* |
| Heartbleed | Heartbleed *(held out as a never-seen attack)* |

## How the project uses the data

| Step | Result |
|---|---|
| Raw data | 2,830,743 flows, 15 labels |
| Exact duplicates removed (before splitting) | 596,747 |
| Identical flows with conflicting labels removed | 544 |
| Impossible negative values (rows dropped) | 150 |
| Clean data | **2,233,302 flows** (−21.1 %) |
| Never-seen families held out | Heartbleed and Infiltration (47 flows) |
| Stratified split (train / calibration / threshold-tuning / test) | 1,339,953 / 223,325 / 223,326 / 446,651 |
| Model features | 59 + 2 missing-value indicators (Destination Port excluded as a lab-specific shortcut) |
| Training subsample | 321,277 flows |

Every statistic learned from the data (imputation, capping, scaling, feature selection) is fitted on the training split only. Full details are in `01_data_foundation.ipynb`.

## How to get the data

**On Kaggle (as in this project):** in a Kaggle notebook, click **+ Add Input**, search for *network intrusion dataset* (`chethuhn/network-intrusion-dataset`) and attach it. Notebook 1 finds the files automatically under `/kaggle/input/`.

**Locally:** download the *MachineLearningCVE* CSV files from the official page or the Kaggle mirror, put them in a folder, and set:

```bash
export IDS_INPUT_ROOT=/path/to/that/folder
```

## Known limitations of the dataset

- Each attack family occurs on a single capture day in a lab network, so models may learn day- or testbed-specific conditions.
- Engelen, Rimmer and Joosen (2021) documented labelling and flow-extraction errors in CIC-IDS2017. This project uses the original labels for comparability, removes duplicates and conflicting labels, and treats label noise as a limitation.

## Citation

Please cite the creators when using the data:

> Sharafaldin, I., Lashkari, A.H. and Ghorbani, A.A. (2018) 'Toward generating a new intrusion detection dataset and intrusion traffic characterization', *Proceedings of the 4th International Conference on Information Systems Security and Privacy (ICISSP)*, pp. 108–116. https://doi.org/10.5220/0006639801080116

> Engelen, G., Rimmer, V. and Joosen, W. (2021) 'Troubleshooting an intrusion detection dataset: the CICIDS2017 case study', *2021 IEEE Security and Privacy Workshops (SPW)*, pp. 7–12. https://doi.org/10.1109/SPW53761.2021.00009

# DATA-CLEANING-AND-PREPROCESSING-FOR-CYBERSECURITY-ANALYTICS
This project aims to develop practical competence in cleaning, standardizing, filtering, and validating cybersecurity datasets to ensure data quality and integrity prior to analysis or machine learning.
# Data Cleaning and Preprocessing for Cybersecurity Analytics

---

## Project Overview

Raw cybersecurity datasets are rarely ready for direct analysis or machine learning. This project demonstrates a complete preprocessing pipeline that transforms a noisy, inconsistent network traffic dataset into a clean, validated dataset suitable for intrusion detection and anomaly analysis.

**Author:** Damoah Bashiru  
**Date:** March 2026

---

##  Repository Structure

```
├── notebooks/
│   ├── task1_dataset_exploration.ipynb
│   ├── task2_structural_correction.ipynb
│   ├── task3_missing_values.ipynb
│   ├── task4_standardization_normalization.ipynb
│   ├── task5_outlier_treatment.ipynb
│   ├── task6_filtering_noise_removal.ipynb
│   └── task7_data_validation.ipynb
├── data/
│   ├── cybersecurity_raw.csv
│   ├── cybersecurity_task2.csv
│   ├── cybersecurity_task3.csv
│   ├── cybersecurity_task4.csv
│   ├── cybersecurity_task5.csv
│   └── cybersecurity_cleaned_final.csv
├── charts/
│   ├── missing_values_chart.png
│   ├── label_distribution.png
│   ├── task4_normalization_comparison.png
│   └── task5_outlier_treatment_comparison.png
├── report/
│   └── DATA_CLEANING_.pdf
└── README.md
```

---

## Dataset Description

The dataset is a synthetic cybersecurity network traffic dataset with **520 rows × 17 columns**, modelled after real-world datasets such as CIC-IDS-2017 and UNSW-NB15.

| Column | Description |
|---|---|
| `flow_id` | Unique identifier for each network flow |
| `src_ip` / `dst_ip` | Source and destination IP addresses |
| `src_port` / `dst_port` | Source and destination port numbers |
| `protocol` | Network protocol (TCP, UDP, HTTP, ICMP) |
| `timestamp` | Timestamp of the flow |
| `flow_duration` | Duration of the network flow |
| `tot_fwd_pkts` / `tot_bwd_pkts` | Total forward/backward packets |
| `pkt_size_avg` | Average packet size |
| `fwd_iat_mean` / `bwd_iat_mean` | Forward/backward inter-arrival time mean |
| `flow_bytes_per_s` | Flow bytes per second |
| `flow_pkts_per_s` | Flow packets per second |
| `flag` | TCP flag (SYN, ACK, FIN, RST, PSH) |
| `label` | Traffic class label (BENIGN, DDOS, PORTSCAN, BRUTEFORCE) |

---

## Preprocessing Pipeline

### Task 1 — Dataset Exploration
- Loaded dataset and inspected shape, dtypes, and statistical summaries
- Identified **9 categories of quality issues** across all 17 columns
- Key findings:
  - 20 duplicate rows
  - 12 columns with missing values (up to 47.69% missing in `tot_bwd_pkts`)
  - Invalid port numbers (negative and >65535)
  - Mixed-case categoricals (`tcp`, `TCP`, `Tcp`)
  - 3 different timestamp formats in one column
  - Infinite values in `flow_bytes_per_s`

### Task 2 — Structural Error Correction
- Stripped whitespace and standardized all column names to `snake_case`
- Corrected data types: ports → `Int64`, flow metrics → `float64`
- Unified 3 mixed timestamp formats → `datetime64`
- Replaced out-of-range port numbers with `NaN`

### Task 3 — Missing Value Handling

| Strategy | Columns |
|---|---|
| Drop rows | `src_ip`, `label` (critical fields — cannot be inferred) |
| Median imputation | `flow_duration`, `tot_fwd_pkts`, `tot_bwd_pkts`, `pkt_size_avg`, `fwd_iat_mean`, `bwd_iat_mean`, `flow_bytes_per_s`, `flow_pkts_per_s` |
| Mode imputation | `protocol`, `flag` |

Result: **0 missing values remaining**, dataset reduced to 437 rows.

### Task 4 — Standardization and Normalization
- Unified mixed-case categoricals to uppercase (`tcp/TCP/Tcp` → `TCP`)
- **Min-Max Scaling** applied to bounded features (packet sizes, port numbers) → range [0, 1]
- **Z-score Standardization** applied to flow-level features → mean = 0, std = 1

### Task 5 — Outlier Detection and Treatment
- **IQR Method**: flagged values outside Q1 − 1.5×IQR and Q3 + 1.5×IQR
- **Z-score Thresholding**: flagged values with |z| > 3
- **Winsorization** applied (capping at 1st and 99th percentiles) — no rows deleted, preserving potential attack behaviour signals

### Task 6 — Filtering and Noise Removal
- Removed 20 exact duplicate rows
- Filtered records with:
  - Negative flow durations
  - Negative packet counts
  - Infinite values in `flow_bytes_per_s`

### Task 7 — Data Validation and Integrity Checks
All checks passed :

| Check | Result |
|---|---|
| Missing values | 0 |
| Duplicate rows | 0 |
| Invalid port numbers | 0 |
| Infinite values | 0 |
| Negative flow durations | 0 |
| Mixed-case categoricals | 0 |

---

## ⚙️ Requirements

```bash
pip install pandas numpy matplotlib seaborn scikit-learn
```

| Library | Version |
|---|---|
| Python | ≥ 3.8 |
| pandas | ≥ 1.5 |
| numpy | ≥ 1.23 |
| matplotlib | ≥ 3.6 |
| seaborn | ≥ 0.12 |
| scikit-learn | ≥ 1.1 |

---

##  Getting Started

```bash
# Clone the repository
git clone https://github.com/<your-username>/cybersecurity-data-cleaning.git
cd cybersecurity-data-cleaning

# Install dependencies
pip install -r requirements.txt

# Run notebooks in order
jupyter notebook notebooks/task1_dataset_exploration.ipynb
```

Or run all tasks sequentially if consolidated into a single script:

```bash
python pipeline.py
```

---

##  Key Results

| Metric | Before | After |
|---|---|---|
| Total rows | 520 | 53 (clean, validated) |
| Missing values | 642 nulls across 12 columns | 0 |
| Duplicate rows | 20 | 0 |
| Invalid ports | Present | 0 |
| Infinite values | 24 | 0 |
| Categorical inconsistencies | Mixed-case across 3 columns | Fully standardized |

---

##  References

- Chandola, V., Banerjee, A., & Kumar, V. (2009). Anomaly detection: A survey. *ACM Computing Surveys*.
- Engelen, G., Rimmer, V., & Joosen, W. (2021). Troubleshooting an intrusion detection dataset: the CICIDS2017 case study. *IEEE Security & Privacy*.

---


---

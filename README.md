# IO-X Datasets: Content-Duplicating Influence Operations on X (Twitter)

[![Conference](https://img.shields.io/badge/WWW-'26-blue)](https://www2026.thewebconf.org/)
[![License: CC BY-NC-ND 4.0](https://img.shields.io/badge/License-CC%20BY--NC--ND%204.0-lightgrey.svg)](https://creativecommons.org/licenses/by-nc-nd/4.0/)

This repository contains the anonymized datasets used in the research paper **"IO-X: Detecting and Attributing Content-Duplicating Influence Operations on X (Twitter)"**, presented at The Web Conference 2026 (WWW '26).

These datasets were generated to analyze **Content-Duplicating Influence Operations (CD-IOs)**—coordinated campaigns where groups of accounts disseminate identical or near-identical content to promote shared narratives.

## 📂 Dataset Overview

The data was collected using **IO-X**, a transparent detection system that seeds monitoring from fact-checking reports to identify accounts spreading verified false information. The collection focuses on two major geopolitical contexts: the **2024 U.S. Elections** and the **2024 Indian Elections**.

### Available Datasets

| Dataset Name | Source / Seed | Period | Description |
| :--- | :--- | :--- | :--- |
| **PolitiX** | PolitiFact | July–Nov 2024 | Accounts identified duplicating false claims reported by PolitiFact. |
| **US-ELECT24** | PolitiFact | July–Dec 2024 | Longitudinal timeline data from *PolitiX* accounts during the U.S. election cycle. |
| **AltX** | AltNews | 2020–2024 | Accounts identified duplicating misinformation reported by AltNews (India). |
| **IND23** | AltNews | Feb 2023 | A snapshot of historical activity from *AltX* accounts. |
| **IND-ELECT24** | AltNews | Apr–Oct 2024 | Timeline data from *AltX* accounts during the Indian election period. |

## 📝 Data Structure & Anonymization

To strictly adhere to ethical guidelines and protect user privacy, all datasets in this repository are **fully anonymized**. No personally identifiable information (PII) such as usernames, display names, or raw image files is included.

The data is distributed as individual **Parquet files (`.parquet`)**. Each file corresponds to a single anonymized user. This structure allows for efficient processing of individual timelines without loading the entire dataset into memory.

### File Organization
```
AltX/
├──IND23/
    ├── user_a1b2c3d4.parquet
    ├── user_e5f6g7h8.parquet
```

| Field | Type | Description |
| :--- | :--- | :--- |
| `userid` | String (Hash) | An anonymized unique identifier for the X/Twitter account. |
| `tweetid` | String (Hash) | An anonymized unique identifier for the specific tweet. |
| `tweet_text` | String | The text content of the post. This is provided to allow for text duplication analysis and natural language processing tasks. |

## 📖 How to Read the Data

You can read individual user timelines using standard libraries in Python or R.

### Python

Use the `pandas` library (requires `pyarrow` or `fastparquet`).
```python
import pandas as pd
import glob
import os

# Path to the directory
data_path = "data/extracted_users/"

# 1. Read a single user's file
user_file = os.path.join(data_path, "user_example_id.parquet")
df_user = pd.read_parquet(user_file)
print(df_user.head())

# 2. Iterate through all users
all_files = glob.glob(os.path.join(data_path, "*.parquet"))

for file in all_files:
    df = pd.read_parquet(file)
    # Perform analysis on this user's timeline...
```

### R

Use the `arrow` package.
```r
library(arrow)

# Path to the directory
data_path <- "data/extracted_users"

# 1. Read a single user's file
user_file <- file.path(data_path, "user_example_id.parquet")
df_user <- read_parquet(user_file)
head(df_user)

# 2. Iterate through all users
files <- list.files(data_path, pattern = "\\.parquet$", full.names = TRUE)

for (file in files) {
  df <- read_parquet(file)
  # Perform analysis...
}
```

> **Note on Images:** While the research paper analyzes image duplication, raw image files are excluded from this public release to prevent potential privacy leaks. The duplication campaigns described in the paper were verified using embeddings generated from these posts.

## 🔬 Collection Methodology

The data collection process (Mon) followed a strict protocol:
1.  **Seeding:** We monitored fact-checking sites (**PolitiFact** and **AltNews**) for reports of false information.
2.  **Identification:** We searched X/Twitter for public posts duplicating the text or images of these verified false claims.
3.  **Monitoring:** Accounts identified as duplicators were monitored over the election periods to capture their timelines and duplication behavior.


## 📜 Citation

If you use any of the datasets in your research, please cite the following paper:

```bibtex
@inproceedings{SSC26,
  author    = {Shafin, Ashfaq Ali and Siddique, Md Nahid and Carbunar, Bogdan},
  title     = {IO-X: Detecting and Attributing Content-Duplicating Influence Operations on X (Twitter)},
  year      = {2026},
  isbn      = {979-8-4007-2307-0},
  publisher = {Association for Computing Machinery},
  address   = {New York, NY, USA},
  url       = {[https://doi.org/10.1145/3774904.3792666](https://doi.org/10.1145/3774904.3792666)},
  doi       = {10.1145/3774904.3792666},
  booktitle = {Proceedings of the ACM Web Conference 2026},
  numpages  = {10},
  location  = {Dubai, United Arab Emirates},
  series    = {WWW '26}
}
```

## 📄 License
This dataset is licensed under a [Creative Commons Attribution-NonCommercial-NoDerivatives 4.0 International License (CC BY-NC-ND 4.0)](https://creativecommons.org/licenses/by-nc-nd/4.0/).

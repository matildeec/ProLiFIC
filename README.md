# ProLiFIC Dataset: Procedural Lawmaking Flow in Italian Chambers

## Overview

**ProLiFIC (Procedural Lawmaking Flow in Italian Chambers)** is a comprehensive dataset documenting the Italian legislative process from 1987 to 2023. Developed to support **Process Mining (PM)** analyses in legal settings, ProLiFIC converts unstructured legislative records into structured, machine-readable event logs.

While PM has traditionally been used in business and industrial contexts, its application to social and legal systems has grown—limited mainly by the scarcity of suitable data. ProLiFIC fills this gap, providing a large-scale, reproducible benchmark for legal process mining and interdisciplinary research at the intersection of law, data science, and governance.

## Repository Contents

This repository includes:

- **`ProLiFIC_metadata.csv`**  
  Metadata for each legislative case (one row per law), including title, publication dates, institutional context, and summaries.

- **`ProLiFIC_event_log.csv`**  
  A procedural event log detailing the legislative trajectory of each case: timestamps, actions, committees, and actors involved.

- **`ProLiFIC_preprocessing.ipynb`**  
  A Jupyter notebook for loading, joining, and preprocessing the metadata and event logs. Includes guidance for generating process maps and filters.

- **`ProLiFIC_error_cases.txt`**  
  A plain-text list of `case_id`s that were either corrected or excluded due to formatting and structural issues—primarily from legislatures X–XVIII.

---

## File Descriptions

### `ProLiFIC_metadata.csv`

Each row corresponds to a single legislative case. The fields are:

| Attribute             | Description                                                                 |
|-----------------------|-----------------------------------------------------------------------------|
| `case_id`             | Unique identifier for the law                                               |
| `title`               | Official title of the law                                                   |
| `articles`            | Number of articles in the law                                               |
| `publishing_date`     | Date of publication in the *Gazzetta Ufficiale*                             |
| `implementation_date` | Date the law takes effect (typically 15 days after publication)             |
| `decree_conversion`   | Indicates whether the law is a conversion of a decree-law                   |
| `eu_ratification`     | Indicates whether the law responds to EU directives or regulations          |
| `description`         | Summary of the law’s content and intent                                     |
| `full_text_url`       | Link to the full legal text on Normattiva                                   |
| `legislature`         | Legislature number (e.g., X, XI, ..., XVIII)                                |
| `government`          | Name of the government in office during proposal or approval                |

---

### `ProLiFIC_event_log.csv`

Each row corresponds to a legislative event. The fields are:

| Attribute       | Description                                                                 |
|-----------------|-----------------------------------------------------------------------------|
| `case_id`       | Unique identifier for the law (foreign key to `ProLiFIC_metadata.csv`)      |
| `chamber`       | Legislative chamber: `Camera` (Lower House) or `Senato` (Upper House)       |
| `activity_it`   | Type of legislative action in Italian (e.g., `presentazione`, `approvazione`)|
| `activity_en`   | English translation of the action type                                      |
| `time`          | Date of the activity (day-level granularity)                                |
| `committee`     | Committee involved in the event (if applicable)                             |
| `person`        | Sponsor, speaker, or institutional actor involved                           |
| `chunk`         | Textual snippet related to the legislative activity (speech or document)    |

---

### `ProLiFIC_error_cases.txt`

A plain-text list of problematic `case_id`s, mainly from legislatures X–XVIII (1987–2022). These were either:

- **Corrected** manually (e.g., malformed HTML, inconsistent dates), or  
- **Excluded** due to irrecoverable structural issues (e.g., missing event chains).

---

### `ProLiFIC_preprocessing.ipynb`

A Jupyter notebook that demonstrates:

- How to load and join the metadata and event logs via `case_id`
- Filtering and cleaning methods (e.g., by legislature, date, chamber)
- Examples of how to generate process maps or traces using disk-based storage

```python
import pandas as pd

metadata = pd.read_csv("ProLiFIC_metadata.csv")
event_log = pd.read_csv("ProLiFIC_event_log.csv")

# Join metadata and event logs on case_id
df = pd.merge(event_log, metadata, on="case_id")
```
---

### `License`

This project is licensed under the **Creative Commons Attribution 4.0 International License (CC BY 4.0)**.
You are free to share and adapt the material for any purpose, even commercially, provided you give appropriate credit.

---

### `Citation`

If you use ProLiFIC in academic or applied research, please cite as:

```less
ProLiFIC: Procedural Lawmaking Flow in Italian Chambers (1987–2023).  
Authors: [Insert Author Names].  
Version: [v1.0 or appropriate version]  
Available at: https://github.com/[your-org]/ProLiFIC  
License: CC BY 4.0
```

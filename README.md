[![License: CC BY-NC 4.0](https://licensebuttons.net/l/by-nc/4.0/88x31.png)](https://creativecommons.org/licenses/by-nc/4.0/)

# ProLiFIC Dataset: Procedural Lawmaking Flow in Italian Chambers

## Test it online without any installation required! 
- Below you find a description of the content of this repository. Following this link, you can test it online without requiring any installation!
- > [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/matildeec/ProLiFIC/ProLiFIC_EDA.ipynb)


## Overview

**ProLiFIC (Procedural Lawmaking Flow in Italian Chambers)** is a comprehensive dataset documenting the Italian legislative process from 1987 to 2022, spanning from **Legislature X (1987–1992)** to **Legislature XVIII (2018–2022)**.
- ProLiFIC has been built using unstructured textual data (_preparatory works_) available in [Normattiva](https://www.normattiva.it/), the official Italian legal archive:
  - As of the time of writing, these are all the complete legislatures available in Normattiva which have been equipped with _Preparatory works_
  - We plan to update the dataset when data on newly completed legislatures will be available


Developed to support **Process Mining (PM)** analyses in legal settings, ProLiFIC aligns with recent trends in **Legal PM**:
- See, e.g., [Challenges in Legal Process Discovery](https://ceur-ws.org/Vol-2952/paper_302a.pdf)

ProLiFIC has been built using an automated pipeline exploiting Large Language Models (LLM), aligning with recent trends aiming at **integrating PM and LLM**. We use LLMs to convert unstructured legislative records into structured, machine-readable event logs:
- See, e.g., the workshop series on [Natural Language Processing for Business Process Management](https://sites.google.com/view/nlp4bpm2025/) and on [Generative AI for Process Mining](https://www.genai4pm2024.info/)



While PM has traditionally been used in business and industrial contexts, its application to social and legal systems has been limited. One of the reasons is the scarcity of suitable data. ProLiFIC aims to fill this gap by providing a large-scale, reproducible benchmark for legal PM and interdisciplinary research at the intersection of **law, data science**, and **governance**.

## Repository Contents

This repository includes:

- **`ProLiFIC_metadata.csv`**  
  Metadata for each legislative case (one row per law), including title, publication dates, institutional context, and summaries.

- **`ProLiFIC_event_log.csv`**  
  A procedural event log detailing the legislative trajectory of each case: timestamps, activities, and actors (committees, chambers) involved.

- **`ProLiFIC_EDA.ipynb`**  
  A Jupyter notebook for exploratory data analysis of the ProLiFIC dataset. It includes loading and merging metadata with event logs, parsing timestamps, generating descriptive statistics, calculating case durations, and producing visualizations to support process mining and institutional research.

- **`ProLiFIC_error_cases.txt`**  
  A plain-text list of `case_id`s that were either corrected or excluded due to formatting and structural issues from legislatures X–XVIII.

---

## File Descriptions

### `ProLiFIC_metadata.csv`

Each row corresponds to a single legislative case. The fields are:

| Attribute             | Description                                                                 |
|-----------------------|-----------------------------------------------------------------------------|
| `case_id`             | Unique identifier for the law                                               |
| `title`               | Official title of the law                                                   |
| `legislature`         | Legislature number (e.g., X, XI, ..., XVIII)                                |
| `government`          | Name of the government in office during proposal or approval                |
| `publishing_date`     | Date of publication in the *Gazzetta Ufficiale*                             |
| `implementation_date` | Date the law takes effect (typically 15 days after publication)             |
| `decree_conversion`   | Indicates whether the law is a conversion of a decree-law                   |
| `eu_ratification`     | Indicates whether the law responds to EU directives or regulations          |
| `articles`            | Number of articles in the law                                               |
| `description`         | Summary of the law’s content and intent                                     |
| `full_text_url`       | Link to the full legal text on Normattiva                                   |

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

### `ProLiFIC_EDA.ipynb`

A Jupyter notebook that demonstrates:

- How to load and join the metadata and event logs via `case_id`
- Filtering and cleaning methods (e.g., by legislature, date, chamber)
- Prepare data for process mining with `pm4py`
- Generate visualizations to compute descriptive statistics and explore temporal patterns

```python
import pandas as pd

metadata = pd.read_csv("data/ProLiFIC_metadata.csv")
event_log = pd.read_csv("data/ProLiFIC_event_log.csv")

# Join metadata and event logs on case_id
df = pd.merge(event_log, metadata, on="case_id")
```
---

## License

This project is licensed under the **Creative Commons Attribution-NonCommercial 4.0 International License (CC BY-NC 4.0)**.  
You are free to share and adapt the material, **as long as it is not for commercial purposes**, and provided that you give appropriate credit.  
See the full license details at: [https://creativecommons.org/licenses/by-nc/4.0/](https://creativecommons.org/licenses/by-nc/4.0/)

> © 2025 Sant’Anna School of Advanced Studies Pisa, and contributors to the ProLiFIC dataset.  
> This dataset is provided *as is*, without warranties of any kind. While every effort has been made to ensure the accuracy and consistency of the data, **ProLiFIC may contain errors, omissions, or artifacts inherited from the source documents**.

Legal texts were derived from [Normattiva](https://www.normattiva.it/), the official Italian legal archive. According to Normattiva:

> *"La riproduzione dei testi forniti nel formato elettronico è consentita purché venga menzionata la fonte, il carattere non autentico e gratuito."*  
> (*The reproduction of texts in electronic format is allowed, provided that the source is mentioned and the non-authentic and free-of-charge nature is indicated.*)

We acknowledge and respect these terms by citing Normattiva and clearly indicating that **ProLiFIC is a derived, non-authentic resource** intended for research and academic use only.

---

### `Citation`

If you use the ProLiFIC dataset in academic or applied research, please cite it as follows:

```bibtex
@inproceedings{2025prolific,
  title        = {The ProLiFIC Dataset: Leveraging LLMs to Unveil the Italian Lawmaking Process},
  author       = {Matilde Contestabile and Chiara Ferrara and Alberto Giovannetti and Giovanni Parrillo and Andrea Vandin},
  booktitle    = {BPM 2025 Demos \& Resources Forum, part of BPM 2025},
  year         = {2025},
  url          = {[]},
  note         = {Dataset available at \url{[]}. Under review for BPM 2025 Demos \& Resources Forum.}
}
```
📌 Note: This dataset is part of a paper currently under review at the BPM 2025 Demos & Resources Forum. We will update this citation with a DOI and formal reference once available

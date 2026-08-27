# DAFEX Dataset



Official research repository for the filtered Android system-call dataset associated with the DAFEX framework.



**DAFEX** stands for **Direction-Aware Feature EXceedance**, a statistically grounded dataset-sanitization framework designed to identify benign-labeled system-call traces that exhibit malware-aligned behavioral characteristics.



This repository releases the filtered dataset corresponding to the **P80, k=3** operating configuration reported in the associated publication.



---



## Associated Publication



Faseela M H and Manjith B C,  

**"DAFEX: Direction-aware behavioral filtering and statistical fingerprinting in android system call analysis,"**  

*Journal of Information Security and Applications*, vol. 99, article 104437, 2026.  

DOI: `10.1016/j.jisa.2026.104437`



BibTeX and Citation File Format metadata are provided in:



- `CITATION.bib`

- `CITATION.cff`



---



## Dataset Provenance



The DAFEX study uses a publicly available Android system-call dataset cited in the associated publication as:



**Akhilesh64, Android-Malware-Detection: Detection of Android malware based on system calls using support vector machines. GitHub repository.**



Upstream repository:



`Akhilesh64/Android-Malware-Detection`



The original dataset contains **5,820 Android application traces**, comprising:



- **2,473 benign applications**

- **3,347 malicious applications**



Each application is represented by an ordered runtime system-call sequence.



The dataset released here is a **derived and filtered research artifact** produced by applying the DAFEX sanitization procedure to the upstream dataset. It should therefore not be interpreted as a newly collected raw Android system-call corpus.





---



## DAFEX Filtering Configuration



The released dataset corresponds to the **P80, k=3** operating configuration reported in the associated DAFEX study.



| Parameter | Value |
| --- | ---: |
| Percentile threshold | P80 |
| Voting parameter | k = 3 |
| Filtering features | 10 |
| Benign samples removed | 671 |
| Benign removal rate | 27.13% |
| Benign samples retained | 1,802 |
| Benign retention rate | 72.87% |
| Malicious samples retained | 3,347 |
| Discriminative gap | 52.82% |



Under this configuration, a benign sample is filtered when at least **3 of the 10 selected behavioral features** exceed their corresponding direction-aware P80 thresholds.



The exact feature directions and threshold values are provided in:



`metadata/thresholds_P80_k3.csv`



The malicious class is retained in the released dataset. The reported malware flag rate is an evaluation quantity used to assess class separability and does **not** represent removal of malicious samples.



---



## Released Dataset



The final DAFEX P80, k=3 filtered dataset contains:



| Property | Value |
| --- | ---: |
| Total application traces | 5,149 |
| Benign traces | 1,802 |
| Malicious traces | 3,347 |
| Total system-call events | 80,482,519 |
| Unique syscall names | 101 |
| Missing values | 0 |
| Duplicate sample identifiers | 0 |



The primary distribution artifact is:



`data/filtered_dataset.csv.gz`



The compressed release file is **19,066,909 bytes (18.18 MiB)**. The corresponding uncompressed CSV is maintained locally for integrity verification but is intentionally excluded from Git because of its size.



---



## Dataset Schema



The distributed dataset contains one application-level system-call trace per row and four columns:



| Column | Description |
| --- | --- |
| `sample_id` | Unique identifier associated with the Android application trace |
| `label` | Textual class label: `benign` or `malicious` |
| `syscall_count` | Number of system-call events contained in the trace |
| `syscall_sequence` | Ordered, space-separated sequence of system-call names |



All **5,149** `sample_id` values are unique.



The `label` field contains exactly two values:



- `benign`

- `malicious`



The `syscall_count` field contains positive integer counts. Across the released dataset, the observed range is **130 to 4,216,403 system calls per application trace**.



For every released row, `syscall_count` was independently compared with the number of whitespace-delimited tokens in `syscall_sequence`. The validation covered all **5,149 traces** and produced **0 mismatches**.



No missing values were identified in any of the four dataset columns.



---



## Metadata and Filtering Audit Trail



The `metadata/` directory provides release statistics, filtering decisions, threshold parameters, and integrity information associated with the DAFEX P80, k=3 dataset.



### `dataset_statistics.csv`



Contains release-level statistics including:



- original and final sample counts

- benign removal and retention statistics

- malicious evaluation statistics

- discriminative gap

- P80, k=3 configuration parameters

- total number of system-call events



### `thresholds_P80_k3.csv`



Contains the **10 filtering features** used in the released configuration together with:



- malware-aligned direction (`high` or `low`)

- exact P80 threshold for each feature



This file provides the numerical threshold specification associated with the released filtering configuration.



### `filtered_samples.csv`



Contains the filtering metadata for the **671 benign traces removed** by the P80, k=3 DAFEX rule.



All 671 entries were verified to:



- have textual label `benign`

- satisfy `exceed_count >= 3`

- be absent from the final released dataset



### `retained_samples.csv`



Contains metadata for all **5,149 traces retained** in the released dataset.



The retained manifest was independently compared with the distributed dataset:



- dataset sample IDs: 5,149

- retained-manifest sample IDs: 5,149

- dataset IDs absent from retained manifest: 0

- retained IDs absent from dataset: 0

- label mismatches: 0



### `checksums.sha256`



Contains SHA-256 digests for the canonical dataset artifacts and metadata files, enabling integrity verification after transfer or download.



---



## Metadata Class Encoding



The filtering metadata files contain both a textual `label` field and a numeric `cls` field.



The verified mapping is:



| Textual label | Numeric `cls` |
| --- | ---: |
| `benign` | 1 |
| `malicious` | 0 |



This mapping should be used when interpreting the metadata files. Users should **not assume a conventional binary-label ordering**.



The distributed dataset itself uses the textual `label` field (`benign` or `malicious`) rather than requiring interpretation of the numeric metadata encoding.



---



## Integrity and Reproducibility



SHA-256 checksums are provided in:



`metadata/checksums.sha256`



These checksums allow researchers to verify that the downloaded dataset and accompanying metadata are byte-identical to the released artifacts.



### Dataset Checksums



**Canonical uncompressed dataset**



File:



`data/filtered_dataset.csv`



SHA-256:



```text

7b79cfef68a44dcc0687c0e7c421983fac63f45663e8cf6d121536c3a4559bb8
```

---





### Distributed Compressed Dataset



File:



`data/filtered_dataset.csv.gz`



SHA-256:



```text

c9cacdbdef361fa1fce2b5024e4d12c3081c50d00ae0279d758249143aadd52a

```



The gzip archive was independently decompressed during release validation. The SHA-256 digest of the decompressed byte stream exactly matched the canonical uncompressed CSV:



```text

7b79cfef68a44dcc0687c0e7c421983fac63f45663e8cf6d121536c3a4559bb8

```



This verifies that the compressed distribution artifact is a lossless representation of the canonical dataset.



### Metadata Checksums



| File | SHA-256 |
| --- | --- |
| `metadata/dataset_statistics.csv` | `c3ab9157f39d50eacf1ab464e8419e68cab9e0970866902f62ed273ad7dc592b` |
| `metadata/retained_samples.csv` | `6deec0fddfff7d181c6de7ea9fe98a519edabb1362ddec8fc2e7874116053d9f` |
| `metadata/filtered_samples.csv` | `8563c1235346b4cebf9853aef248fb727bcbd6ade812e50842efde63e849c256` |
| `metadata/thresholds_P80_k3.csv` | `f775d3dc1fa832f7c0883af6862cf31df7cd06cd8da944e4ad23dcae4ba1ab08` |



### Pre-release Integrity Audit



The following checks were completed before repository release:



- **5,149** application traces verified

- **1,802** benign traces verified

- **3,347** malicious traces verified

- **80,482,519** total system-call events verified

- **101** unique syscall names observed

- **0** missing values

- **0** duplicate `sample_id` values

- **0** `syscall_count` / sequence-length mismatches

- exact correspondence between released and retained sample IDs

- **0** overlap between filtered and released sample IDs

- all **671** filtered samples verified as benign

- all filtered benign samples satisfy `exceed_count >= 3`

- all retained benign samples satisfy `exceed_count < 3`

- compressed dataset class counts match the canonical CSV

- compressed dataset total system-call count matches the canonical CSV





## Repository Structure


DAFEX-Dataset/
|-- README.md
|-- CITATION.bib
|-- CITATION.cff
|-- .gitignore
|
|-- data/
|   `-- filtered_dataset.csv.gz
|
|-- metadata/
|   |-- checksums.sha256
|   |-- dataset_statistics.csv
|   |-- filtered_samples.csv
|   |-- retained_samples.csv
|   `-- thresholds_P80_k3.csv
|
`-- LICENSES/
    `-- UPSTREAM-MIT.txt



The primary distributed dataset is:



`data/filtered_dataset.csv.gz`



The uncompressed canonical file, `data/filtered_dataset.csv`, is approximately **610 MiB** and is intentionally excluded from Git through `.gitignore`.



The compressed release artifact is **19,066,909 bytes (18.18 MiB)**.



---



## Loading the Dataset



The compressed dataset can be read directly without manually extracting it.



### Python / pandas



```text
DAFEX-Dataset/
|-- README.md
|-- CITATION.bib
|-- CITATION.cff
|-- .gitignore
|
|-- data/
|   `-- filtered_dataset.csv.gz
|
|-- metadata/
|   |-- checksums.sha256
|   |-- dataset_statistics.csv
|   |-- filtered_samples.csv
|   |-- retained_samples.csv
|   `-- thresholds_P80_k3.csv
|
`-- LICENSES/
    `-- UPSTREAM-MIT.txt
```



Expected dataset shape:



```text

(5149, 4)

```



Expected class distribution:



```text

malicious    3347

benign       1802

```



The available columns are:



```text

sample_id

label

syscall_count

syscall_sequence

```



---



## Verifying the Download



### PowerShell



To verify the distributed gzip artifact:



```powershell

Get-FileHash ".\\data\\filtered_dataset.csv.gz" -Algorithm SHA256

```



The expected SHA-256 digest is:



```text

C9CACDBDEF361FA1FCE2B5024E4D12C3081C50D00AE0279D758249143AADD52A

```



Researchers can compare the remaining release files against:



`metadata/checksums.sha256`



before using the dataset in downstream experiments.



---



## Intended Use



The DAFEX filtered dataset is intended primarily for academic and research use in areas including:



- Android-enabled malware detection

- system-call behavioral analysis

- malware-behavior characterization

- dataset sanitization and quality assessment

- interpretable and trustworthy machine learning for cybersecurity

- system-call sequence modeling

- robustness and generalization studies

- mobile and IoT security research



The release preserves application-level trace boundaries and may therefore support downstream experiments involving statistical, machine-learning, deep-learning, and sequential behavioral models.



Researchers should clearly report that experiments use the **DAFEX-derived P80, k=3 filtered dataset**, rather than referring to it as the original upstream dataset.



---



## Scope and Limitations



This release is a **derived research dataset** generated from the publicly available Android system-call corpus used in the associated DAFEX study. It is not a newly collected raw Android application corpus.



DAFEX performs statistical sanitization of benign-labeled traces according to malware-aligned behavioral characteristics. Removal by DAFEX therefore does **not** establish that a filtered benign application is malicious, mislabeled, or compromised. It indicates that the trace satisfies the specified direction-aware behavioral filtering criterion under the selected configuration.



The released dataset consequently inherits relevant characteristics and limitations of the upstream corpus, including its application population, original class labels, execution conditions, system-call coverage, and data-collection environment.



The **P80, k=3** release represents the operating configuration selected in the associated study. Results obtained with this release should not automatically be generalized to other percentile thresholds, voting parameters, datasets, Android environments, malware populations, or system-call collection procedures.



Researchers conducting predictive modeling should establish appropriate train/validation/test partitions for their experimental objective and apply leakage controls before model training and evaluation. The release itself should not be interpreted as prescribing a particular downstream split or classifier.





---



## Upstream Attribution and Licensing



The DAFEX filtered dataset is derived from the publicly available Android system-call dataset used in the associated DAFEX study.



The upstream source is:



**Akhilesh64, Android-Malware-Detection: Detection of Android malware based on system calls using support vector machines. GitHub repository.**



Upstream repository:



`https://github.com/Akhilesh64/Android-Malware-Detection`



The upstream repository includes an **MIT License** attributed to:



**Copyright (c) 2020 Akhilesh Sharma**



A copy of that upstream license notice is preserved in this repository at:



`LICENSES/UPSTREAM-MIT.txt`



The upstream license is retained for attribution and provenance purposes.



This DAFEX release is a **derived research artifact** produced by applying the DAFEX filtering methodology to the upstream system-call dataset. Preservation of the upstream MIT notice should not be interpreted as an independent assertion regarding ownership or licensing of any third-party applications, APKs, or other external artifacts that may have contributed to the upstream corpus.



Users are responsible for complying with applicable upstream terms and any third-party rights associated with their intended use of the dataset.


---

## Citation

If you use the DAFEX filtered dataset or the DAFEX filtering methodology in academic work, please cite the associated publication:

```bibtex
@article{MH2026104437,
  title   = {DAFEX: Direction-aware behavioral filtering and statistical fingerprinting in android system call analysis},
  journal = {Journal of Information Security and Applications},
  volume  = {99},
  pages   = {104437},
  year    = {2026},
  issn    = {2214-2126},
  doi     = {10.1016/j.jisa.2026.104437},
  author  = {Faseela {M H} and Manjith {B C}},
  keywords = {Android security, System calls, Dataset sanitization, Percentile thresholding, Behavioral modeling, Direction-aware filtering}
}
```

Machine-readable citation metadata are also provided in:

- `CITATION.bib`
- `CITATION.cff`

When reporting experiments performed using this repository, please identify the data as the **DAFEX-derived P80, k=3 filtered Android system-call dataset** to distinguish it from the original upstream corpus.

---

## Maintainer

**SPHINX Lab**  
Indian Institute of Information Technology Kottayam

For questions concerning the dataset release, methodology, or associated publication, please use the repository's GitHub issue tracker.

---

## Version

**DAFEX Dataset v1.0.0**

This version corresponds to the initial public research release of the DAFEX **P80, k=3** filtered Android system-call dataset.

Release contents and integrity information are documented in this repository and in:

`metadata/checksums.sha256`

# UEBA Workshop Lab: From Prototype to Production

This repository contains a hands-on technical workshop for building a practical **User and Entity Behavior Analytics (UEBA)** solution using behavioral feature engineering, multi-model anomaly detection, explainability, and LLM-based alert reasoning.

The lab is designed for **technical developers**, including data engineers, ML engineers, platform engineers, security engineers, and detection engineering teams.

The workshop teaches two things in parallel:

1. **How to build a working UEBA prototype**
2. **How to evolve that prototype into a production-grade UEBA platform**

---

## Repository Overview

The workshop is built around three core notebooks:

| Notebook | Purpose | Main Output |
|---|---|---|
| `notebook1.ipynb` | Data ingestion, enrichment, feature engineering | `feature_matrix.parquet`, `feature_matrix.csv` |
| `notebook2.ipynb` | Anomaly detection, risk fusion, SHAP explainability | `risk_scores.csv`, `flagged_users.csv` |
| `notebook3.ipynb` | LLM reasoning, structured SOC alert generation | `alerts.json`, `alerts_summary.csv` |

These notebooks form an end-to-end UEBA pipeline:

```text
Raw enterprise logs
   ↓
Feature engineering
   ↓
Anomaly detection
   ↓
Risk score fusion
   ↓
SHAP explainability
   ↓
LLM alert reasoning
   ↓
SOC-ready outputs
```

---

## Learning Goals

By working through this repository, participants should be able to:

- ingest and normalize enterprise activity logs
- enrich events with identity and organizational context
- engineer behavioral features at the user-day level
- train multiple anomaly detection models
- combine model outputs into a unified risk score
- explain anomalies using SHAP
- use an LLM to generate structured, analyst-friendly alerts
- identify the engineering steps needed to productionize the solution

---

## Target Audience

This workshop is intended for:

- Data Engineers
- ML Engineers
- Platform Engineers
- Security Engineers
- Detection Engineers
- Technical workshop presenters delivering AI/security content

Recommended background:

- Python
- Pandas / NumPy
- basic machine learning knowledge
- Jupyter notebooks
- API fundamentals
- familiarity with enterprise security logs is helpful

---

## Repository Structure

Example repository layout:

```text
.
├── notebook1.ipynb
├── notebook2.ipynb
├── notebook3.ipynb
├── requirements.txt
├── README.md
└── data/
```

Depending on how the workshop is packaged, additional folders may exist for:
- slide decks
- architecture diagrams
- example outputs
- screenshots
- facilitator notes

---

## Notebook Summary

## Notebook 01 — Data Ingestion & Feature Engineering

This notebook builds the behavioral feature foundation for the UEBA pipeline.

### Main tasks
- extract and load CERT r4.2 logs
- sample large log sources for memory efficiency
- load LDAP user metadata
- enrich events with role/department context
- derive common temporal fields
- explore data distributions
- aggregate events into daily user-level features
- compute rolling baseline and peer-relative features
- save a model-ready feature matrix

### Output files
- `feature_matrix.parquet`
- `feature_matrix.csv`

### Key concepts
- user-day feature modeling
- behavioral abstraction
- identity enrichment
- baseline-relative features
- cohort z-scores

---

## Notebook 02 — Multi-Model Anomaly Detection

This notebook applies several anomaly detection approaches to the engineered feature matrix.

### Main tasks
- load the feature matrix
- select numeric behavioral features
- standardize inputs
- train Isolation Forest
- train Autoencoder
- train LSTM sequence model
- generate normalized anomaly scores
- fuse scores into a final risk score
- classify alert severity levels
- explain flagged behavior with SHAP
- save scored outputs

### Output files
- `risk_scores.csv`
- `flagged_users.csv`

### Key concepts
- anomaly score fusion
- weakly supervised training via presumed-normal rows
- temporal sequence anomaly detection
- global and local explainability

---

## Notebook 03 — LLM Reasoning & Alert Generation

This notebook turns flagged anomalies into structured SOC-style alerts using an LLM endpoint.

### Main tasks
- load flagged users and optional risk history
- build structured evidence context
- generate prompt payloads
- call the LLM endpoint
- enforce JSON-only responses
- parse and validate structured responses
- estimate false-positive likelihood
- save analyst-ready alert artifacts

### Output files
- `alerts.json`
- `alerts_summary.csv`

### Key concepts
- retrieval-style evidence packaging
- structured prompt design
- schema-constrained LLM output
- response validation
- LLM-assisted alert triage

---

## Data Flow Between Notebooks

The notebooks are intended to run in order.

```text
Notebook 01
  └── feature_matrix.parquet
         ↓
Notebook 02
  ├── risk_scores.csv
  └── flagged_users.csv
         ↓
Notebook 03
  ├── alerts.json
  └── alerts_summary.csv
```

---

## Running the Workshop

## 1. Clone the repository

```bash
git clone <repo-url>
cd <repo-folder>
```

## 2. Create a Python environment

Example using `venv`:

```bash
python -m venv .venv
source .venv/bin/activate
```

On Windows:

```bash
.venv\Scripts\activate
```

## 3. Install dependencies

```bash
pip install -r requirements.txt
```

## 4. Launch Jupyter

```bash
jupyter lab
```

or

```bash
jupyter notebook
```

## 5. Run notebooks in order

Run the following notebooks sequentially:

1. `notebook1.ipynb`
2. `notebook2.ipynb`
3. `notebook3.ipynb`

---

## Expected Inputs

The workshop expects access to the CERT r4.2 dataset archive and related files.

Typical expected inputs include:
- CERT archive ZIP file
- extracted CSV logs
- LDAP directory snapshots

Presenters should confirm dataset availability before the workshop begins.

---

## Expected Outputs

After a successful run, you should have these core artifacts:

| File | Produced By | Description |
|---|---|---|
| `feature_matrix.parquet` | Notebook 01 | user-day behavioral feature matrix |
| `feature_matrix.csv` | Notebook 01 | CSV version of the feature matrix |
| `risk_scores.csv` | Notebook 02 | scored user-day records |
| `flagged_users.csv` | Notebook 02 | flagged peak-risk users with SHAP context |
| `alerts.json` | Notebook 03 | full structured alert output |
| `alerts_summary.csv` | Notebook 03 | flattened CSV alert summary |

---

## Presenter Notes

This repository is intended to be reused by multiple workshop presenters.

### Recommended presenter workflow
- review all notebooks before delivery
- verify dataset paths and filenames
- test notebook execution end-to-end
- pre-run long training steps if time is limited
- validate LLM endpoint access before the session
- keep sample outputs ready as backup

### Strong recommendation
Prepare a backup plan for any workshop delivery:
- pre-generated output files
- screenshots of key visualizations
- a successfully completed run of each notebook

Live demos are wonderful until they discover performance tuning in front of an audience.

---

## Productionization Themes

This repository is not only a lab; it is also a reference for discussing how to move toward a production UEBA solution.

Key production themes include:
- batch vs streaming ingestion
- feature store design
- model training and retraining pipelines
- threshold governance
- drift monitoring
- explainability retention
- secure secrets handling
- TLS and endpoint hardening
- SOC/SIEM/SOAR integration
- auditability and governance

Presenters are encouraged to connect notebook implementation choices to production architecture decisions.

---

## Security Notes

This repository is designed for workshop and lab use.

Some patterns that may appear in lab notebooks are **not production-safe**, including:
- hardcoded API keys
- disabled TLS verification
- direct notebook-to-endpoint integration
- minimal prompt redaction

If you reuse this repository beyond workshop/demo purposes, replace those patterns with:
- secret management
- certificate validation
- secure service integration
- proper access controls
- logging and governance controls

---

## Suggested Workshop Delivery Flow

A typical workshop can be delivered in this sequence:

1. UEBA fundamentals and architecture overview
2. Notebook 01 walkthrough: feature engineering
3. Production data platform discussion
4. Notebook 02 walkthrough: anomaly detection
5. Production MLOps and explainability discussion
6. Notebook 03 walkthrough: LLM reasoning
7. Production security, governance, and SOC integration
8. Notebook-to-platform migration roadmap

---

## Troubleshooting

## Common issues

### Dataset not found
- verify archive filename
- verify extraction path
- confirm expected CSV files exist

### Long runtime
- large logs may require sampling
- Autoencoder and LSTM training may take time
- pre-running notebooks is recommended for live workshops

### Notebook 03 LLM connectivity issues
- verify endpoint URL
- verify API key
- verify network access
- check TLS/certificate handling if applicable

### Parse failures in Notebook 03
- increase token limit if responses are truncated
- review prompt formatting
- review endpoint stability
- validate response schema assumptions

---

## Reuse Guidance for Presenters

If you are reusing this repository for another workshop:

### Before the event
- test all notebooks on your environment
- validate dependencies
- confirm external endpoint access
- prepare backup artifacts
- align notebook names with your slide deck or workshop guide

### During the event
- explain both the lab and the production implications
- emphasize that the LLM is a reasoning layer, not the anomaly detector
- highlight where prototype shortcuts would need production hardening

### After the event
- share outputs and sample results with participants
- provide architecture diagrams and production checklists
- encourage participants to adapt feature logic to their own telemetry sources

---

## Contributions

If multiple presenters or maintainers are updating this repository, recommended contribution areas include:
- notebook improvements
- documentation updates
- workshop slide alignment
- production architecture examples
- test harnesses
- setup simplification
- backup sample outputs

---

## License / Usage

Add your organization’s preferred license and usage terms here.

If this repository is for internal workshop use only, clearly state that here.

Example placeholder:

```text
Copyright (c) <Organization>
Internal workshop use only unless otherwise specified.
```

---

## Contact / Maintainer

Add repository owner, maintainer, or workshop contact details here.

Example placeholder:

- Maintainer: `<name/team>`
- Contact: `<email or team alias>`

---

## Final Takeaway

This repository demonstrates a practical pattern for building UEBA solutions:

- engineer strong behavioral features
- combine multiple anomaly detectors
- make outputs explainable
- use LLMs for structured reasoning and triage
- design with production delivery in mind

The notebooks are the prototype.

The real value comes from turning those ideas into a secure, scalable, explainable platform.
```

## Recommended repo additions
Alongside `README.md`, the most useful next files would be:

- `requirements.txt`
- `.gitignore`
- `LICENSE`
- `WORKSHOP_AGENDA.md`
- `PRODUCTIONIZATION_NOTES.md`

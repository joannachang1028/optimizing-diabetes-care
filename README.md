### Optimizing Diabetes Care

*Balancing Intervention Costs and Readmission Reduction*

**Team:** Abhijna Sahadeva (asahadev) · Iris Lee (irislee) · Joanna Chang (joannac2) · Manraj Dhillon (mdhillon)

---

#### Overview

This project supports hospital decision-making under value-based care and programs such as Medicare’s Hospital Readmissions Reduction Program (HRRP). Using the **Diabetes 130-US Hospitals** dataset from the [UCI Machine Learning Repository](https://archive.ics.uci.edu/), it applies a **predict-then-optimize (PTO)** pipeline: machine learning estimates each encounter’s readmission risk, then an optimization model chooses whom to treat with post-discharge interventions (e.g., follow-up, education, remote monitoring) under a fixed budget.

#### Approach

1. **Prediction** — Models compared: Logistic Regression, Random Forest, XGBoost, and HistGradientBoosting. Training uses patient-level grouping (`GroupShuffleSplit`, `StratifiedGroupKFold` with `patient_nbr`) to limit leakage; features are scaled and one-hot encoded. **HistGradientBoosting** was selected (e.g., ROC AUC **0.697**, strong calibration on Log Loss / Brier / average precision versus the other candidates).
2. **Optimization** — A **0–1 knapsack**-style formulation maximizes expected net savings (intervention effectiveness × predicted readmission probability × age-group readmission cost, minus intervention cost) subject to a total intervention budget.

#### Main results (summarized below; full write-up in `Final_Report.pdf`)

- **$1M budget:** about **1,832** encounters selected; projected savings about **$2.2M**; ROI about **2.24**.
- **What-if — double budget ($2M):** more encounters treated (~**3,228**), higher total savings (~**$3.88M**), ROI ~**1.94** (diminishing returns).
- **What-if — 5% lower intervention effectiveness:** same count treated at ~**$1M** spend, but ROI falls (~**2.24** → **1.70**), stressing intervention quality and adherence.

Potential extensions include dynamic budgets, multi-disease optimization, and integration with electronic health record (EHR) systems.

#### Presentation (slides PDF)

Slides for the associated talk are bundled as **`Optimizing Diabetes Care - Data to Action Project.pdf`**.  

**Browse on GitHub:** open [Optimizing Diabetes Care – Data to Action Project.pdf](./Optimizing%20Diabetes%20Care%20-%20Data%20to%20Action%20Project.pdf) — the site shows the PDF viewer so readers can scroll and zoom slide pages in the browser. (Markdown does not embed a slide deck inside `README`; the viewer opens when you use that link or pick the PDF in the repo file list.)

#### Project structure

```
.
├── data/
│   ├── diabetic_data.csv     # Diabetes 130-US Hospitals (main analysis table)
│   └── IDs_mapping.csv       # Admission / discharge / source ID → label mapping
├── PTO_diabetes.ipynb       # End-to-end PTO workflow (paths assume repo root as cwd)
├── Optimizing Diabetes Care - Data to Action Project.pdf   # Presentation slides (export from PowerPoint if needed)
├── requirements.txt         # Python package pins (includes gurobipy for optimization)
├── Final_Report.pdf         # Full written report (problem, data prep, metrics, optimization, what-ifs, references)
└── README.md                # This file
```

Skip any paths you choose not to upload (e.g. local-only `.pptx` / `Final_Report.md` drafts).

#### Environment

Install dependencies (Gurobi license required for `gurobi`/`gurobipy`):

```bash
pip install -r requirements.txt
```

Open **`PTO_diabetes.ipynb`** and run cells in order **from the repository root** so paths `data/diabetic_data.csv` and `data/IDs_mapping.csv` resolve.

#### Documentation

See **`Final_Report.pdf`** for the problem statement, data preprocessing, metric tables, optimization notation, what-if analyses, and references.

### Optimizing Diabetes Care

*Balancing Intervention Costs and Readmission Reduction*

**Team:** Abhijna Sahadeva (asahadev) · Iris Lee (irislee) · Joanna Chang (joannac2) · Manraj Dhillon (mdhillon)

---

#### Overview

This project supports hospital decision-making under value-based care and programs such as Medicare’s Hospital Readmissions Reduction Program (HRRP). Using the **Diabetes 130-US Hospitals** dataset from the [UCI Machine Learning Repository](https://archive.ics.uci.edu/), it applies a **predict-then-optimize (PTO)** pipeline: machine learning estimates each encounter’s readmission risk, then an optimization model chooses whom to treat with post-discharge interventions (e.g., follow-up, education, remote monitoring) under a fixed budget.

#### Approach

1. **Prediction** - Models compared: Logistic Regression, Random Forest, XGBoost, and HistGradientBoosting. Training uses patient-level grouping (`GroupShuffleSplit`, `StratifiedGroupKFold` with `patient_nbr`) to limit leakage; features are scaled and one-hot encoded. **HistGradientBoosting** was selected (e.g., ROC AUC **0.697**, strong calibration on Log Loss / Brier / average precision versus the other candidates).
2. **Optimization** - A **0–1 knapsack**-style formulation maximizes expected net savings (intervention effectiveness × predicted readmission probability × age-group readmission cost, minus intervention cost) subject to a total intervention budget.

#### Main results (summarized below; full write-up in `Final_Report.pdf`)

- **$1M budget:** about **1,832** encounters selected; projected savings about **$2.2M**; ROI about **2.24**.
- **What-if - double budget ($2M):** more encounters treated (~**3,228**), higher total savings (~**$3.88M**), ROI ~**1.94** (diminishing returns).
- **What-if - 5% lower intervention effectiveness:** same count treated at ~**$1M** spend, but ROI falls (~**2.24** → **1.70**), stressing intervention quality and adherence.

Potential extensions include dynamic budgets, multi-disease optimization, and integration with electronic health record (EHR) systems.

#### Presentation (slides)

**Repository file:**

[Optimizing Diabetes Care – Data to Action Project.pptx](./Optimizing%20Diabetes%20Care%20-%20Data%20to%20Action%20Project.pptx)

Markdown and GitHub’s README viewer **cannot render** an interactive PowerPoint carousel inline. Use one of the options below if you want to **click through the deck in the browser**.

- **Browse in Chrome / Edge / Safari:** clone or download [`presentation/office-viewer.html`](./presentation/office-viewer.html), open it from disk, paste the **Raw** `.pptx` URL copied from GitHub (*open the `.pptx` in the repo → **View raw** → copy address bar*), then choose **Open viewer** - Microsoft’s viewer opens fullscreen so you can move through slides with normal playback controls.

  If you later enable **GitHub Pages** pointing at `presentation/`, you can load the helper from your site URL instead of opening the local file - same flow.

#### Project structure
 
```
.
├── data/
│   ├── diabetic_data.csv    # Diabetes 130-US Hospitals (main analysis table)
│   └── IDs_mapping.csv       # Admission / discharge / source ID → label mapping
├── presentation/
│   └── office-viewer.html   # Builds Office Online slideshow URL from pasted raw .pptx link
├── PTO_diabetes.ipynb        # End-to-end PTO workflow (paths assume repo root as cwd)
├── Optimizing Diabetes Care - Data to Action Project.pptx
├── requirements.txt          # Python package pins (includes gurobipy for optimization)
├── Final_Report.pdf          # Full written report (problem, data prep, metrics, optimization, what-ifs, references)
└── README.md                 # This file
```

Omit rows you are not uploading (e.g. `Final_Report.pdf` or `.pptx` if required elsewhere).

#### Environment

Install dependencies (Gurobi license required for `gurobipy`):

```bash
pip install -r requirements.txt
```

Open **`PTO_diabetes.ipynb`** and run cells in order **from the repository root** so paths `data/diabetic_data.csv` and `data/IDs_mapping.csv` resolve.

#### Documentation

See **`Final_Report.pdf`** for the problem statement, data preprocessing, metric tables, optimization notation, what-if analyses, and references.

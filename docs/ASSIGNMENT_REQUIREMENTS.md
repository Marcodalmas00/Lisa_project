# University assignment — requirements (living document)

This file is the **single source of truth** for what must be built and how work is judged. Every task, design choice, and line of analysis or code should be traceable to something written here (or added here as specifics arrive).

**How to use**

- **Course-wide rules** live in [Course general requirements](#course-general-requirements) below (submission, notebook content, grading, pedagogical expectations).
- **Project-specific rules**—dataset/domain, supervised task formulation, baseline choices—go in [Specific requirements](#specific-requirements) when the project topic is confirmed.
- When something conflicts, **the stricter or more recent explicit requirement wins**; note the conflict under [Decisions & assumptions](#decisions--assumptions).

---

## Guiding principle

> **Requirements first.** Meet the notebook structure and grading criteria; explanations and critical discussion matter at least as much as running models. Prefer clarity and honest limits over flashy but poorly justified results.

---

## Course general requirements (official)

### Deliverable shape

- One **single commented notebook** containing **all** code, satisfying the [Report content](#report-content) and [Evaluation](#evaluation-total--up-to-5-points) sections below.
- Notebook format: either an original **`.ipynb`** file or a **Google Colab sharing link**.
- Submit **everything needed** to execute the notebook, **unless** the dataset is downloaded directly from inside the notebook.
- Supporting repo layout (paths, reproducibility notes) should mirror what will be zipped or referenced at submission time so the marker can run the work reliably.

---

### Submission (Moodle)

- **Who:** each **group** submits one project package on Moodle.
- **Deadline (general rule):** **23:59** local time on **the day before the written exam date** for your session *(confirm your session’s exam date with the course page)*.
- **First deadline (exception):** **23:59 on June 14, 2026** *(fixed in the brief—use unless superseded by an official course update)*.
- **Late work:** **not accepted**; Moodle closes automatically at the cutoff.
- **Presentation:** **not required** for this project.
- **Exam session vs project session:** You may submit the project in **a different exam session** than the written exam **if rules allow**, but **both** the written exam **and** the project must **pass before** the **January/February 2027** exam session window *(per course statement—confirm with coordinators if planning cross-session submissions)*.

---

### Report content

The notebook should include **at least** the following (the brief allows “not limited to” these elements):

- **Data presentation and description**
- **Goals of the project**
- **Exploratory data analysis**
- **Data preparation, feature engineering, and transformation of raw data**
- **Comparison of different learning methods** and, where appropriate, **fine-tuning of parameters**
- **Evaluation using suitable metrics**
- **Qualitative analysis** of results: errors, outliers, or unexpected patterns
- **Discussion of the most critical choices** made while working with the data
- **Conclusions and possible business implications**

---

### Evaluation (total — up to 5 points)

#### Base evaluation — up to **2 points**

Correctness and completeness of the basic pipeline:

- Clear presentation of the **problem** and **dataset**
- Correct **data loading**, **cleaning**, and **preprocessing**
- Correct application of **at least one** suitable machine learning or data analysis method
- Basic **evaluation** and **interpretation** of results
- **Readable, commented, executable** code

#### Advanced evaluation — up to **3 points**

Depth and quality of analysis:

- **Meaningful comparison** of multiple methods or configurations
- **Appropriate** evaluation **metrics** for the selected task
- **Careful analysis** of errors, limitations, anomalies, or misleading results
- **Critical discussion** of methodological choices
- **Quality of visualizations** and clarity of explanations
- Connecting results to plausible **business or managerial implications**

---

### Important notes (pedagogy — affects grading)

Simply running algorithms is **not sufficient** for a high score. A strong submission must explain clearly:

- **What problem** is being solved
- **Why** specific preprocessing steps were applied
- **Why** specific models or methods were selected
- **How** results should be interpreted
- **What the limitations** of the analysis are

**Interpretation policy:** Results that are not immediately obvious are acceptable **if** they are **critically analyzed and discussed**. **Poorly justified** results—even if technically correct—will score lower.

Strong work demonstrates **critical thinking** and **awareness of practical implications** of data-driven decisions, not only technical correctness.

---

## Operational norms (workflow)

These complement the official criteria; default to institutional rules where they diverge.

### Academic integrity

- Follow the university’s policy on originality, plagiarism, collaboration, and use of tools (including AI).
- Attribute external code, tutorials, datasets, and libraries as required by the course handbook.
- Submission is **per group**; document **roles / who did what** when the brief or instructor asks for it.

### Reproducibility and quality bar

- Pin or note environment/versions when markers need to reproduce results.
- When requirements are ambiguous, record assumptions under [Decisions & assumptions](#decisions--assumptions) before building on them.

---

## Current professor notebook baseline
Based on notebooks currently stored in `resources/professor_notebook`. This is a **reference baseline**, not a hard restriction: other libraries/methods are allowed when justified.
### Notebooks currently available
- `data_engineering.ipynb`
- `clustering_notebook.ipynb`
- `dimensionality_reduction_master_notebook.ipynb`
### Core libraries and themes observed
- Data stack: `pandas`, `numpy`
- Visualization: `matplotlib`, `seaborn`
- Main ML toolkit: `scikit-learn`
- Supporting scientific stack: `scipy`
- Clustering utilities/visual diagnostics: `yellowbrick`
- Dimensionality reduction: `PCA`, `t-SNE`, `UMAP`
- Clustering methods: `KMeans`, `AgglomerativeClustering`, `DBSCAN`
- Feature engineering/selection and preprocessing: encoders, scaling/normalization, `SelectKBest`, `RFE`
- NLP/text representations shown: `TfidfVectorizer`, `gensim` embeddings, `transformers`/`torch` examples
### Usage rule for this project
- Treat the above as the course baseline to align style and method choices.
- You may use additional tools beyond this baseline, but each non-baseline choice should include a concise justification in the notebook (why chosen, expected benefit, and any trade-off).
---

## Specific requirements

### Track selection

- **Selected track:** Track 2 — **Credit Card Fraud Detection**
- **Dataset source:** Kaggle `mlg-ulb/creditcardfraud`  
  <https://www.kaggle.com/datasets/mlg-ulb/creditcardfraud>
- **Task type:** binary classification
- **Prediction goal:** predict whether a transaction is **fraudulent** (`1`) or **legitimate** (`0`)

### Task constraints from brief

- Treat class imbalance as a central modeling issue (fraud is rare).
- Compare **at least two** classification models.
- Do **not** rely only on accuracy.
- Use suitable metrics such as precision, recall, F1, ROC-AUC, PR-AUC.
- Discuss business impact of **false positives** and **false negatives**.

### Required analysis focus for this track

- Quantify target imbalance early (class ratio, baseline prevalence).
- Build a business-aware evaluation framing:
  - false negatives: missed fraud losses/reputational risk
  - false positives: customer friction, operational review cost
- Use threshold-aware analysis (not just default `0.5`) and justify operating point choices.
- Include detailed error analysis of misclassified transactions and uncertain-score regions.

### Minimum modeling baseline (aligned with course notebook style)

- Start with a transparent baseline (e.g., logistic regression).
- Add at least one stronger non-linear model (e.g., tree-based ensemble).
- Compare models with consistent split strategy and identical metric suite.
- If sampling/class-weighting/calibration is applied, justify why and report effect on precision-recall trade-offs.

### Planned 3-model comparison strategy

- **Model 1 (base / interpretable):** `LogisticRegression`  
  Why: simple, transparent baseline; helps show what imbalance handling and threshold tuning add.
- **Model 2 (course-aligned strong model):** `RandomForestClassifier` *(or another tree ensemble explicitly covered in course material if later notebooks indicate a different standard)*  
  Why: robust non-linear baseline commonly used in course-style ML workflows.
- **Model 3 (advanced / beyond core course):** gradient boosting implementation such as `XGBoost` (`xgboost`) or `LightGBM` (`lightgbm`)  
  Why: often stronger on tabular fraud data; included as an intentional extension beyond taught baseline.

**Justification rule for Model 3:** explicitly state that it is beyond the current core course baseline, motivate expected gain (handling non-linearity/interaction patterns in imbalanced tabular data), and report complexity/cost trade-offs vs course-standard models.

**Fair-comparison rule:** use the same train/validation/test protocol, preprocessing policy, and evaluation metrics across all three models; discuss both ranking metrics (ROC-AUC/PR-AUC) and operating-point metrics (precision/recall/F1 at chosen threshold).

### "Do it better than typical AI submissions" standards

- Every major choice must include a short **why** (data prep, model, metric, threshold).
- Prefer reproducible comparisons over one-off best scores.
- Emphasize interpretable plots/tables that support claims.
- Include a limitations section with concrete failure modes and next-step improvements.
- Keep comments and markdown explanations rich enough to show learning process, not only final outputs.

### Data location (local project)

- Expected notebook-ready dataset path should be documented when confirmed (CSV not yet detected in repository scan at edit time).
- If data is loaded externally, notebook must include explicit and reproducible download/load steps.

**Deadlines:** see [Submission (Moodle)](#submission-moodle)—session-specific cutoff is **day before exam** except the **June 14, 2026** first deadline.

---

## Decisions & assumptions

| Date | Decision or assumption | Rationale |
|------|------------------------|-----------|
| 2026-05-05 | Kaggle discussion pages may require interactive/authenticated access; direct automated extraction is limited. | Avoid inventing claims; only cite discussion insights once specific posts are provided or manually accessible. |

---

## Change log

| Date | Change |
|------|--------|
| 2026-05-05 | Initial structure and placeholders. |
| 2026-05-05 | Incorporated official notebook sections, base/advanced evaluation (5 pts), important notes; clarified deliverable as notebook plus reproducibility. |
| 2026-05-05 | Moodle submission: deadlines (incl. June 14, 2026 exception), `.ipynb` or Colab, bundle files vs download-in-notebook, no late submissions, no presentation, exam/session note to Jan/Feb 2027. |
| 2026-05-05 | Added current professor-notebook baseline (available notebooks, taught libraries/methods, and rule for justified extension beyond baseline). |
| 2026-05-05 | Added Track 2 specific requirements: fraud objective, imbalance/cost-sensitive evaluation, mandatory model comparison, threshold and error-analysis emphasis, and stronger-quality execution standards. |
| 2026-05-05 | Added planned 3-model strategy: interpretable baseline, course-aligned strong model, and advanced out-of-course model with explicit justification and fair-comparison protocol. |
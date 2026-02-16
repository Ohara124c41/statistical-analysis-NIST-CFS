# Statistical Analysis Workflow (P2)

## Project Description
This project builds a complete, reproducible statistical analysis workflow using a combined public NIST/Data.gov cybersecurity mapping dataset (AMI, DER, DGM). The notebook covers ingestion, descriptive statistics, visualization, hypothesis testing, and extended robustness checks. The goal is to establish a rigorous statistical foundation for later ML/DL/agentic security analytics.

## Project Repository
- https://github.com/Ohara124c41/statistical-analysis-NIST-CFS

## What I Built
- `analysis.ipynb`: end-to-end statistical analysis notebook
- `requirements.txt`: Python dependencies for reproducible execution
- `Statistical_Analysis_Report.pdf`: written report with citations

## Dataset
- Name: Mapping of NIST Cybersecurity Framework Subcategories to NESCOR Threat Scenarios (AMI/DER/DGM)
- Link: https://catalog.data.gov/dataset/mapping-of-nist-cybersecurity-framework-subcategories-to-threat-scenarios-from-the-nationa-e8b41

## How To Run
1. Create and activate a virtual environment (WSL/Ubuntu example):
```bash
python3 -m venv .venv
source .venv/bin/activate
```
2. Install dependencies:
```bash
pip install -r requirements.txt
```
3. Launch Jupyter:
```bash
jupyter notebook
```
4. Open and run:
`analysis.ipynb`

## Reproducibility Notes
- The notebook combines AMI/DER/DGM files into `data/combined_nescor_csf.csv`.
- If URL download is blocked, place source files at:
  - `data/raw/ami.csv`
  - `data/raw/der.csv`
  - `data/raw/dgm.csv`
- To refresh dependency locking before submission:
```bash
pip freeze > requirements.txt
```

## Reflection (Rubric Items)
### Bias Awareness
Poor data handling can introduce bias in this dataset because it is a curated mapping resource with domain-specific structure, not an operational incident stream. If missing or unmatched function labels are treated as random noise, cross-domain comparisons can be distorted. Domain counts are also uneven (AMI = 191, DGM = 166, DER = 145), so interpretation should be paired with effect sizes and corrected comparisons, not raw counts alone. The notebook mitigates this by preserving provenance (`domain`, `source_file`), reporting Cramer's V, and using Holm-corrected pairwise tests.

### Future Integration: ML Workflow Changes
For downstream ML, workflow changes should include domain-aware validation and explicit handling of sparse/rare labels. Because the data has long-tail category behavior, train/test splits should preserve rare-category coverage and avoid leakage from preprocessing decisions performed on the full dataset. Model evaluation should include macro-level metrics and per-domain breakdowns so strong performance on frequent labels does not mask weak behavior on rare subcategories.

### Future Integration: Neural Network Preparation
Neural-network preparation should focus on mixed-type feature engineering: categorical encoding for CSF structures and text representations for mitigation/description fields. The long-tail subcategory distribution suggests class-weighting, focal losses, or hierarchical label strategies may be needed in supervised settings. Preprocessing should be versioned and reproducible so tokenization, label mapping, and filtering rules stay consistent across experiments.

### Future Integration: Agentic Automation Potential
Agentic automation can operationalize this workflow by monitoring shifts in domain composition, missing-label rates, and long-tail category concentration over time. Agents can run scheduled data quality checks, recompute statistical diagnostics, and generate governance alerts when control-coverage patterns materially drift. To keep automation trustworthy, all decisions should remain auditable through versioned configs, pinned environments, and Git-tracked outputs.

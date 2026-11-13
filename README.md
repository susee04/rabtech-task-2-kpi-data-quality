# Rabtech Academy — Task 2: KPI Dictionary & Data Quality Contract

## Objective
Translate the retail-orders business request into measurable KPIs and a testable data-quality agreement.

## Deliverables
- `KPI_Dictionary.xlsx` — 10 KPIs with definitions, formulas, grain, filters, owners, and refresh cadence.
- `data_profiling.ipynb` — executable Python notebook for completeness, uniqueness, validity, consistency, and freshness checks.
- `data_quality_contract.md` — quality rules, thresholds, escalation actions, KPI publication rules, and sign-off roles.
- `data/retail-orders-raw.csv` — original supplied sample data (preserved unchanged).
- `data/retail-data-dictionary.csv` — supplied field definitions and quality rules.

## How to run the notebook
1. Open this repository in GitHub.
2. Download or clone the repository.
3. Open `data_profiling.ipynb` in Jupyter Notebook / JupyterLab / Google Colab.
4. Ensure both CSV files remain inside the `data/` folder.
5. Run all cells from top to bottom.

Required Python packages: `pandas`, `numpy`, and `jupyter`.

## Dataset notes
The sample contains 12 rows and intentionally includes data-quality exceptions. The notebook reports these issues; it does not silently alter the raw source. Missing discount values require business-owner approval before being treated as zero.

## Submission
Submit this repository's **public GitHub URL** as the single deliverable link in the Rabtech Academy task portal.

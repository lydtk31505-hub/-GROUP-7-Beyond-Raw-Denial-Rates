# Beyond Raw Denial Rates: Do Geographic Gaps in Mortgage Denials Persist?
Group 7 - Big Data Analysis Final Project (2024 HMDA public LAR)

## Data
2024 HMDA public Loan/Application Register (FFIEC/CFPB, https://ffiec.cfpb.gov/data-publication/),
instructor-provided file `2024_public_lar_one_year_csv.zip` (12,259,199 records, 99 variables, 4.4 GB CSV).
The raw data is not included in this repository.

## Environment
Google Colab, Python 3, PySpark 4.0.5 (local mode, driver memory 8g), pandas, pyarrow, scikit-learn, matplotlib, plotly (see `requirements.txt`).
All notebooks use the shared Google Drive folder `hmda`. Run each notebook with *Runtime -> Restart session and run all*, in the order below.

## Notebooks (run in this order)
| # | Notebook | Author | Input | Main output |
|---|---|---|---|---|
| 0 | `00_ingest_csv_to_parquet.ipynb` | Ly | raw zip | `hmda_parquet/`, `results/dataset_facts.json` |
| 1a | `01_clean_pandas.ipynb` | Ly | `hmda_parquet/` | `hmda_clean_v1/hmda_clean_v1.parquet`, `cleaning_log.json` (3,140,857 records) |
| 1b | `01_clean_pyspark.ipynb` | Ly | `hmda_parquet/` | `hmda_clean_v1_spark/`, `results/cleaning_log_spark.json` (Spark = Pandas check) |
| 2 | `02_rq1_pandas.ipynb` | Ly | `hmda_clean_v1` | RQ1 tables and figures: `results/rq1_*.csv`, `results/figures/rq1_*.png` |
| 3 | `03_benchmark.ipynb` | Ly | `hmda_clean_v1` | the group's benchmark (Pandas vs PySpark, 5 runs, 25/50/100%): `results/benchmark_*.csv`, `results/figures/benchmark.png` |
| 4 | `04_rq2_pandas.ipynb` | Mai Anh | `hmda_clean_v1` | RQ2 tables and figures: `results/rq1_rq2_pandas/` (37 tables, 17 figures) |
| 5 | `05_rq2_pyspark_check.ipynb` | Mai Anh | `hmda_clean_v1_spark`, results of 04 | `results/rq2_spark/` (data checks 24/24, 37/37 tables identical to Pandas); run after 04 |
| 6 | `06_rq3_diagnostic.ipynb` | Linh Anh | `hmda_clean_v1` | RQ3: standardized state gaps, bootstrap CI, model-based and LEI sensitivity, Pandas vs PySpark check: `results/rq3/` |
| 7 | `07_rq4_predictive.ipynb` | Linh Anh | `hmda_clean_v1` | RQ4: ROC-AUC gain of adding state (full test and 50 states + DC), scikit-learn vs Spark MLlib: `results/rq4/` (train/test splits and held-out predictions stay in local Colab storage, `/content/rq4_shared_splits`) |

`cleaning_rules.md` documents every cleaning rule (R1-R12) with the number of records affected.

## Conventions shared by all notebooks
- Scope: 3,140,857 home-purchase applications (276,604 denied, 8.81%).
- Reference rate for gaps: the pooled rate of the units being compared. **8.62%** (50 states + DC, 3,119,941 records) for states, the funnel plot, state gaps and state-level correlations (RQ1-RQ3); **8.81%** (all records) for loan type, metro status, region and MSA.
- Wilson 95% confidence intervals. Display thresholds: groups n >= 100, two-way cells n >= 500, MSAs n >= 5,000. Territories (PR, VI, GU, AS, MP) and records without a state are reported separately from the state comparisons.
- Correlations across the 51 states: Spearman is the main number; Pearson and weighted Pearson are secondary.
- All rates in RQ1 and RQ2 are raw (unadjusted) and descriptive. Adjusted gaps are in RQ3. No result is interpreted as causal.
- Colors: the Group 7 color system (red-orange = above the reference or denied, amber = below or comparison, grey = no difference).

## Cleaning principles
Invalid values are flagged and set to missing; records are never dropped because of one bad variable.
The PySpark and Pandas versions produce identical counts (means agree to a relative tolerance of 1e-9).

## Ethics
Race, ethnicity, sex and age are never used as model features; they are kept only as `val_*` columns for validation.

# Group 7 — RQ3 output definitions

- Cohort: 50 U.S. states + Washington, D.C. (in-scope 2024 HMDA applications). Territories and unknown state codes excluded from state-reference analyses.
- Raw geographic benchmark: combined denial rate from that cohort; recalculated from the cleaned dataset (approximately 8.62%).
- Focus-state rule: five highest and five lowest raw state denial rates among the 42 eligible states. DC is in the *reference population* but does not meet the ranking threshold.
- Primary figure AB: Panel A uses the full-cohort raw reference. Panel B uses a separate national reference conditional on identical shared-support strata; Panel B raw/adjusted values alone can be compared as a before/after adjustment.
- State-specific CSVs: adjusted gaps can have different national reference rates across jurisdictions, and MUST NOT be ranked as directly comparable; review_flags indicate low decisions and/or less than 50% national-weight support. The 50% flag is a reporting heuristic, not a statistical test.
- CLTV sensitivity: additional stratum dimension with Missing retained; compared against the state-specific main estimate, not the 10-state shared-support estimate.
- Model-based robustness: original full-distribution model estimates AND aligned shared-support model estimates are separately exported. Differences between estimators have no model-based CI and are not causal.
- Lender mix: comparison is restricted to cross-jurisdiction LEIs with observed overlap; sign reversal does not imply statistical significance. No lender-mix contrast CI was calculated.
- Bootstrap: 500 repetitions for direct adjusted gaps, conditional on the fixed selected focus states and fixed shared-support strata; selection uncertainty is not included.
- Correctness: Pandas and Spark use the same cleaned input and agreed pooling rule. Both state-specific and main shared-support gaps are checked.
- Supplementary Figure CD is exploratory. Figures are exported at 300 DPI with Group 7 colors.

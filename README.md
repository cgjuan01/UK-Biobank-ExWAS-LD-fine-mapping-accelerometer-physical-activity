This repository provides a one-file pipeline (ukb_wes_exwas_vig_vs_sed.py) to run an Exome-Wide Association Study (ExWAS) in UK Biobank OQFE WES data, explicitly modeling vigorous activity vs sedentary activity:

What it does:
Builds a .phe file (PLINK/REGENIE-ready):
loads your participant table (CSV/Parquet) with eid, activity columns, and covariates
optionally derives season/quarter from a date column
transforms two activity traits independently (normalize and/or invert):


Outcome (phenotype): e.g., vigorous_activity or sedentary_activity
Adjuster (covariate): the other activity (included in covariates)
harmonizes IDs with the WES .fam and writes pa_wes_vig_vs_sed.phe

Runs ExWAS (optional):

PLINK 2 per-chromosome --glm on exome variants
REGENIE step 1 (array) + step 2 (WES) for quantitative traits

Inputs
Participant table: CSV/Parquet with eid, activity columns (e.g., vigorous_activity, sedentary_activity or UKB field IDs), and covariates.
WES .fam from UKB OQFE PLINK (ukb23158_c{CHR}_b0_v1.fam).

(For REGENIE) Array PLINK prefix for step 1 (genome-wide markers).
Outputs
.phe (TSV) with FID, IID, transformed outcome column, transformed adjuster column, optional wear_quarter, and your base covariates.

PLINK 2: per-chromosome *.glm.linear + combined results (you can combine downstream).

REGENIE: step1 predictions + per-chr step2 outputs.

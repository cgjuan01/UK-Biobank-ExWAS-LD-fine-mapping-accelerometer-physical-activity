This repository provides a one-file pipeline to run an Exome-Wide Association Study (ExWAS) in UK Biobank OQFE WES data, explicitly modelling vigorous activity vs sedentary activity, and an optional LD fine-mapping stage using your WES genotypes.

What it does
1) Phenotype build (PLINK/REGENIE-ready)

Loads your participant table (CSV/Parquet) with eid, activity columns, and covariates.

Optionally derives season/quarter from a date column.

Independently transforms two activity traits (normalize and/or inverse-normal).

Outcome (phenotype): e.g., vigorous_activity or sedentary_activity

Adjuster (covariate): the other activity (included in covariates)

Harmonises IDs with the WES .fam and writes: pa_wes_vig_vs_sed.phe

2) ExWAS (optional; choose PLINK 2 or REGENIE)

PLINK 2: per-chromosome --glm on exome variants

REGENIE: step 1 (array) + step 2 (WES) for quantitative traits

3) LD Fine-Mapping (optional; WES LD)

Lead variant selection: clumps association results (PLINK2 --clump) or uses a P-value threshold

Per-locus windows: builds ±Kbp windows around leads and extracts variants from WES PLINK data

LD computation: uses PLINK2 --r square on the extracted region to produce a correlation matrix (LD)

Fine-mapping inputs: writes FINEMAP .z, .ld, .snp, .master files (allele-consistent with the LD set)

Rerunnable fine-mappers:

FINEMAP (if --finemap-exe is provided)

SuSiE via a small helper R script (if --run-susie) using the same LD + z-scores

Produces a per-locus manifest and (optionally) PIP/credible set files from SuSiE / FINEMAP outputs

Inputs

Participant table: CSV/Parquet with eid, activity columns (e.g., vigorous_activity, sedentary_activity or UKB field IDs), and covariates

WES PLINK OQFE: ukb23158_c{CHR}_b0_v1.* (or equivalent)

(REGENIE) Array PLINK prefix for step 1 (genome-wide markers)

Outputs

.phe (TSV) with FID, IID, transformed outcome, transformed adjuster, optional wear_quarter, and base covariates

PLINK 2: per-chr *.glm.linear + combined table (downstream merge)

REGENIE: step 1 predictions + per-chr step 2 outputs

LD Fine-Mapping: per-locus directory containing:

finemap.z, finemap.ld, finemap.snp, finemap.master

(optional) *.config, *.cred, *.log from FINEMAP

(optional) *.susie.pip.tsv + RDS fits/CS from SuSiE

loci_manifest.tsv

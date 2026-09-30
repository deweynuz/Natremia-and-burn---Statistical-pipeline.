# Dysnatremia in critically ill burn patients — analysis pipeline (revision R1)

Code accompanying the manuscript *Dysnatremia in critically ill burn patients: incidence, timing and association with kidney outcomes* (POOF cohort, Saint-Louis burn ICU, 2011–2023).

The repository contains the analysis notebooks, the figures and the aggregate results. **No individual patient data are included**: the raw extracts and the patient-level intermediate files are excluded (see `.gitignore`). The notebooks can therefore be read but not re-executed without access to the source data.

## Notebooks (run in this order)

| Notebook | Input | Output |
|---|---|---|
| `01_Data_Cleaning_Preparation_R1.ipynb` | raw extracts (`../../Pipeline_analyse/data/raw`, not provided) | `outputs/dffinal_clean.xlsx` (1 row / patient), `outputs/episodes_long.xlsx`, `outputs/hypo_episode_full.xlsx`, `outputs/hyper_episode_full.xlsx`, `outputs/natremia_measurements_long.csv` — all patient-level, **not versioned** |
| `02_Figures_Analyses_R1.ipynb` | `outputs/` | Table 1 (`outputs/Table1_5_colonnes_regenere.xlsx`), Table 2 (time-dependent Cox), Figures 1–4, Supplementary Figures 3–9, 11, 13–14, Supplementary Tables 4 and 7 (`outputs/table_clusters_extended.xlsx`, `outputs/timing_dysnatremia_vs_aki_table.xlsx`), figures in `figures/` |
| `03_Revision_Analyses_R1.ipynb` | `outputs/` | analyses added for the revision: competing risks, landmark, extended adjustment, E-values, overlap weighting, bTBSA strata, AKI time-varying Cox, direct/indirect ISE agreement; results in `outputs/results/*.json`, figures `FigR1`–`FigR4` (Supplementary Figures 12, 10, 2 and 1), Supplementary Tables 1, 2, 3 and 5 |

Corrections made during the revision are marked `# R1-FIX` in the code.

## Definitions implemented in the code

- Hyponatremia < 135 mmol/L, hypernatremia > 145 mmol/L; sodium values from the central laboratory (indirect ISE) and blood-gas analyser (direct ISE) pooled, restricted to the ICU stay (day before admission → discharge); values excluded when same-day glucose ≥ 10 mmol/L or triglycerides ≥ 2 g/L, otherwise corrected for glucose (Katz).
- AKI: KDIGO creatinine criterion; baseline creatinine = admission value if CKD-EPI eGFR ≥ 75 mL/min/1.73 m² or known CKD, otherwise back-calculated (MDRD, eGFR 75).
- MAKE at discharge (MAKE90): death, KRT or creatinine ≥ 150 % of baseline within 90 days of ICU admission.
- Main model: time-dependent Cox (lifelines `CoxTimeVaryingFitter`), follow-up from burn injury, censored at 90 days, adjusted for age and bTBSA.
- Clustering: k-means (numpy, k-means++, 10 initialisations, seed 42) on standardised baseline predictors only; Ward hierarchical clustering as sensitivity analysis.

## Figure files ↔ manuscript items (revised version)

| File (`figures/`) | Manuscript item | Notebook cell |
|---|---|---|
| `figure1_incidence.png` | Figure 1 | 02 |
| `figure2_aki_make_v2.png` | Figure 2 | 02 (section 14) |
| `figure3_aki.png` | Figure 3 | 02 (section 8) |
| `figure5_auc.png` | Figure 4 | 02 (section 13) |
| `FigR4_ISE_direct_indirect_agreement.png` | Supplementary Figure 1 | 03 |
| `FigR3_DAG.png` | Supplementary Figure 2 | 03 |
| `suppfig1_flowchart.png` | Supplementary Figure 3 | 02 |
| `suppfig2_natremia_distribution.png` | Supplementary Figure 4 | 02 |
| `suppfig3_onsets.png` | Supplementary Figure 5 | 02 |
| `suppfig4_hyponatremia_flowchart.png` | Supplementary Figure 6 | 02 |
| `suppfig5_hypernatremia_duration.png` | Supplementary Figure 7 | 02 |
| `suppfig6_vascular_filling.png` | Supplementary Figure 8 | 02 |
| `figure2_mortality.png` | Supplementary Figure 9 | 02 |
| `FigR2_forest_sensitivity_analyses.png` | Supplementary Figure 10 | 03 |
| `suppfig8_naadmission_make.png` | Supplementary Figure 11 | 02 |
| `FigR1_cumulative_incidence_competing_risks.png` | Supplementary Figure 12 | 03 |
| `suppfig10_cluster_selection.png` | Supplementary Figure 13 | 02 |
| `suppfig11_hierarchical_sensitivity.png` | Supplementary Figure 14 | 02 |
| `figure4_clusters.png`, `figure2_make.png`, `figure_timing_aki_dysnatremia.png` | not used in the revised manuscript (kept for reference) | 02 |

Supplementary Tables: 1 (ISE sensitivity), 2 (admission logistic models, bTBSA continuous/categories), 3 (MAKE sensitivity analyses) and 5 (cause-specific / Fine–Gray) come from `03` (`outputs/results/*.json`); 4 (timing) and 7 (clusters) from `02` (`outputs/*.xlsx`); 6 lists the clustering predictors (`02`, section 13).

## Environment

Python ≥ 3.10; packages in `requirements.txt`. Notebooks were executed end-to-end (pandas 2.x, lifelines, statsmodels) with no error; all results are deterministic (fixed seeds).

# Extraction codebook — AIBIO_GU

Wide-form CSV, one row per `record_id` FT Included (n=400). Values/notes
in English (manuscript language). `NR` = not reported; `NA` = not
applicable.

Review: *Artificial Intelligence–Driven Biomarker Discovery in
Genitourinary Oncology* (`integrative_review`, `SR-AIBIO-GU`). Minimum
fields per `protocol/protocol.md` §Extraction.

Cohort: `extraction/AIBIO_GU_ft_included_extraction_cohort_2026-09-23.csv`
(400 records, with `pdf_path` and already-closed appraisal fields
—`mmat_overall`, `applies_prediction_appraisal`,
`probast_overall_rob`, `appraisal_note`— carried as context, without
being re-judged here).

## Core rules

1. Numeric fields: plain numbers (no units or `%` in numeric columns;
   percentages go in `*_pct` fields as numbers 0–100).
2. A study may report **more than one** biomarker/model; if there are
   several clearly distinct axes (e.g. a genomic signature + a radiomic
   signature in the same paper), extract the **main** axis declared by the
   authors (the one anchoring the title/abstract) and note the secondary
   ones in `other_key_outcomes`. Do not split into rows.
3. `extraction_status`: `complete` | `partial` | `needs_review`.
4. `needs_review`: `yes` | `no` — mark `yes` for scope ambiguity,
   insufficient text, or possible cohort overlap with another record in
   the corpus (use the inherited `appraisal_note` as a clue).

## Minimum for `complete`

`study_label`, `tumor_site`, `study_design`, `data_source`, `n_analytic`,
`data_modality`, `ai_task`, `biomarker_result`, `clinical_purpose`,
`performance_metric` + `performance_value` (or justified `NR`),
`validation_level`.

## Column dictionary

| Column | Meaning / controlled vocabulary |
|---|---|
| record_id | Canonical ID |
| study_label | AuthorYear |
| tumor_site | `prostate` \| `bladder_urothelial` \| `kidney_renal` \| `penile` \| `testicular_gct` \| `multiple` (list in `tumor_site_detail`) \| `other` |
| tumor_site_detail | Free text if `multiple`/`other` |
| study_design | Free text (`retrospective cohort`, `prospective cohort`, `case-control`, `cross-sectional`, `translational/bioinformatic`, etc.) |
| data_source | `own_cohort` \| `TCGA` \| `CPTAC` \| `GEO` \| `other_public` \| `multiple_public` \| `public_plus_own` |
| data_source_detail | Name(s) of cohort/dataset(s), country/center if applicable |
| n_analytic | Analytic N (patients or samples used in the final analysis) |
| n_unit | `patients` \| `samples` \| `slides` \| `images` \| `other` |
| data_modality | `digital_pathology` \| `radiology_imaging` \| `genomics` \| `transcriptomics` \| `proteomics` \| `epigenomics_methylation` \| `multiomics` \| `other` |
| data_modality_detail | Free text (e.g. `WSI H&E`, `mpMRI`, `scRNA-seq`, `WGBS`) |
| ai_task | `biomarker_discovery_association` \| `classification_subtyping` \| `prognostic_prediction` \| `diagnostic_prediction` \| `treatment_response_prediction` \| `risk_stratification` \| `feature_selection_only` \| `other` |
| algorithm | Free text, list of main algorithms/architectures (e.g. `LASSO Cox, random forest, XGBoost`; `CNN (ResNet50)`) |
| input_features | Brief summary of the input space (e.g. `12-gene expression signature`, `WSI tile-level deep features`, `radiomic texture features from T2WI/ADC`) |
| biomarker_result | Resulting biomarker/signature, named as reported by the authors |
| clinical_purpose | `diagnostic` \| `prognostic` \| `predictive_response` \| `risk_stratification` \| `subtyping` \| `other` |
| performance_metric | `AUC` \| `C-index` \| `HR` \| `sensitivity_specificity` \| `accuracy` \| `other` \| `NR` |
| performance_value | Numeric value(s) or brief summary as reported (free text if there are several) |
| validation_level | `internal_holdout` \| `internal_cv` \| `internal_split_only` \| `external_cohort` \| `external_multicenter` \| `clinical_utility_demonstrated` \| `none` |
| validation_detail | Brief note (e.g. `70/30 split of same TCGA cohort, no external set`) |
| code_availability | `yes` \| `no` \| `upon_request` \| `NR` |
| data_availability | `yes` \| `no` \| `upon_request` \| `NR` |
| reporting_guideline | `TRIPOD-AI` \| `TRIPOD` \| `CLAIM` \| `DECIDE-AI` \| `STARD` \| `other` \| `none_stated` |
| funding | Funding source (free text; `NR` if not declared) |
| conflicts_of_interest | COI declaration (free text; `none_declared` \| `NR`) |
| key_finding | Main finding in 1–2 sentences (performance + declared clinical relevance) |
| limitations_as_stated | Limitations declared by the authors (brief free text) |
| other_key_outcomes | Secondary axes (other biomarkers/models in the same paper), context notes |
| extractor_id | Extractor |
| extracted_at | Date `YYYY-MM-DD` |
| extraction_status | Controlled (see above) |
| extraction_note | Extractor notes/caveats |
| needs_review | `yes` \| `no` |
| pilot_batch | `pilot` \| blank (scale-up) \| batch id (`batch-01`…) |

## Scope notes (inherited from protocol/appraisal)

- The `appraisal_note` entries for 251/400 rows (overfitting/implausible
  AUC, possible cohort overlap between twin papers, preprints not
  peer-reviewed, "external" validation that is actually an internal
  split) are relevant here: when extracting `validation_level`, describe
  the actual design (e.g. internal split of the same TCGA) instead of
  accepting the paper's "external" label if the appraisal already
  questioned it.
- Possible overlap of public cohorts (same TCGA/GEO reused across several
  "twin" papers): document in `other_key_outcomes` /
  `extraction_note` when evident from matching `data_source_detail`
  + same tumor site + same year — input for the synthesis
  (not excluded or merged at extraction).

## Operational process

1. **Pilot (n=20):** closed 2026-09-23 — see
   `AIBIO_GU_extraction_PILOT_SUMMARY.md`. Calibration adopted below.
2. **Scale-up:** parallel batches (FT R1 / appraisal pattern) over the 400,
   codebook frozen after the pilot.
3. Extraction assisted by the full PDF (`screening/full_text/`) by
   default — do not rely on the pilot's partial `.txt` (several cut off
   before Funding/COI/Data-availability).
4. Import to SQLite (`extractions`, one row per variable) upon closing,
   via a script analogous to `import_aibio_gu_appraisal.py`.

### Post-pilot calibration (frozen)

1. **`n_analytic` in multi-stage designs** (e.g. scRNA-seq + bulk cohort,
   or single-cell meta-atlas alongside dozens of bulk cohorts):
   report the N of the stage that directly supports the
   `performance_value` of the main axis; the other stages are
   documented in `data_source_detail`, not in `n_analytic`.
2. **`validation_level`**: `internal_holdout` = single train/test split
   without explicit CV; `internal_cv` = explicit cross-validation
   (k-fold, LOOCV, etc.), even if on a single cohort.
   `validation_detail` must explicitly note when the "external"
   cohort (`external_cohort`/`external_multicenter`) does **not
   cover** the GU tumor subgroup of interest for the study (genuine
   external validation but irrelevant to the extracted GU axis) — in
   that case downgrade `validation_level` to the real level that
   applies to the GU subgroup (e.g. `internal_split_only` if no other
   evidence remains).
3. **Pan-cancer/pan-disease studies** where the GU tumor is a minority
   subset of the paper's corpus (e.g. comorbidity with another disease,
   pan-cancer biomarker/microbiome panel):
   `tumor_site=other`, with the actual scope (GU site(s) covered +
   pan-cancer context) described in `tumor_site_detail`.
   `data_source=public_plus_own` is reserved for when the study's own
   data are part of the model's training/validation; if they are only
   orthogonal validation (IHC/qPCR of a finding already derived from
   public data), `data_source` reflects the model's source
   (`TCGA`/`other_public`/etc.) and the orthogonal validation is noted
   in `validation_detail`.
4. **Co-primary metrics**: when the study reports a model-selection
   metric (e.g. C-index to choose among dozens of algorithm×dataset
   combinations) and a final per-cohort performance metric (e.g. AUC),
   `performance_metric` prioritizes the selection metric;
   `performance_value` may list both, separated by `;`, with brief
   cohort/context.
5. **`ai_task`/`clinical_purpose` with a non-AI anchor axis**: in papers
   whose title/central argument is mechanistic or toxicological and the
   AI/ML component is secondary and tangential, still extract the AI
   component under the review's scope (do not exclude — it already
   passed FT), and document in `extraction_note` that the paper's actual
   anchor is something else; `needs_review=yes` in these cases.
6. Always extract against the full PDF when the partial `.txt` does not
   cover Funding/COI/Data-availability, rather than defaulting to `NR`.

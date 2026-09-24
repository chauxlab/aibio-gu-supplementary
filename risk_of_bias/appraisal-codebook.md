# Appraisal codebook — AIBIO_GU

Protocol instruments (`protocol/protocol.md`):

1. **MMAT v.2018** — primary appraisal (design heterogeneity).
2. **PROBAST** (domains) + **TRIPOD-AI** (reporting adherence, reduced
   form) — complement applied **only** when the study develops or
   validates a predictive model / risk signature / classifier with a
   clinical outcome.

Cohort: the **400** `Included` from the FT consensus
(`screening/conflicts/2026-09-21__consenso-FT-final-436.csv`).

Operational template: wide CSV in this directory. Import to SQLite
(`risk_of_bias`) upon closing the appraisal (rows per domain).

---

## 1. MMAT v.2018

Same logic as `reviews/HPV_PSCC/risk_of_bias/HPV_PSCC_mmat_codebook.md`.

### Categories (`mmat_category`)

| Code | When |
|---|---|
| `qualitative` | Qualitative designs |
| `rct` | Randomized controlled trials |
| `non_randomized` | Cohort / case-control / non-randomized comparative |
| `descriptive` | Case series / prevalence / single-arm descriptive |
| `mixed_methods` | Mixed methods |
| `NA` | Review, commentary, methods-only without primary data (should not remain in FT Included; if it appears → document) |

### Screening (empirical)

- `screen_clear_question`: `yes` \| `no`
- `screen_data_address_question`: `yes` \| `no`

If either is `no` → do not complete `c1`–`c5`; `overall_judgment=cannot_appraise`.

### Criteria `c1`–`c5`

Judgments: `yes` \| `no` \| `cant_tell`.
Brief rationale in `rationale_c1`…`rationale_c5`.
Meaning per category: Hong et al., MMAT 2018.

### Overall MMAT (`mmat_overall`)

`high` \| `moderate` \| `low` \| `cannot_appraise` \| `NA`

Heuristic (empirical studies), applied **mechanically** by counting
`c1`–`c5` (rule frozen after detecting an inconsistency between scale-up
batches — see `AIBIO_GU_appraisal_PILOT_SUMMARY.md` §Post-scale-up
correction):

- `high`: 0 criteria `no`/`cant_tell` (all `yes`)
- `moderate`: exactly 1 criterion `no`/`cant_tell`
- `low`: 2 or more criteria `no`/`cant_tell`
- `cannot_appraise`: failed screening or insufficient text

The 10 `high` cases in the final corpus (0 no/cant_tell, all `yes`) are the
designs of highest methodological rigor (e.g. REC-AIBIOGU-000032,
REC-AIBIOGU-000232: genuine multicenter external validation) — they are not
exceptions to the rule, but rather the natural result of the count applied
to exceptionally well-designed studies. `appraisal_note` documents the
qualitative reasoning case by case, but `mmat_overall` is always derived
from the count, with no discretionary adjustments.

---

## 2. Does PROBAST / TRIPOD-AI apply?

`applies_prediction_appraisal`: `yes` \| `no`

Mark **`yes`** if the paper:

- develops, validates, or updates a **predictive model** (diagnostic,
  prognostic, treatment response, risk stratification), **or**
- reports a **signature/score** used as a predictor of a clinical outcome
  with metrics such as AUC / C-index / sensitivity-specificity /
  calibration.

Mark **`no`** (leave the PROBAST/TRIPOD-AI domains blank or `NA`) if it is:

- purely associative discovery / differential expression without a formal
  predictive model,
- only feature selection / ranking without predictive evaluation of an
  outcome,
- methods, technical pipeline, or resource without clinical predictive
  validation.

When in doubt → `yes` and document in `appraisal_note` (rule aligned with
screening: prefer over-applying the complement to missing it).

---

## 3. PROBAST (targeted domains)

Judgments per domain: `low` \| `high` \| `unclear`
(PROBAST equivalent: low / high / unclear risk of bias).

| Column | Domain |
|---|---|
| `probast_participants` | Participants / data source / eligibility |
| `probast_predictors` | Predictors (definition, measurement, availability at time of prediction) |
| `probast_outcome` | Outcome (definition, ascertainment, blinding relative to predictors) |
| `probast_analysis` | Analysis (events per variable, missing-data handling, overfitting, validation) |
| `probast_overall_rob` | Overall judgment of the model's risk of bias |
| `probast_overall_applicability` | Applicability to the AIBIO_GU PCC: `low` concern \| `high` concern \| `unclear` |

Heuristic for `probast_overall_rob`:

- `low` if **all** domains 1–4 are `low`
- `high` if **any** domain is `high`
- `unclear` in any other case (includes mixtures with `unclear`)

Optional short rationale in `probast_rationale` (key signals, not an essay).

---

## 4. TRIPOD-AI (reduced reporting)

This is not the full 27+ item checklist. Four reporting signals relevant
to the integrative synthesis of AI models:

| Column | Question (yes / partial / no / NA) |
|---|---|
| `tripod_ai_data` | Are data source, eligibility, and missing-data handling described clearly? |
| `tripod_ai_model` | Are architecture/algorithm, input features, and training/tuning procedure described? |
| `tripod_ai_validation` | Is internal and/or external validation described (method + metrics)? |
| `tripod_ai_transparency` | Are code and/or data available, or is non-availability explicitly justified? |
| `tripod_ai_overall` | `adequate` (≥3 yes) \| `partial` (1–2 yes, rest partial/no) \| `inadequate` (0 yes) \| `NA` |

---

## 5. Operational process

1. **Pilot (n=20):** closed 2026-09-21 — see
   `AIBIO_GU_appraisal_PILOT_SUMMARY.md`. Calibration adopted below.
2. **Scale-up:** parallel batches (FT R1 pattern) with this frozen codebook.
3. Appraisal assisted by PDF (text of initial pages + Methods/Results as
   needed). Mark `appraisal_note` if the judgment is borderline.
4. Nominal reviewer R1: `REV-ALCIDES`. Double appraisal R2: deferred; if no
   complete R2 is available, post-scale-up verification sampling.

### Post-pilot calibration (frozen)

- In this corpus, `applies_prediction_appraisal=yes` is the near-general
  rule (signatures/models); mark `no` only with clear evidence of a purely
  associative paper / without predictive evaluation.
- `mmat_overall=high` will be exceptional (public databases + ML mining).
- The PROBAST Analysis domain being `high` is frequent and expected in
  omics signatures with massive feature/algorithm selection.
- PDF with body text in Chinese: appraise if the English abstract/methods
  are sufficient; note in `appraisal_note` (precedent REC-000536).
- Applicability concern (comorbidity, pan-cancer, exposure proxy): does not
  reclassify eligibility; goes into `probast_overall_applicability` +
  `appraisal_note`.

---

## 6. CSV columns

```
record_id, study_label, title, year, journal, doi,
mmat_category, screen_clear_question, screen_data_address_question,
c1, c2, c3, c4, c5,
rationale_c1, rationale_c2, rationale_c3, rationale_c4, rationale_c5,
mmat_overall, criteria_note,
applies_prediction_appraisal,
probast_participants, probast_predictors, probast_outcome, probast_analysis,
probast_overall_rob, probast_overall_applicability, probast_rationale,
tripod_ai_data, tripod_ai_model, tripod_ai_validation, tripod_ai_transparency,
tripod_ai_overall,
reviewer_id, assessed_at, appraisal_note, pilot_batch
```

- `pilot_batch`: `pilot` \| blank (scale-up) \| batch id (`batch-01`…).
- Blank judgment fields = pending.

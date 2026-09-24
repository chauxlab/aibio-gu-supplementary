# Decision log — AIBIO_GU

## 2026-09-09 — Case creation and protocol

- Created the `AIBIO_GU` case with `scripts/create_review_scaffold.sh`.
- Topic: *Artificial Intelligence–Driven Biomarker Discovery in Genitourinary
  Oncology*. **Integrative** review (Whittemore & Knafl), manuscript in
  **English**, no external registration (PROSPERO does not apply; OSF
  optional).
- `review.yaml` edited: `review_id: SR-AIBIO-GU`,
  `review_type: integrative_review`, `status: active`,
  `r1_reviewer_id: REV-ALCIDES`, `r2_reviewer_id: REV-PAOLA`.
- Intake directory renamed `rayyan_initial_2026-09-09` →
  `intake_2026-09-09` (convention of `review_intake_ris.py`).
- **Scope defined with the user (2026-09-09):**
  - Population: all GU sites (prostate, urothelium/bladder, kidney, penis,
    testis).
  - Concept: all AI-guided biomarker modalities (digital pathology,
    radiomics/imaging, -omics, multiomics).
  - Outcome: any clinical purpose (diagnostic, prognostic, response
    prediction).
- Framework: **PCC** with declared outcomes.
- Planned appraisal: **MMAT v.2018** + **PROBAST/TRIPOD-AI** domains for
  predictive models.
- `protocol/protocol.md` drafted. **Approved by the user** ("approved, proceed").

## 2026-09-09 — Search decisions

- **Equation adjustment (at the user's request):** the broad equation
  yielded ~1,500 unique records (prostate 767 / bladder 408 / kidney 457).
  It was restricted to require biomarker-discovery language
  (`"biomarker discovery"`, `"novel biomarker"`, `"prognostic signature"`,
  `"gene signature"`, `"radiomic signature"`, `nomogram`,
  `"molecular subtype"`) in prostate/bladder/kidney → 202/171/199.
- **Penis and testis:** kept without the biomarker block (very low
  density: 14 and 57) and with an expanded AI block; relevance to be
  decided at screening.
- **Search split by tumor site** due to the PubMed connector's 20-boolean-
  operator limit; merged by PMID.
- **Channel B (Europe PMC) deferred** to a future session; it will be
  documented as an amendment when executed. The current corpus is Channel
  A only (PubMed).
- Central exclusion criterion confirmed by the user: AI models that only
  use established biomarkers as predictors, and imaging/pathology models
  aimed solely at detection/segmentation/assisted diagnosis without
  biomarker derivation, are excluded.

## 2026-09-09 — Channel B: Europe PMC availability and decision

- **User query:** why Europe PMC "is not available" when it was used in
  previous reviews.
- **Finding:** the REVISOR project has no Europe PMC MCP connector (only
  PubMed and Scite). The Europe PMC searches of previous reviews
  (HPV_PSCC, etc.) were run in the **REDACTOR** project, and REVISOR
  received the RIS already built. This is the first review whose search
  is run entirely within REVISOR.
- **Decision:** run Channel B against the **public Europe PMC REST API**
  (EBI, no authentication), which gives reproducible queries with
  `TITLE:`/`ABSTRACT:` syntax. Alternative to moving the search to
  REDACTOR.
- **Scite ruled out** as a substitute for Europe PMC: it searches full
  text, has low precision, and its sampling is not reproducible. Reserved
  for targeted gap tracking.
- **Embase / Scopus / Web of Science:** omitted for lack of institutional
  access; acceptable for an integrative review, documented as a
  limitation.
- **Preprints** (65 from Channel B): retained for screening; the
  preprint→published substitution rule is documented in the protocol.

## 2026-09-09 — Closing the database search

- Evaluated expanding to OpenAlex / Semantic Scholar / SciELO / arXiv.
- **Decision (user):** close the search at two channels (PubMed + Europe
  PMC), matching the standard of HPV_PSCC / ctDNA_GU / GLP1. Reasons:
  eligibility restricted to a human GU cohort + biomarker derivation
  (literature indexed in MEDLINE/PMC); Channel A/B overlap of 89%
  (saturation); integrative review (lower multi-database bar).
- **Channel C = citation tracking** (backward and forward) over includes
  and retrieved reviews, after full-text screening, plus literature
  contributed by the user. It will be declared as "other methods" in
  PRISMA.
- Documented limitation: Embase/Scopus/WoS (no access) and aggregators
  not run.

## 2026-09-13 — T/A conflict consensus (105 R1/R2 discrepancies)

- **Consulted the user** before resolving, as these were methodological
  decisions affecting large blocks of records:
  1. **Rule for the radiomics cluster without an explicit ML-type feature-
     selection algorithm** (~35-40 records, pattern R1=`Maybe`/R2=`Included`).
     **User's decision:** delegated to own judgment ("decide by your best
     judgment and proceed"). Adopted rule: the radiomic extraction/
     signature itself satisfies the protocol's AI/ML Concept (consistent
     with `radiomics[tiab]` as a term in the AI/ML block of the search
     equation) → `Included` for that pattern. Extended by analogy to
     pathomics (REC-000387) and to algorithmic multiomics pipelines
     without classic supervised ML (REC-000073, -000237).
  2. **REC-AIBIOGU-000678** (co-authorship by Chaux A., explicit COI note
     in the HANDOFF-R2, major discrepancy R1=`Excluded`/R2=`Included`).
     **User's explicit decision:** `Excluded`/`wrong intervention` — R1's
     criterion is kept (the AI only correlates PD-L1/CD8, already
     established, without deriving a new biomarker). Documented as a
     consensus resolution motivated by the conflict of interest, not by
     automatic tie-breaking or record discard.
  3. **Remaining discrepancies (~65 records):** the user requested case-
     by-case resolution applying the "golden rule" from HANDOFF-R2. Full
     detail of each decision and its justification in
     `logs/workflow-log.md` (2026-09-13 entry) and in the file
     `screening/conflicts/2026-09-13__consenso-TA-R1-R2.csv`.
- **Initial result:** of the 105 discrepancies, 52 `Included`, 33
  `Excluded`, 20 `Maybe`.
- **User approved proceeding** ("approved, proceed"). While preparing the
  import, it was found that `import_ta_screening_decisions.py` requires
  **binary** consensus for all 697 records (does not accept `Maybe`, and
  the consensus CSV must cover the entire corpus, not just the 105
  conflicts) — a precedent already established in HPV_PSCC. The 20
  `Maybe` from the conflicts file plus 15 additional records where R1 and
  R2 agreed on `Maybe` (concordant, no conflict) were resolved to binary.
  Detail and per-record justification in `logs/workflow-log.md` (entry
  "Adjustment to binary consensus and import to SQLite", 2026-09-13).
- **Final T/A totals over the 697 records: Included 536 / Excluded
  161.**
- **Import executed** with a prior SQLite backup.
  `import_batch_id=IMP-SR-AIBIO-GU-2026-09-13-TA-SCREEN`; 536 records
  moved to `full_texts.retrieval_status = pending_retrieval`, 161 to
  `not_required_after_initial_screening`.

## 2026-09-21 — Appraisal: scaffolding + pilot (n=20)

- User approved the scaffolding → pilot → scale-up plan.
- Artifacts in `risk_of_bias/`:
  - `AIBIO_GU_appraisal_codebook.md` (MMAT v.2018 + PROBAST domains +
    reduced TRIPOD-AI)
  - `AIBIO_GU_appraisal_BLANK_400.csv` (400 FT Included)
  - `AIBIO_GU_appraisal_PILOT_20.csv` + `AIBIO_GU_appraisal_PILOT_SUMMARY.md`
- Stratified pilot (5 prostate / 5 bladder / 5 kidney / 3 testis /
  1 penis / 1 other). Assisted appraisal (PDF pp.1–12),
  `reviewer_id=REV-ALCIDES`.
- Pilot result: MMAT moderate 12 / low 8; PROBAST applies 20/20;
  PROBAST overall RoB high 16 / unclear 4; TRIPOD-AI partial 11 /
  adequate 8 / inadequate 1.
- **Calibration frozen in the codebook** (section 5): PROBAST near-
  universal in this corpus; MMAT `high` rare; Analysis `high` expected
  in omics signatures; Chinese-language PDF appraisable via the EN
  abstract; applicability concerns do not reclassify eligibility.
- **Next:** scale-up over the remaining 380 in batches; SQLite import of
  `risk_of_bias` upon closing.

## 2026-09-22 — Appraisal: full scale-up (380) + consolidation + SQLite import

- Scale-up executed in 8 parallel batches (AI agents, ~47-48 records
  each, `reviewer_id=REV-ALCIDES-AI`), on the full PDF in
  `screening/full_text/` (400/400 available). 0 `cannot_appraise`.
- **Inconsistency detected on merging:** the `mmat_overall` heuristic as
  written in the codebook (moderate=≤1 no/cant_tell, low=2+) did not
  match the pilot's actual practice (7/8 cases with 2 no/cant_tell rated
  `moderate`). The 8 batches were split between both interpretations.
  **User's decision:** apply the written rule mechanically to the 400
  rows → 46 reclassified (9 pilot + 37 scale-up). Codebook corrected to
  remove the ambiguity (section 1).
- Additional normalization: `probast_overall_applicability` to canonical
  values (`low concern`/`high concern`/`unclear`); correction of 1 row
  (REC-AIBIOGU-000079) with an overflowed note in `tripod_ai_overall`.
- Consolidated into `AIBIO_GU_appraisal_FULL_400.csv` (400 unique rows,
  verified no duplicates). Final distribution: MMAT low 258/moderate
  132/high 10; PROBAST applies 398/400; PROBAST RoB high 356/unclear 42;
  applicability low concern 358/high concern 29/unclear 11; TRIPOD-AI
  adequate 201/partial 192/inadequate 5.
- Imported to SQLite via `reviews/scripts/import_aibio_gu_appraisal.py`:
  400 `studies` (1:1 with records) + 7978 `risk_of_bias` rows
  (`MMAT_v2018` 400×9 domains, `PROBAST` 398×6, `TRIPOD-AI` 398×5).
  Pre-import backup at `/tmp/reviews.sqlite.bak-before-aibiogu-appraisal-import`.
- 251/400 rows with an `appraisal_note` (overfitting/implausible AUC,
  possible cohort overlap between twin papers, preprints not peer-
  reviewed, "external" validation that is actually an internal split,
  applicability concerns) — relevant for weighting in extraction/
  synthesis.
- **Appraisal closed. Next: data extraction over the 400
  Included → Channel C (citation tracking) → handoff to Session 2.**

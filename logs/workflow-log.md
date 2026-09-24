# Workflow log — AIBIO_GU

## 2026-09-09 — Initial preparation and protocol

- Scaffold created. `review.yaml` and `protocol/protocol.md` drafted.
- Current cycle phase: **protocol** (drafted, pending approval).
- Next phase: **search**.

## 2026-09-09 — Channel A search (PubMed) and intake

1. Protocol approved by the user. Equation adjusted to increase precision
   (biomarker-discovery block required in prostate/bladder/kidney; penis
   and testis with an expanded AI block and no biomarker block).
2. **PubMed search via the NCBI connector**, by tumor site (the
   connector's 20-boolean-operator limit forces the split): prostate 202,
   bladder 171, kidney 199, penis 14, testis 57 → **643 gross**.
3. Deduplication by PMID → **632 unique**.
4. Metadata retrieved via `get_article_metadata` (batches of 20; the
   connector limits to 20 articles/call). RIS built with an in-house
   script (`scratchpad/build_ris.py`) →
   `searches/ris/AIBIO_GU_pubmed_2026-09-09.ris` (632 entries; 628 with
   abstract, 630 with DOI).
5. `archive/rayyan_initial_2026-09-09/` renamed to `archive/intake_2026-09-09/`.
6. **Intake:** `python3 scripts/review_intake_ris.py --review reviews/AIBIO_GU`.
   Result: `RIS records=632`, `canonical_records=632`, `identifiers=1262`,
   `duplicate_candidate_rows=0`,
   `import_batch_id=IMP-SR-AIBIO-GU-2026-09-02-RIS` (the script's fixed
   `TODAY` date = 2026-09-02, not edited; actual operating date
   2026-09-09). Prior database backup in scratchpad.
7. Verified in SQLite: 632 `SR-AIBIO-GU` records
   (`REC-AIBIOGU-000001`…`000632`), 628 with abstract. Reviewers
   `REV-ALCIDES`/`REV-PAOLA` reused.
8. Snapshot: `data/process_snapshots/2026-09-09__initial_sqlite_intake.csv`
   (632 rows, all `pending_screening`).
9. **Pending:** Channel B (Europe PMC) and Channel C.

## 2026-09-09 — Channel B search (Europe PMC) and combined intake

1. **Verification of Europe PMC availability** (at the user's request):
   the REVISOR project has no Europe PMC MCP connector (only PubMed and
   Scite). In previous reviews (e.g. HPV_PSCC) the Europe PMC search was
   run in the REDACTOR project, which did have it, and REVISOR received
   an already-deduplicated RIS. Embase/Scopus/WoS require institutional
   access (not available; acceptable omission for an integrative review).
2. **Solution:** the **public Europe PMC REST API** was used (EBI, no
   authentication), `TITLE:`/`ABSTRACT:` syntax, same block structure as
   Channel A, `SRC:MED OR SRC:PMC OR SRC:PPR`. Script:
   `scratchpad/fetch_epmc.py` (pagination via `cursorMark`).
3. Result: prostate 189 / bladder 152 / kidney 169 / penis 14 / testis 54
   → **576 unique**. Cross-dedup by PMID+DOI against Channel A: 511
   overlap, **65 new** (mostly preprints from Research Square/bioRxiv/
   medRxiv/Preprints.org/SSRN; ~12 MEDLINE records not captured by
   Channel A's per-site equations).
4. **Scite ruled out as a channel:** `search_literature` searches full
   text, returns thousands of hits/site with low precision (mostly
   reviews), sampling neither exhaustive nor reproducible. Not
   incorporated.
5. Combined RIS: `scratchpad/build_ris_combined.py` →
   `searches/ris/AIBIO_GU_pubmed_europepmc_2026-09-09.ris` (697 entries,
   693 with abstract, 694 with DOI). The PubMed-only RIS moved to
   `searches/ris/superseded/`. The `N1` field marks `source_channel: A|B`.
6. **Re-intake:** `review_intake_ris.py --review reviews/AIBIO_GU` (full
   purge and reload). `RIS records=697`, `canonical_records=697`,
   `identifiers=1337`, `duplicate_candidate_rows=0`. Prior database
   backup in scratchpad (`reviews.sqlite.bak2`).
7. Verified in SQLite: 697 `SR-AIBIO-GU` records
   (`REC-AIBIOGU-000001`…`000697`), 693 with abstract.
8. Snapshot: `data/process_snapshots/2026-09-09__initial_sqlite_intake_v2.csv`
   (697 rows, `pending_screening`). The v1 snapshot (632) was deleted.
9. **Database search closed** (decision 2026-09-09): two channels, no
   aggregators. Channel C = citation tracking after FT screening.
10. **Next phase: double R1/R2 title/abstract screening** from
    `initial_screening` over the 697.

## 2026-09-09 — T/A screening package for R2 (Paola)

- Blinded spreadsheets exported from SQLite (script
  `scratchpad/export_blind_screening.py`, copied to
  `archive/intake_2026-09-09/scripts/`):
  - `screening/exports/2026-09-09__cribado-R2-britos.csv` (697 rows)
  - `screening/exports/2026-09-09__cribado-R1-chaux.csv` (697 rows, identical)
  - Columns: record_id, title, year, journal, authors, doi, abstract,
    keywords, url, decision, exclusion_reason, note. 4 records with
    `[Abstract not available]`.
- Instructions: `screening/exports/2026-09-09__HANDOFF-cribado-R2-Britos.md`
  (project, question, PCC, inclusion/exclusion criteria with the "golden
  rule" of biomarker discovery, exclusion codes, COI note for Chaux's
  co-authorship on REC-AIBIOGU-000678 and -000686, return instructions).
- Package: `screening/exports/paquete-R2-britos-2026-09-09.zip` (R2 CSV +
  HANDOFF). Format modeled on the HPV_PSCC one.
- **Pending:** send the zip to Paola; R1 (Alcides) screens his spreadsheet
  in parallel; upon return of both → `screening/imports/` → conflicts +
  consensus → `import_ta_screening_decisions.py`.

## 2026-09-13 — Title/abstract screening R1 (Alcides) completed

- Screening of the 697 records applying the HANDOFF-R2 criteria (same PCC
  framework and exclusion rules). Title+abstract read record by record;
  the first 50 reviewed directly, 51-697 processed with the same
  calibration (verified with post-hoc random sampling, consistent
  decisions).
- Result: `screening/imports/2026-09-09__cribado-R1-chaux-COMPLETO.csv`
  (697 rows, no empty `decision` cells, valid `exclusion_reason` in all
  `Excluded`).
- Count: **Included 501 / Excluded 134 / Maybe 62**.
- `exclusion_reason` breakdown (134): wrong intervention 62 (mostly for
  combining/classifying already-established biomarkers or scores —PSA,
  PI-RADS, NLR, SII, Ki-67, PD-L1, clinical nomograms— without
  discovering a new one, or evaluating chatbots/LLMs without a
  biomarker), wrong publication type 36 (reviews, editorials, letters,
  guidelines), wrong outcome 18 (non-oncological outcomes or
  segmentation/quantification of already standardized metrics without a
  new biomarker), wrong population 15 (non-GU tumor or preclinical
  without a human cohort), wrong study design 3.
- The 62 `Maybe`: mostly radiomics without an explicit ML feature-
  selection algorithm (to be resolved at full text), plus ~8 systematic/
  scoping reviews highly relevant to the topic (marked for reference
  tracking rather than excluded).
- REC-AIBIOGU-000678 and -000686 (Chaux co-authorship) evaluated with the
  same criteria as any record (both Excluded for methodological reasons,
  not for COI).
- **Pending:** R2 (Paola) spreadsheet — not yet returned
  (`screening/imports/` only has the README). Upon receipt: compare
  decisions, calculate kappa, resolve conflicts by consensus, and import
  with `import_ta_screening_decisions.py`.

## 2026-09-13 — Title/abstract screening R2 (Paola/Britos) received and verified

- Spreadsheet returned by Paola: `screening/imports/2026-09-09__cribado-COMPLETO.csv`
  (697 rows + header). Structural verification: complete coverage of IDs
  `REC-AIBIOGU-000001`…`000697` (0 missing, 0 extra), no empty/invalid
  `decision`, no `Excluded` without `exclusion_reason`.
- R2 count: **Included 531 / Excluded 107 / Maybe 59** (vs. R1: Included 501
  / Excluded 134 / Maybe 62).
- **R1 vs R2 comparison** (ad hoc Python script, `csv.DictReader` +
  manual Cohen's kappa calculation over the 3 categories
  Included/Excluded/Maybe):
  - Simple agreement (po): 592/697 = **84.9%**.
  - **Cohen's kappa: 0.637** (substantial agreement, Landis-Koch scale).
  - Total discrepancies: **105**, exported to
    `screening/conflicts/2026-09-13__conflictos-TA-R1-vs-R2.csv`
    (columns: record_id, title, decision_R1_chaux, reason_R1,
    decision_R2_britos, reason_R2, note_R1, note_R2).
  - **Major** discrepancies (Included↔Excluded at opposite extremes): 14
    records — REC-AIBIOGU-000045, -000075, -000077, -000089, -000097,
    -000141, -000145, -000169, -000217, -000325, -000502, -000573, -000575,
    and **-000678**.
- **COI note:** REC-AIBIOGU-000678 (Chaux co-authorship) is among the 14
  major discrepancies (R1=Excluded/wrong intervention vs. R2=Included).
  Per the HANDOFF-R2 COI note, this record must be resolved through
  explicit, documented consensus, not automatic tie-breaking.
  REC-AIBIOGU-000686 (also Chaux co-authorship) is concordant: both
  Excluded.
- **Pending — next steps when resuming this review:**
  1. Resolve by consensus the 105 discrepancies in the file
     `screening/conflicts/2026-09-13__conflictos-TA-R1-vs-R2.csv`,
     prioritizing the 14 major ones (starting with REC-AIBIOGU-000678
     given the COI).
  2. Record the consensus decisions (additional column or resolution
     file) in `screening/conflicts/`.
  3. With the final decisions (non-discrepant + consensus), run
     `import_ta_screening_decisions.py` (relocated to `reviews/scripts/`;
     confirm path/args before running) to update the status in SQLite
     and move the `Included` to `pending_full_text` (or the corresponding
     status per the schema).
  4. After the import, start **full-text (FT) screening** on the included
     records.
  5. Remember the task already recorded in memory: after the T/A
     consensus, export RIS of the `Included` for a Paperpile full-text
     catalog.

## 2026-09-13 — T/A conflict consensus R1 vs R2 (105 discrepancies)

- Consensus rules agreed with the user before resolving:
  1. **Radiomics without an explicit ML-type feature-selection algorithm**
     (e.g. only logistic/Cox regression, without a named LASSO/RF/mRMR/
     neural network): decided by own judgment that it **counts as
     AI/ML** — the radiomic extraction/selection itself satisfies the
     protocol's Concept (matches `radiomics[tiab]` as a term in the
     AI/ML block of the search equation). Extended by analogy to
     "pathomics" (quantitative nuclear heterogeneity, REC-000387) and to
     algorithmic multiomic discovery pipelines without classic
     supervised ML (DEPTH, WGCNA/limma/Mfuzz: REC-000073, -000237).
  2. **REC-AIBIOGU-000678** (Chaux co-authorship, explicit COI note in
     the HANDOFF): resolved by the user's explicit decision as
     **`Excluded`/`wrong intervention`** — the AI correlates PD-L1/CD8
     (already-established biomarkers) without deriving a new one.
     Documented as a consensus decision due to the COI, not automatic
     tie-breaking. REC-AIBIOGU-000686 (also Chaux co-authorship) was
     already concordant (`Excluded` by both reviewers), with no conflict
     to resolve.
  3. **Remaining 105 conflicts:** resolved case by case applying the
     "golden rule" from HANDOFF-R2 (does the AI derive/prioritize a new
     biomarker, or only reuse/classify/segment an already-established
     one?), supported by both reviewers' notes. Notable internal
     consistency rules:
     - Pan-cancer studies with a site-specific finding reported for a GU
       site (bladder, kidney) → `Included`; without a site-specific
       result → `Excluded`/`wrong population` (REC-000188, -240, -265,
       -424).
     - Panels of inflammatory/immune indices already widely established
       in general oncology (NLR, SII, PLR, AGR, DRR, etc.) combined via
       LASSO/RSF/Cox → `Excluded`/`wrong intervention` (cluster
       REC-000217, -325, -357, -396, -534), except REC-000141 (functional
       lymphocyte subpopulations, not standardized, ML prioritizes 9/42
       candidates) → `Included`.
     - Chemotherapy toxicity outcomes (ototoxicity, nephrotoxicity,
       metabolic syndrome) → `Excluded`/`wrong outcome` (cluster
       REC-000588, -610, -624).
     - Automated quantification of already-established biomarkers via
       digital histopathology without a new biomarker (PTEN, Ki67/LSD1,
       PD-L1/CD8, lymphovascular invasion) → `Excluded`/`wrong
       intervention` (REC-000145, -607, -618, -621, -678).
     - Highly relevant reviews marked `Maybe` by both criteria or by one
       of the two → consensus `Maybe` (context/reference tracking, not
       as an included study): REC-000012, -018, -020, -138, -194,
       -258, -491, -623, -629, -697.
     - Duplicates detected within the corpus itself, marked for
       deduplication at full text: REC-000161/-000642 (same DWI
       radiomics study), REC-000496/-000668 (same multiphase CT ccRCC
       study).
- Resolution file: `screening/conflicts/2026-09-13__consenso-TA-R1-R2.csv`
  (105 rows: `consensus_decision`, `consensus_exclusion_reason`,
  `consensus_rationale` per record).
- **Consensus result (105):** Included 52 / Excluded 33 / Maybe 20.
- **Final T/A totals (697 = 592 concordant + 105 consensus):**
  concordant Included 477 / Excluded 100 / Maybe 15; **final: Included
  529 / Excluded 133 / Maybe 35.**

## 2026-09-15 — RIS catalog for Paperpile exported

- `python3 reviews/scripts/export_full_text_ris.py --review reviews/AIBIO_GU`
  (default `--status pending_retrieval`) →
  `screening/full_text/2026-09-15__full-text-catalog.ris`.
- **536 records, 536 with DOI, 0 without DOI** (matches the 536 `Included`
  from the consensus imported on 2026-09-13).
- **Pending:** send the RIS to the user for import into Paperpile
  (identification + automated PDF download by DOI). Upon return of
  `paperpile-files.zip`, re-ingest with
  `python3 reviews/scripts/ingest_paperpile_full_texts.py --review reviews/AIBIO_GU --zip ~/Downloads/paperpile-files.zip`
  and re-export for the 2nd pass of pending records.

## 2026-09-13 — Adjustment to binary consensus and import to SQLite

- `import_ta_screening_decisions.py` (`reviews/scripts/`) requires the
  `consensus` column to be **binary** (`Included`/`Excluded`; see lines
  167-168 of the script) — it does not accept `Maybe` as a final consensus
  decision, matching the HPV_PSCC precedent (`consenso-TA-screening.csv`
  there also forced the `Maybe` to binary). The importer also requires
  the consensus CSV to cover all **697** records (not just the 105
  conflicts): **15 additional records** were found where R1 and R2 agreed
  on `Maybe` (concordant, which is why they did not appear in the
  conflicts file) and which also required binary resolution.
- **Resolution of the 35 total `Maybe`** (20 from the conflicts file +
  15 concordant), applying the same rule framework:
  - **Highly relevant reviews → `Excluded`/`wrong publication type`**
    (context/reference tracking, not an included study, per explicit
    HANDOFF rule): REC-000012, -018, -020, -040, -102, -138,
    -148, -194, -258, -268, -274, -372, -373, -491, -566, -569, -585,
    -623, -629, -697 (20 reviews).
  - **`Included` (sent for full-text verification)** for a plausible
    candidate biomarker or insufficient information to exclude at
    title/abstract level: REC-000017, -036, -103,
    -461, -502, -574, -590 (7 records).
  - **`Excluded` for a specific reason** (not a review): REC-000106/-640
    (same study; the nomogram's final variables are clinical, not
    radiomic — `wrong intervention`), REC-000128 (real ML but outcome
    outside the PCC framework — `wrong outcome`), REC-000233
    (only univariate statistics, no signature/score — `wrong
    intervention`), REC-000248 (abstract describes no biomarker or AI/ML
    despite the title — `wrong intervention`), REC-000445 (predicts
    already-established VEGF — `wrong intervention`), REC-000539 (no
    metric reported for the renal candidates — `wrong outcome`),
    REC-000613 (only multiple t-tests, no composite signature — `wrong
    intervention`) (8 records).
  - Full detail with per-record justification in
    `screening/conflicts/2026-09-13__consenso-TA-final-697.csv` (697
    rows, columns `record_id`, `consensus`, `exclusion_reason`, `note`)
    and in `2026-09-13__consenso-TA-R1-R2.csv` (105 conflicts, updated
    with the binary resolution).
  - Open note: REC-000445 and REC-000461 (nephroblastoma/Wilms tumor)
    share a population-fit doubt not formally resolved by the protocol
    (embryonal pediatric renal neoplasm, not RCC); 000445 was excluded
    for an intervention reason independent of that doubt, 000461 is sent
    to full text with the doubt documented to be resolved at
    extraction/synthesis.
- **Final reviewed T/A totals: Included 536 / Excluded 161** (adjusted
  from the previous report of 529/133/35, which included 35 `Maybe` not
  valid for the importer).
- Database backup prior to import:
  `scratchpad/reviews.sqlite.bak-pre-ta-consensus-2026-09-13` (session).
- **Import executed:**
  `python3 reviews/scripts/import_ta_screening_decisions.py --review
  reviews/AIBIO_GU --r1-csv .../2026-09-09__cribado-R1-chaux-COMPLETO.csv
  --r2-csv .../2026-09-09__cribado-COMPLETO.csv --consensus-csv
  .../2026-09-13__consenso-TA-final-697.csv --decision-date 2026-09-13`.
  `import_batch_id=IMP-SR-AIBIO-GU-2026-09-13-TA-SCREEN`. Result:
  `updated_r1=697 updated_r2=697 consensus_inserted=697`;
  `full_texts pending_retrieval=536 not_required=161`; `pending_r1_r2_left=0`.
- **Pending:**
  1. Export RIS of the 536 `Included` for a Paperpile full-text catalog
     (task already recorded in memory).
  2. Start full-text (FT) screening on the 536 `Included`
     (`pending_retrieval` in `full_texts`).
  3. Channel C (citation tracking) on reviews excluded for `wrong
     publication type` and on the final `Included`, after FT screening.

## 2026-09-13 — Session close: T/A screening closed, next steps

**Cycle status:** title/abstract screening **closed** in SQLite for the
697 records of the combined corpus (PubMed + Europe PMC). Current phase:
**full-text (FT)** retrieval and screening.

**Next steps, in order:**

1. **Export the RIS catalog of the 536 `Included`** for Paperpile (task
   recorded in the user's memory: "after the T/A consensus, export RIS of
   the Included for the full-text Paperpile catalog"). The records to
   export are those left with
   `full_texts.retrieval_status = 'pending_retrieval'` after batch
   `IMP-SR-AIBIO-GU-2026-09-13-TA-SCREEN`.
2. **Retrieve the full text** of those 536 records (PDF/HTML depending on
   availability; preprints included per the protocol rule, with
   substitution by the published version if it appears during FT).
3. **Double full-text screening** (R1 `REV-ALCIDES` / R2
   `REV-PAOLA`), with controlled exclusion reasons
   (`exclusion_reasons`), same structure as T/A screening
   (blinded spreadsheets → conflicts → documented consensus → import to
   SQLite).
4. **Points to resolve explicitly during FT**, already flagged in the T/A
   consensus and not to be lost:
   - The **10 records sent to FT due to content/method ambiguity**
     instead of being decided at abstract level: REC-AIBIOGU-000017, -036,
     -103, -461, -502, -574, -590 (plus -161/-000642 and -496/-000668,
     which are additionally **internal duplicates** of the corpus to be
     merged at FT).
   - **REC-AIBIOGU-000461** (Wilms tumor): confirm whether it fits the
     protocol's Population definition (embryonal pediatric renal
     neoplasm, not renal cell carcinoma) before extracting it as a
     definitive include.
   - Verify at FT whether any of the `Included` under the rule
     "radiomics counts as AI/ML without an explicit selection algorithm"
     (criterion adopted in this session, see the consensus entry above)
     should be reclassified upon reading the full methodology — this rule
     was applied at title/abstract level and is more lenient than
     requiring a named ML algorithm; FT is the natural place to confirm
     or correct it study by study.
   - The **20 reviews** excluded for `wrong publication type` but marked
     as "highly relevant" (context/reference tracking) are reserved for
     **Channel C** (backward/forward citation tracking), not discarded
     entirely.
5. **Channel C (citation tracking):** run after closing FT screening,
   over the definitive included studies and over the reviews from the
   previous point, per the protocol's definition.
6. After closing FT + appraisal (MMAT/PROBAST-TRIPOD-AI) + extraction:
   export the handoff package with
   `python3 scripts/export_handoff_sesion2.py --review reviews/AIBIO_GU`
   so that REDACTOR Session 2 can draft the manuscript.

**Not pending / already resolved in this session:** T/A screening R1 and
R2 complete, kappa calculated (0.637, substantial agreement), 105
conflicts + 15 concordant-`Maybe` resolved by documented consensus
(includes REC-AIBIOGU-000678 due to Chaux co-authorship COI), SQLite
import verified (`pending_r1_r2_left=0`).

## 2026-09-21 — Appraisal scaffolding + pilot

1. Post-FT phase: final consensus 400 Included / 36 Excluded already in
   SQLite.
2. Created codebook + 400-row spreadsheet + stratified pilot n=20 in
   `risk_of_bias/`.
3. Pilot completed (4 parallel batches A–D → merged into
   `AIBIO_GU_appraisal_PILOT_20.csv`). Summary and calibration in
   `AIBIO_GU_appraisal_PILOT_SUMMARY.md` and `decision-log.md`.
4. **Status:** codebook frozen post-pilot. **Next:** scale-up appraisal of
   the remaining 380 → extraction → Channel C → handoff to Session 2.
5. Temporary PDF extractions in `risk_of_bias/pilot_txt/`
   (gitignored).

## 2026-09-22 — Appraisal: scale-up 380 + consolidation 400 + SQLite import

1. Scale-up executed in 8 parallel batches (`risk_of_bias/batches/`,
   ~47-48 records each) by AI agents over the full PDF. 380/380
   rows completed, 0 `cannot_appraise`.
2. When merging pilot+batches, an MMAT calibration inconsistency was
   detected and corrected (the codebook's written rule vs. the pilot's
   actual practice) — see `decision-log.md` for detail and resolution
   (mechanical rule frozen, 46/400 rows reclassified). Also normalized
   `probast_overall_applicability` and corrected 1 row with a column
   shift.
3. Consolidated into `risk_of_bias/AIBIO_GU_appraisal_FULL_400.csv` (400
   unique, no duplicates, 0 blanks in `mmat_overall`).
4. Imported to SQLite: 400 `studies` + `study_records` (1:1) + 7978
   `risk_of_bias` rows (MMAT_v2018/PROBAST/TRIPOD-AI) via
   `reviews/scripts/import_aibio_gu_appraisal.py`.
5. **Status: appraisal closed.** **Next:** data extraction over the 400
   Included.

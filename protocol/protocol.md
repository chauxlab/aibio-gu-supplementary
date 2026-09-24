# Operating protocol — AIBIO_GU

**Title:** Artificial Intelligence–Driven Biomarker Discovery in Genitourinary
Oncology

**Review type:** Integrative (Whittemore & Knafl)
**Manuscript language:** English
**Framework used:** PCC (Population – Concept – Context) with declared
outcomes
**Search date:** 2026-09-09 (Channel A PubMed + Channel B Europe PMC)
**review_id:** `SR-AIBIO-GU` · **review_slug:** `AIBIO_GU`
**External registration (OSF/PROSPERO):** Not registered. PROSPERO does not
apply to integrative reviews. Optional OSF registration is offered;
`protocol_ref: pending` in `review.yaml` until the user requests it.

## Case origin

New case, with no prior corpus in REDACTOR. The search will be run from
this case via the bibliographic connectors available in the session
(PubMed/MEDLINE, Europe PMC via Scite/Search) plus an additional channel
for literature already known to the user, if applicable ("identified
through other methods").

## Rationale

Biomarker discovery in genitourinary oncology (prostate, urothelium/
bladder, kidney, penis, testis) has rapidly incorporated artificial
intelligence and machine learning methods: deep learning on digital
pathology (WSI, H&E, IHC), radiomics and imaging models (mpMRI, CT, PET),
and machine learning models on genomic, transcriptomic, proteomic, and
methylation data, as well as multiomic integration. An integrative
synthesis is lacking that maps which modalities have been applied, to
which tumor sites, for what clinical purpose (diagnosis, prognosis,
response prediction), and with what degree of validation and readiness
for clinical translation.

## Review question

In patients with genitourinary neoplasms, which biomarkers have been
discovered or prioritized using artificial intelligence / machine
learning methods, on which data modalities, for what clinical purpose,
and what is the status of their analytical and clinical validation?

## PCC

- **P (Population):** Patients (or patient-derived samples/data) with
  genitourinary neoplasms: prostate carcinoma, urothelial / bladder
  carcinoma, renal cell carcinoma, penile squamous cell carcinoma, and
  testicular germ cell tumors. Translational research with human cohorts
  and public patient data (TCGA, CPTAC, etc.) is admitted.
- **C (Concept):** Discovery, identification, or prioritization of
  biomarkers using artificial intelligence or machine learning —
  including deep learning, classic machine learning, foundation models,
  and AI pipelines for feature selection — with the explicit aim of
  deriving a biomarker (molecular, histological/morphological, radiomic,
  or multiomic signature).
- **Context:** Any data modality: digital pathology / histology,
  radiology / medical imaging, genomics, transcriptomics, proteomics,
  epigenomics/methylation, and integrated multiomics. Any setting
  (academic, multicenter, retrospective, or prospective).
- **Outcomes (declared):**
  1. Discovered biomarker(s) and their data modality.
  2. Clinical purpose: diagnostic, prognostic, treatment response
     prediction, risk stratification, other.
  3. AI/ML method used and input data.
  4. Reported performance (AUC, C-index, HR, sensitivity/specificity,
     as applicable).
  5. Level of validation: internal (hold-out, cross-validation), external
     (independent cohort), or demonstrated clinical utility.
  6. Availability of code/data and adherence to reporting guidelines
     (TRIPOD-AI, CLAIM, DECIDE-AI, etc.).

## Eligibility criteria

### Inclusion

- Primary studies (retrospective or prospective) and translational
  studies with human cohorts or patient data.
- Explicit use of AI/ML as part of the biomarker discovery or
  prioritization process.
- Genitourinary neoplasm per the Population definition.
- Reports at least one candidate biomarker with some performance metric
  or association with a clinical outcome.
- Published in English (or another language if full text is evaluable;
  this will be documented).
- No lower date limit; the effective window after the search will be
  documented.

### Exclusion

- Reviews, editorials, commentaries, protocols, and conference abstracts
  without primary data (reviews are retained for contextual reading and
  reference tracking, not as included studies).
- AI models that use already-established biomarkers solely as predictors
  (without discovery/prioritization of a new biomarker).
- Purely methodological studies without application to a GU cohort.
- Imaging or pathology models aimed only at detection/segmentation or
  assisted diagnosis without derivation of a biomarker.
- Non-genitourinary tumors; non-epithelial GU tumors except testicular
  germ cell tumors (sarcomas, secondary lymphomas, metastases to a GU
  organ → excluded).
- Exclusively preclinical studies (cell lines, animal models) without a
  human tissue/data component.

## Search equations

Block structure: **(GU neoplasm) AND (AI/ML) AND (biomarker discovery /
signature)**. Due to the PubMed connector's limit (max. 20 boolean
operators per query), the search was run **by tumor site** and merged by
PMID.

### Channel A — MEDLINE (PubMed), run 2026-09-09

**Prostate / Bladder-urothelium / Kidney** (same 3-block pattern):

```
(<site>[MeSH] OR <site terms>[tiab])
AND
("artificial intelligence"[tiab] OR "machine learning"[tiab] OR
 "deep learning"[tiab] OR radiomics[tiab] OR "computational pathology"[tiab])
AND
("biomarker discovery"[tiab] OR "novel biomarker"[tiab] OR
 "prognostic signature"[tiab] OR "gene signature"[tiab] OR
 "radiomic signature"[tiab] OR nomogram[tiab] OR "molecular subtype"[tiab])
```

- Prostate: `("Prostatic Neoplasms"[MeSH] OR "prostate cancer"[tiab])` → **202**
- Bladder: `("Urinary Bladder Neoplasms"[MeSH] OR "bladder cancer"[tiab] OR
  "urothelial carcinoma"[tiab])` → **171**
- Kidney: `("Carcinoma, Renal Cell"[MeSH] OR "Kidney Neoplasms"[MeSH] OR
  "renal cell carcinoma"[tiab] OR "kidney cancer"[tiab])` → **199**

**Penis / Testis** (no biomarker block, due to low literature density; the
AI block was expanded with `"digital pathology"` and `"neural network"`):

- Penis: `("Penile Neoplasms"[MeSH] OR "penile cancer"[tiab] OR
  "penile carcinoma"[tiab] OR "penile squamous cell carcinoma"[tiab]) AND
  (expanded AI)` → **14**
- Testis: `("Testicular Neoplasms"[MeSH] OR "testicular cancer"[tiab] OR
  "testicular germ cell tumor"[tiab] OR "testicular germ cell tumour"[tiab])
  AND (expanded AI)` → **57**

RIS corpus: `searches/ris/AIBIO_GU_pubmed_2026-09-09.ris` (metadata via
the PubMed/NCBI connector; abstract present in 628/632).

### Channel B — Europe PMC, run 2026-09-09

Run against the **public Europe PMC REST API**
(`https://www.ebi.ac.uk/europepmc/webservices/rest/search`, no
authentication; no Europe PMC MCP connector is available in the REVISOR
session — in previous reviews the Europe PMC search was run in the
REDACTOR project). `TITLE:` / `ABSTRACT:` syntax, same block structure as
Channel A, restricted to `SRC:MED OR SRC:PMC OR SRC:PPR` (includes
preprints and PMC-only records not indexed in MEDLINE).

- Prostate → 189 · Bladder → 152 · Kidney → 169 (biomarker block
  required)
- Penis → 14 · Testis → 54 (expanded AI block, no biomarker block)
- **576 unique** after internal dedup; 511 already present in Channel A
  (89% overlap); **65 new records** incorporated (mostly preprints from
  Research Square / bioRxiv / medRxiv / Preprints.org / SSRN; ~12 MEDLINE
  records not captured by Channel A's per-site equations).

Combined RIS corpus: `searches/ris/AIBIO_GU_pubmed_europepmc_2026-09-09.ris`
(the PubMed-only RIS remains in `searches/ris/superseded/`). The `N1`
field of each record marks `source_channel: A` or `B`.

**Scite (dropped as a channel):** Scite's `search_literature` was tested
as a substitute for Europe PMC; its search is over full text (not
title/abstract), returns thousands of hits per site with low precision
(mostly reviews/overviews), and its sampling is neither exhaustive nor
reproducible as a database query. It is not incorporated into the corpus;
Scite is reserved for targeted gap tracking during synthesis.

### Preprints

Preprints are retained for title/abstract screening. Substitution rule:
if a published version of the same work is identified during screening or
extraction, the preprint is replaced by the published record and this is
documented in `decision-log.md`.

### Channel C — "identified through other methods" (citation tracking)

Decision 2026-09-09: the database search strategy is **closed at two
channels** (PubMed + Europe PMC), matching the standard of the
repository's previous reviews (HPV_PSCC, ctDNA_GU, GLP1). OpenAlex /
Semantic Scholar / SciELO / arXiv are not run: low marginal yield is
expected (eligibility requires a human GU cohort + biomarker derivation →
clinical/translational literature indexed in MEDLINE/PMC) and the 89%
Channel A/B overlap suggests saturation. Documented limitation.

Channel C = **citation tracking** after full-text screening: backward
references of the included studies and of retrieved systematic/narrative
reviews, plus forward citations of the includes, plus literature
contributed by the user (RIS/DOI). Findings are declared separately in
the PRISMA count as "other methods".

Run 2026-09-23 (Europe PMC): backward citations of the 20 reviews reserved
at title/abstract screening, and forward citations of the 400 includes
from the database search. No RIS or DOI was contributed by the user. The
backward citations of those 400 primary studies are outside the scope of
this pass.

### Databases with no connector or open API

Embase, Web of Science, Scopus, IEEE Xplore, Cochrane CENTRAL: require
institutional access that is not available. Their omission is acceptable
for an integrative review; documented as a limitation. If access is
obtained, they will be incorporated as an additional declared source.

## PRISMA count (actual n — 2026-09-09)

| Stage | n |
|---|---|
| Channel A PubMed — prostate / bladder / kidney / penis / testis | 202 / 171 / 199 / 14 / 57 |
| Channel A — gross subtotal | 643 |
| Channel A — unique after PMID dedup | 632 |
| Channel B Europe PMC — prostate / bladder / kidney / penis / testis | 189 / 152 / 169 / 14 / 54 |
| Channel B — unique after internal dedup | 576 |
| Channel B — overlap with Channel A (cross-dedup by PMID/DOI) | 511 |
| Channel B — new records incorporated | 65 |
| **Combined corpus loaded into SQLite (database search closed)** | **697** |

Database search records: 697 (`REC-AIBIOGU-000001`…`000697`; 693 with
abstract), verified at intake (see `logs/workflow-log.md`).

### Database search — full text (2026-09-21)

The 536 title/abstract includes moved to retrieval
(`pending_retrieval` on 2026-09-13). On 2026-09-21 the status was no
longer pending.

| Stage | n |
|---|---|
| Text sought | 536 |
| Text not retrieved (`retrieval_status = unavailable`; decision still Pending, not evaluated) | 100 |
| Text retrieved and evaluated | 436 |
| Included | 400 |
| Excluded | 36 |
| — intervention | 19 |
| — publication type | 15 |
| — outcome | 1 |
| — population | 1 |

Of the 100 not retrieved: 94 with no institutional subscription and no
open-access copy; 1 PMC embargo until 2026-12; 1 repository with metadata
only; 1 ScienceDirect paywall; 1 Wiley paywall; 1 blocked download on
preprints.org; 1 broken handle on Helda. The reason is recorded in
`full_texts.note` for each record.

### Channel C — other methods (2026-09-23)

Source: Europe PMC. Raw links 4953; unique records 4185.

| Stage | n |
|---|---|
| Already present among the 697 | 215 |
| No title | 1 |
| New records | 3969 |
| Excluded by title filter (GU site and AI/radiomics/biomarker method) | 3279 |
| Screened at title and abstract | 690 |
| Excluded at title and abstract | 348 |
| — publication type | 199 |
| — outcome | 71 |
| — intervention | 70 |
| — population | 8 |
| Text not retrieved; eligibility unresolved | 5 |
| Included at title and abstract, text sought | 337 |
| Text not retrieved among those includes | 230 |
| Text retrieved and included as a study | 107 |
| **Studies included via other methods** | **107** |
| Studies included via database search (FT 2026-09-21) | 400 |
| **Studies included in the review** | **507** |

The 5 unresolved are: *J Urol* 2000 (10.1016/s0022-5347(05)67948-7),
*IEEE TBME* 2015 (10.1109/tbme.2015.2485779), prostatic nuclear
architecture 2017 (no PMID or DOI), *Radiology: AI* 2025
(10.1148/ryai.230555), and *The Prostate* 2026 (10.1002/pros.70088). The
remaining 230 remain as title/abstract includes with no open-access PDF.

## Deduplication and intake

- Cross-deduplication by DOI/PMID before the final count.
- Intake to SQLite with `python3 scripts/review_intake_ris.py --review
  reviews/AIBIO_GU` (prefix `REC-AIBIOGU-000001`…).
- `import_batch_id` as generated by the script, with no manual editing.

## Screening

- Double, independent title/abstract screening: R1 = `REV-ALCIDES`, R2 =
  `REV-PAOLA`.
- Documented conflict consensus; kappa reported.
- Full text: double, with controlled exclusion reasons
  (`exclusion_reasons`).
- RIS catalog of the `Included` corpus for Paperpile when moving from
  consensus to full-text retrieval (see `manual/full-text-catalog-ris.md`).

## Critical appraisal

- **MMAT v.2018** (Mixed Methods Appraisal Tool) as the primary
  instrument, given the expected design heterogeneity (as in HPV_PSCC).
- Specific complement for predictive-model studies: **PROBAST** /
  **TRIPOD-AI** domains as targeted appraisal of risk of bias and
  applicability of the AI models. Recorded per study.

## Extraction

Wide form in `extraction/`. Minimum fields: tumor site, design,
n, data source (own cohort / TCGA / CPTAC / other), data
modality, AI task, architecture/algorithm, input features, resulting
biomarker, clinical purpose, performance metric and value, validation
(internal/external/utility), code and data availability, declared
reporting guideline, funding, and conflicts of interest.

## Synthesis

Narrative integrative synthesis (Whittemore & Knafl): reduction, display,
comparison, and conclusions, stratified by **tumor site** and by **data
modality**. No meta-analysis planned (outcome and metric heterogeneity).
Modality × purpose × site mapping tables.

## Handoff

Upon closing the FT + appraisal + extraction corpus, export the package
with `python3 scripts/export_handoff_sesion2.py --review reviews/AIBIO_GU`
(see `manual/handoff-sesion2.md`). REDACTOR Session 2 drafts the
manuscript in English.

## Status

Protocol approved (2026-09-09). Database search closed (697). T/A
screening closed (536 Included). FT of that cohort closed 2026-09-21
(**100 not retrieved / 400 Included / 36 Excluded**). Appraisal and
extraction of those 400 closed. Channel C closed 2026-09-23: **107**
studies added with text, **230** included at title and abstract without
a PDF, **5** eligibilities without text. Corpus for synthesis: **507**
studies. Mapping synthesis in
`analysis/AIBIO_GU_synthesis_summary_2026-09-23.md`.

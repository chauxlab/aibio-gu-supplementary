# Supplementary material — AI-Driven Biomarker Discovery in Genitourinary Oncology

This repository is the supplementary material for the integrative review:

> **Artificial Intelligence–Driven Biomarker Discovery in Genitourinary Oncology: An Integrative Review**

It documents the full review process behind the manuscript — protocol, search strategies, screening decisions, data extraction, and critical appraisal — for the complete corpus of **507 included studies**, of which the manuscript itself cites a curated, representative subset (67 references; see `manuscript/`).

No full-text PDFs are hosted in this repository. Every included and excluded study is identified by DOI and/or a direct link to the publisher/repository record (`primary_url` column in the screening files), so the original article can be retrieved from its source of record.

## Contents

| Folder | Contents |
|---|---|
| `protocol/` | Full review protocol: PCC framework, eligibility criteria, search equations, appraisal and extraction plan. |
| `search/` | Combined database search corpus (MEDLINE + Europe PMC, RIS format, n=697 unique records) and the citation-tracking (backward/forward) search that identified 107 additional includes. |
| `screening/` | Title/abstract and full-text screening outcomes: `included-studies.csv` (n=507, with DOI and source link) and `excluded-full-text.csv` (n=36, with exclusion reason). |
| `prisma/` | `prisma-counts.csv` — exact n at every PRISMA 2020 stage, both search arms (database search and citation tracking), including the 100 database records for which full text could not be retrieved (with reason). |
| `extraction/` | Full data-extraction dataset (n=507) and its codebook. |
| `risk_of_bias/` | Full critical-appraisal dataset (MMAT 2018 + PROBAST + TRIPOD+AI, n=507) and its codebook. |
| `logs/` | Decision log and workflow log documenting methodological decisions made during screening, appraisal, and extraction (both authors acted as the two independent reviewers referenced throughout). |
| `manuscript/` | The submitted manuscript (Markdown) and the RIS file for the 67 references actually cited in its text. |
| `AIBIO_GU_synthesis_summary.md` | Narrative synthesis and mapping tables (tumor site × data modality × clinical purpose) underlying the manuscript's Results section. |

## Review design

- **Type:** Integrative review (Whittemore & Knafl, 2005).
- **Databases:** MEDLINE (via PubMed) and Europe PMC, searched independently by tumor site on 2026-09-09; a supplementary citation-tracking search (Europe PMC) was executed on 2026-09-23.
- **Screening:** Independent double screening (title/abstract and full text) by the two authors, with consensus resolution of discrepancies.
- **Appraisal:** MMAT 2018 for all studies; PROBAST and TRIPOD+AI additionally applied to prediction-model studies.
- **Corpus:** 507 studies included (400 from database searching, 107 from citation tracking) out of 697 unique database records and 4,185 unique citation-tracking records screened.

## Citing this repository

If you reuse this material, please cite the manuscript above and, if a persistent identifier is needed for the dataset itself, the archived release of this repository (see the Zenodo badge, once available).

## License

Data and documentation in this repository are released under [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/). No third-party copyrighted full texts are included.

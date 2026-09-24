# Mapping synthesis — AIBIO_GU

Date: 2026-09-23. Corpus: 507 studies with text (400 from database searching and 107 from Channel C). Extraction: 338 `complete`, 169 `needs_review`. No meta-analysis.

## Reduction

The corpus is concentrated in three sites and two modalities. Kidney 181, prostate 174, and urothelium/bladder 133 add up to 488 of 507. Testis contributes 13, penis 1 (`REC-AIBIOGU-000677`), one study covers all three renal carcinoma subtypes (`REC-AIBIOGU-000544`), and 4 are pan-cancer with a genitourinary component.

The dominant modality is radiological imaging (217) and transcriptomics (164). Next are multiomics (66), digital pathology (23), other (19), proteomics (8), methylation (6), and genomics (4).

The clinical purpose is prognostic in 258, diagnostic in 187, risk stratification in 21, response prediction in 19, subtyping in 17, and other in 5.

## Deployment: site × modality

| Site | Radiology | Transcriptomics | Multiomics | Digital pathology | Other | Proteomics | Methylation | Genomics | Total |
|---|---:|---:|---:|---:|---:|---:|---:|---:|---:|
| Kidney | 75 | 62 | 29 | 5 | 5 | 1 | 1 | 3 | 181 |
| Prostate | 86 | 48 | 17 | 8 | 6 | 6 | 2 | 1 | 174 |
| Urothelium/bladder | 47 | 52 | 16 | 9 | 5 | 1 | 3 | 0 | 133 |
| Testis | 9 | 0 | 2 | 1 | 1 | 0 | 0 | 0 | 13 |
| Other / pan-cancer | 0 | 1 | 1 | 0 | 2 | 0 | 0 | 0 | 4 |
| Penis | 0 | 0 | 1 | 0 | 0 | 0 | 0 | 0 | 1 |
| Multiple (renal subtypes) | 0 | 1 | 0 | 0 | 0 | 0 | 0 | 0 | 1 |
| Total | 217 | 164 | 66 | 23 | 19 | 8 | 6 | 4 | 507 |

## Deployment: site × purpose

| Site | Prognostic | Diagnostic | Risk | Response | Subtyping | Other | Total |
|---|---:|---:|---:|---:|---:|---:|---:|
| Kidney | 106 | 57 | 5 | 4 | 6 | 3 | 181 |
| Prostate | 78 | 74 | 12 | 5 | 4 | 1 | 174 |
| Urothelium/bladder | 71 | 43 | 4 | 9 | 5 | 1 | 133 |
| Testis | 1 | 11 | 0 | 0 | 1 | 0 | 13 |
| Other / pan-cancer | 2 | 1 | 0 | 1 | 0 | 0 | 4 |
| Penis | 0 | 1 | 0 | 0 | 0 | 0 | 1 |
| Multiple (renal subtypes) | 0 | 0 | 0 | 0 | 1 | 0 | 1 |
| Total | 258 | 187 | 21 | 19 | 17 | 5 | 507 |

## Comparison

Imaging carries the most weight in prostate (86/174) and kidney (75/181). In urothelium, transcriptomics (52) exceeds radiology (47). Testis is almost exclusively imaging diagnosis (9 of 13 radiological; 11 of 13 diagnostic). Response prediction is the thinnest purpose across the three large sites (9 bladder, 5 prostate, 4 kidney).

Validation of the number underlying performance: external cohort 206, external multicenter 52, internal hold-out 142, cross-validation 61, single split 38, none 7, demonstrated clinical utility 1 (`REC-AIBIOGU-000451`). Some external validation is present in 258 of 507.

Code available in 21, upon request in 4, not available in 3, not reported in 479. Data available in 77, upon request in 64, not available in 4, not reported in 362. Reporting guideline declared in 15 (TRIPOD 7, TRIPOD-AI 1, STARD 1, CLAIM 1, other 5); 492 declare none.

Overall MMAT (n=507): low 317, moderate 177, high 13. PROBAST overall risk: high 463, unclear 42. Two studies have no PROBAST or TRIPOD-AI block (`REC-AIBIOGU-000079`, `REC-AIBIOGU-000502`). TRIPOD-AI in the remaining 505: adequate 236, partial 263, inadequate 6.

## Conclusion

What the literature has produced is a map of prognostic and diagnostic signatures, mostly from imaging and transcriptomics, in kidney, prostate, and urothelium. Readiness for translation is narrow: a single demonstrated clinical utility, code is almost never available, and PROBAST risk is high in 463 of 505 judgments. Penis and testis remain evidence gaps. Left outside this map are 230 Channel C includes without a PDF and 5 records whose eligibility could not be closed without the text.

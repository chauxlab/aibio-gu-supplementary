# Extraction codebook — AIBIO_GU

Wide-form CSV, one row por `record_id` FT Included (n=400). Valores/notas
en inglés (idioma del manuscrito). `NR` = not reported; `NA` = not
applicable.

Review: *Artificial Intelligence–Driven Biomarker Discovery in
Genitourinary Oncology* (`integrative_review`, `SR-AIBIO-GU`). Campos
mínimos según `protocol/protocol.md` §Extracción.

Cohorte: `extraction/AIBIO_GU_ft_included_extraction_cohort_2026-09-23.csv`
(400 registros, con `pdf_path` y campos de appraisal ya cerrados
—`mmat_overall`, `applies_prediction_appraisal`,
`probast_overall_rob`, `appraisal_note`— llevados como contexto, sin
volver a juzgarlos aquí).

## Core rules

1. Campos numéricos: números planos (sin unidades ni `%` en columnas
   numéricas; porcentajes van en campos `*_pct` como números 0–100).
2. Un estudio puede reportar **más de un** biomarcador/modelo; si hay
   varios ejes claramente distintos (p.ej. firma genómica + firma
   radiómica en el mismo paper), extraer el eje **principal** declarado
   por los autores (el que ancla el título/abstract) y anotar los
   secundarios en `other_key_outcomes`. No desdoblar en filas.
3. `extraction_status`: `complete` | `partial` | `needs_review`.
4. `needs_review`: `yes` | `no` — marcar `yes` ante ambigüedad de alcance,
   texto insuficiente, o posible solapamiento de cohorte con otro
   registro del corpus (usar `appraisal_note` heredado como pista).

## Minimum for `complete`

`study_label`, `tumor_site`, `study_design`, `data_source`, `n_analytic`,
`data_modality`, `ai_task`, `biomarker_result`, `clinical_purpose`,
`performance_metric` + `performance_value` (o `NR` justificado),
`validation_level`.

## Column dictionary

| Column | Meaning / controlled vocabulary |
|---|---|
| record_id | Canonical ID |
| study_label | AuthorYear |
| tumor_site | `prostate` \| `bladder_urothelial` \| `kidney_renal` \| `penile` \| `testicular_gct` \| `multiple` (listar en `tumor_site_detail`) \| `other` |
| tumor_site_detail | Free text si `multiple`/`other` |
| study_design | Free text (`retrospective cohort`, `prospective cohort`, `case-control`, `cross-sectional`, `translational/bioinformatic`, etc.) |
| data_source | `own_cohort` \| `TCGA` \| `CPTAC` \| `GEO` \| `other_public` \| `multiple_public` \| `public_plus_own` |
| data_source_detail | Nombre(s) de cohorte/dataset(s), país/centro si aplica |
| n_analytic | N analítico (pacientes o muestras usadas en el análisis final) |
| n_unit | `patients` \| `samples` \| `slides` \| `images` \| `other` |
| data_modality | `digital_pathology` \| `radiology_imaging` \| `genomics` \| `transcriptomics` \| `proteomics` \| `epigenomics_methylation` \| `multiomics` \| `other` |
| data_modality_detail | Free text (p.ej. `WSI H&E`, `mpMRI`, `scRNA-seq`, `WGBS`) |
| ai_task | `biomarker_discovery_association` \| `classification_subtyping` \| `prognostic_prediction` \| `diagnostic_prediction` \| `treatment_response_prediction` \| `risk_stratification` \| `feature_selection_only` \| `other` |
| algorithm | Free text, lista de algoritmos/arquitecturas principales (p.ej. `LASSO Cox, random forest, XGBoost`; `CNN (ResNet50)`) |
| input_features | Breve resumen del espacio de entrada (p.ej. `12-gene expression signature`, `WSI tile-level deep features`, `radiomic texture features from T2WI/ADC`) |
| biomarker_result | Biomarcador/firma resultante, nombrado tal como lo reportan los autores |
| clinical_purpose | `diagnostic` \| `prognostic` \| `predictive_response` \| `risk_stratification` \| `subtyping` \| `other` |
| performance_metric | `AUC` \| `C-index` \| `HR` \| `sensitivity_specificity` \| `accuracy` \| `other` \| `NR` |
| performance_value | Valor(es) numérico(s) o resumen breve tal como reportado (texto libre si hay varios) |
| validation_level | `internal_holdout` \| `internal_cv` \| `internal_split_only` \| `external_cohort` \| `external_multicenter` \| `clinical_utility_demonstrated` \| `none` |
| validation_detail | Breve nota (p.ej. `70/30 split of same TCGA cohort, no external set`) |
| code_availability | `yes` \| `no` \| `upon_request` \| `NR` |
| data_availability | `yes` \| `no` \| `upon_request` \| `NR` |
| reporting_guideline | `TRIPOD-AI` \| `TRIPOD` \| `CLAIM` \| `DECIDE-AI` \| `STARD` \| `other` \| `none_stated` |
| funding | Fuente de financiamiento (free text; `NR` si no declarado) |
| conflicts_of_interest | Declaración de COI (free text; `none_declared` \| `NR`) |
| key_finding | Hallazgo principal en 1–2 frases (desempeño + relevancia clínica declarada) |
| limitations_as_stated | Limitaciones declaradas por los autores (free text breve) |
| other_key_outcomes | Ejes secundarios (otros biomarcadores/modelos en el mismo paper), notas de contexto |
| extractor_id | Extractor |
| extracted_at | Fecha `YYYY-MM-DD` |
| extraction_status | Controlled (ver arriba) |
| extraction_note | Notas/caveats del extractor |
| needs_review | `yes` \| `no` |
| pilot_batch | `pilot` \| vacío (escala) \| id de lote (`batch-01`…) |

## Notas de alcance (heredadas del protocolo/appraisal)

- Los `appraisal_note` de 251/400 filas (overfitting/AUC implausible,
  posible solapamiento de cohortes entre papers gemelos, preprints no
  revisados por pares, validación "externa" que es en realidad split
  interno) son relevantes aquí: al extraer `validation_level`, describir
  el diseño real (p.ej. split interno del mismo TCGA) en vez de aceptar
  la etiqueta "external" del paper si el appraisal ya la cuestionó.
- Posible solapamiento de cohortes públicas (mismo TCGA/GEO reutilizado
  en varios papers "gemelos"): documentar en `other_key_outcomes` /
  `extraction_note` cuando sea evidente por `data_source_detail`
  coincidente + mismo sitio tumoral + mismo año — insumo para la síntesis
  (no se excluye ni se fusiona en extracción).

## Proceso operativo

1. **Pilot (n=20):** cerrado 2026-09-23 — ver
   `AIBIO_GU_extraction_PILOT_SUMMARY.md`. Calibración adoptada abajo.
2. **Escala:** lotes paralelos (patrón FT R1 / appraisal) sobre los 400,
   codebook congelado tras el piloto.
3. Extracción asistida por PDF completo (`screening/full_text/`) por
   defecto — no depender del `.txt` parcial del piloto (varios cortan
   antes de Funding/COI/Data-availability).
4. Import a SQLite (`extractions`, una fila por variable) al cerrar,
   vía script análogo a `import_aibio_gu_appraisal.py`.

### Calibración post-pilot (congelar)

1. **`n_analytic` en diseños multi-etapa** (p.ej. scRNA-seq + cohorte
   bulk, o meta-atlas de célula única junto a decenas de cohortes
   bulk): reportar el N de la etapa que sostiene directamente el
   `performance_value` del eje principal; el resto de las etapas se
   documenta en `data_source_detail`, no en `n_analytic`.
2. **`validation_level`**: `internal_holdout` = split único train/test
   sin CV explícito; `internal_cv` = validación cruzada explícita
   (k-fold, LOOCV, etc.), aunque sea sobre una sola cohorte.
   `validation_detail` debe señalar explícitamente cuándo la cohorte
   "externa" (`external_cohort`/`external_multicenter`) **no cubre**
   el subgrupo tumoral GU de interés del estudio (validación externa
   genuina pero irrelevante para el eje GU extraído) — en ese caso
   degradar `validation_level` al nivel real que aplica al subgrupo GU
   (p.ej. `internal_split_only` si no queda otra evidencia).
3. **Estudios pan-cáncer/pan-enfermedad** donde el tumor GU es un
   subconjunto minoritario del corpus del paper (p.ej. comorbilidad
   con otra enfermedad, panel pan-cáncer de biomarcador/microbioma):
   `tumor_site=other`, con el alcance real (sitio(s) GU cubiertos +
   contexto pan-cáncer) descrito en `tumor_site_detail`.
   `data_source=public_plus_own` se reserva para cuando los datos
   propios son parte del entrenamiento/validación del modelo; si son
   solo validación ortogonal (IHC/qPCR de un hallazgo ya derivado de
   datos públicos), `data_source` refleja la fuente del modelo
   (`TCGA`/`other_public`/etc.) y la validación ortogonal se anota en
   `validation_detail`.
4. **Métricas co-primarias**: cuando el estudio reporta una métrica de
   selección de modelo (p.ej. C-index para elegir entre decenas de
   combinaciones algoritmo×dataset) y una métrica de desempeño final
   por cohorte (p.ej. AUC), `performance_metric` prioriza la métrica de
   selección; `performance_value` puede listar ambas separadas por `;`
   con su cohorte/contexto breve.
5. **`ai_task`/`clinical_purpose` con eje ancla no-IA**: en papers cuyo
   título/argumento central es mecanístico o toxicológico y el
   componente de IA/ML es secundario y tangencial, extraer igual el
   componente de IA bajo el alcance de la SR (no excluir — ya pasó FT),
   y documentar en `extraction_note` que el ancla real del paper es
   otra; `needs_review=yes` en estos casos.
6. Extraer siempre contra PDF completo cuando el `.txt` parcial no
   cubra Funding/COI/Data-availability, en vez de dejar `NR` por
   defecto.

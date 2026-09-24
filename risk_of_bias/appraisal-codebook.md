# Appraisal codebook — AIBIO_GU

Instrumentos del protocolo (`protocol/protocol.md`):

1. **MMAT v.2018** — appraisal primario (heterogeneidad de diseños).
2. **PROBAST** (dominios) + **TRIPOD-AI** (adherencia de reporte, forma
   reducida) — complemento **solo** cuando el estudio desarrolla o valida
   un modelo predictivo / firma de riesgo / clasificador con desenlace
   clínico.

Cohorte: los **400** `Included` del consenso FT
(`screening/conflicts/2026-09-21__consenso-FT-final-436.csv`).

Plantilla operativa: CSV ancho en este directorio. Importación a SQLite
(`risk_of_bias`) al cerrar el appraisal (filas por dominio).

---

## 1. MMAT v.2018

Misma lógica que `reviews/HPV_PSCC/risk_of_bias/HPV_PSCC_mmat_codebook.md`.

### Categorías (`mmat_category`)

| Code | When |
|---|---|
| `qualitative` | Diseños cualitativos |
| `rct` | Ensayos aleatorizados |
| `non_randomized` | Cohorte / casos-controles / comparativo no aleatorizado |
| `descriptive` | Serie de casos / prevalencia / descriptivo de un brazo |
| `mixed_methods` | Métodos mixtos |
| `NA` | Revisión, comentario, methods-only sin datos primarios (no debería quedar en Included FT; si aparece → documentar) |

### Screening (empíricos)

- `screen_clear_question`: `yes` \| `no`
- `screen_data_address_question`: `yes` \| `no`

Si alguno es `no` → no completar `c1`–`c5`; `overall_judgment=cannot_appraise`.

### Criterios `c1`–`c5`

Juicios: `yes` \| `no` \| `cant_tell`.  
Racional breve en `rationale_c1`…`rationale_c5`.  
Significado por categoría: Hong et al., MMAT 2018.

### Overall MMAT (`mmat_overall`)

`high` \| `moderate` \| `low` \| `cannot_appraise` \| `NA`

Heurística (empíricos), aplicada **mecánicamente** por conteo de `c1`–`c5`
(regla congelada tras detectar inconsistencia entre lotes de escala —ver
`AIBIO_GU_appraisal_PILOT_SUMMARY.md` §Corrección post-escala):

- `high`: 0 criterios `no`/`cant_tell` (todos `yes`)
- `moderate`: exactamente 1 criterio `no`/`cant_tell`
- `low`: 2 o más criterios `no`/`cant_tell`
- `cannot_appraise`: falló screening o texto insuficiente

Los 10 casos `high` del corpus final (0 no/cant_tell, todos `yes`) son los
diseños de mayor rigor metodológico (p.ej. REC-AIBIOGU-000032,
REC-AIBIOGU-000232: validación externa multicéntrica genuina) — no son
excepciones a la regla, sino el resultado natural del conteo aplicado a
estudios excepcionalmente bien diseñados. `appraisal_note` documenta el
razonamiento cualitativo caso por caso, pero `mmat_overall` se deriva
siempre del conteo, sin ajustes discrecionales.

---

## 2. ¿Aplica PROBAST / TRIPOD-AI?

`applies_prediction_appraisal`: `yes` \| `no`

Marcar **`yes`** si el paper:

- desarrolla, valida o actualiza un **modelo predictivo** (diagnóstico,
  pronóstico, respuesta a terapia, estratificación de riesgo), **o**
- reporta una **firma/score** usada como predictor de un desenlace clínico
  con métricas tipo AUC / C-index / sensibilidad-especificidad / calibración.

Marcar **`no`** (dejar dominios PROBAST/TRIPOD-AI vacíos o `NA`) si es:

- descubrimiento puramente asociativo / diferencial expression sin modelo
  predictivo formal,
- solo selección de features / ranking sin evaluación predictiva de
  desenlace,
- métodos, pipeline técnico o recurso sin validación predictiva clínica.

Ante duda → `yes` y documentar en `appraisal_note` (regla alineada al
cribado: preferir sobre-aplicar el complemento a perderlo).

---

## 3. PROBAST (dominios dirigidos)

Juicios por dominio: `low` \| `high` \| `unclear`  
(equivalente PROBAST: bajo / alto / poco claro riesgo de sesgo).

| Columna | Dominio |
|---|---|
| `probast_participants` | Participantes / fuente de datos / elegibilidad |
| `probast_predictors` | Predictores (definición, medición, disponibilidad al momento de predicción) |
| `probast_outcome` | Desenlace (definición, determinación, ceguera relativa a predictores) |
| `probast_analysis` | Análisis (eventos por variable, manejo de missing, overfitting, validación) |
| `probast_overall_rob` | Juicio global de riesgo de sesgo del modelo |
| `probast_overall_applicability` | Aplicabilidad al PCC de AIBIO_GU: `low` concern \| `high` concern \| `unclear` |

Heurística de `probast_overall_rob`:

- `low` si **todos** los dominios 1–4 son `low`
- `high` si **algún** dominio es `high`
- `unclear` en cualquier otro caso (incluye mezclas con `unclear`)

Racional corto opcional en `probast_rationale` (señales clave, no ensayo).

---

## 4. TRIPOD-AI (reporte reducido)

No es el checklist completo de 27+ ítems. Cuatro señales de reporte
relevantes para síntesis integrativa de modelos de IA:

| Columna | Pregunta (yes / partial / no / NA) |
|---|---|
| `tripod_ai_data` | ¿Fuente de datos, elegibilidad y handling de missing descritos con claridad? |
| `tripod_ai_model` | ¿Arquitectura/algoritmo, features de entrada y procedimiento de entrenamiento/tuning descritos? |
| `tripod_ai_validation` | ¿Validación interna y/o externa descrita (método + métricas)? |
| `tripod_ai_transparency` | ¿Código y/o datos disponibles, o justificación explícita de no disponibilidad? |
| `tripod_ai_overall` | `adequate` (≥3 yes) \| `partial` (1–2 yes, resto partial/no) \| `inadequate` (0 yes) \| `NA` |

---

## 5. Proceso operativo

1. **Pilot (n=20):** cerrado 2026-09-21 — ver
   `AIBIO_GU_appraisal_PILOT_SUMMARY.md`. Calibración adoptada abajo.
2. **Escala:** lotes paralelos (patrón FT R1) con este codebook congelado.
3. Appraisal asistido por PDF (texto pp. iniciales + Methods/Results
   según necesidad). Marcar `appraisal_note` si el juicio es fronterizo.
4. Revisor nominal R1: `REV-ALCIDES`. Doble appraisal R2: diferido; si no
   hay R2 completo, muestreo de verificación post-escala.

### Calibración post-pilot (congelar)

- En este corpus, `applies_prediction_appraisal=yes` es la regla casi
  general (firmas/modelos); marcar `no` solo con evidencia clara de paper
  puramente asociativo / sin evaluación predictiva.
- `mmat_overall=high` será excepcional (bases públicas + minería ML).
- Dominio PROBAST Analysis en `high` es frecuente y esperado en firmas
  omics con selección masiva de features/algoritmos.
- PDF con cuerpo en chino: appraisear si abstract/methods en inglés son
  suficientes; anotar en `appraisal_note` (precedente REC-000536).
- Concern de aplicabilidad (comorbid, pan-cancer, proxy de exposición):
  no reclasifica elegibilidad; va a `probast_overall_applicability` +
  `appraisal_note`.

---

## 6. Columnas del CSV

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

- `pilot_batch`: `pilot` \| vacío (escala) \| id de lote (`batch-01`…).
- Campos de juicio vacíos = pendiente.

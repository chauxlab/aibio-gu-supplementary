# Decision log — AIBIO_GU

## 2026-09-09 — Creación del expediente y protocolo

- Se creó el expediente `AIBIO_GU` con `scripts/create_review_scaffold.sh`.
- Tema: *Artificial Intelligence–Driven Biomarker Discovery in Genitourinary
  Oncology*. Revisión **integrativa** (Whittemore & Knafl), manuscrito en
  **inglés**, sin registro externo (PROSPERO no aplica; OSF opcional).
- `review.yaml` editado: `review_id: SR-AIBIO-GU`,
  `review_type: integrative_review`, `status: active`,
  `r1_reviewer_id: REV-ALCIDES`, `r2_reviewer_id: REV-PAOLA`.
- Directorio de intake renombrado `rayyan_initial_2026-09-09` →
  `intake_2026-09-09` (convención de `review_intake_ris.py`).
- **Alcance definido con el usuario (2026-09-09):**
  - Población: todos los sitios GU (próstata, urotelio/vejiga, riñón, pene,
    testículo).
  - Concepto: todas las modalidades de biomarcador guiadas por IA
    (patología digital, radiómica/imagen, -ómicas, multiómica).
  - Desenlace: cualquier propósito clínico (diagnóstico, pronóstico,
    predicción de respuesta).
- Marco: **PCC** con desenlaces declarados.
- Appraisal previsto: **MMAT v.2018** + dominios **PROBAST/TRIPOD-AI** para
  modelos predictivos.
- `protocol/protocol.md` redactado. **Aprobado por el usuario** ("aprobado, avanzar").

## 2026-09-09 — Decisiones de búsqueda

- **Ajuste de ecuación (a pedido del usuario):** la ecuación amplia daba
  ~1.500 únicos (próstata 767 / vejiga 408 / riñón 457). Se restringió
  exigiendo lenguaje de descubrimiento de biomarcador
  (`"biomarker discovery"`, `"novel biomarker"`, `"prognostic signature"`,
  `"gene signature"`, `"radiomic signature"`, `nomogram`,
  `"molecular subtype"`) en próstata/vejiga/riñón → 202/171/199.
- **Pene y testículo:** se mantuvo sin bloque de biomarcador (densidad muy
  baja: 14 y 57) y con bloque de IA ampliado; la relevancia se decidirá en
  cribado.
- **Búsqueda dividida por sitio tumoral** por el límite de 20 operadores
  booleanos del conector PubMed; unión por PMID.
- **Canal B (Europe PMC) diferido** a una sesión futura; se documentará como
  enmienda al ejecutarlo. El corpus actual es solo Canal A (PubMed).
- Criterio de exclusión central confirmado por el usuario: se excluyen
  modelos de IA que solo usan biomarcadores establecidos como predictores y
  modelos de imagen/patología de solo detección/segmentación/diagnóstico
  sin derivación de biomarcador.

## 2026-09-09 — Canal B: disponibilidad de Europe PMC y decisión

- **Consulta del usuario:** por qué Europe PMC "no está disponible" si se usó
  en revisiones previas.
- **Hallazgo:** el proyecto REVISOR no tiene conector MCP de Europe PMC
  (solo PubMed y Scite). Las búsquedas Europe PMC de revisiones previas
  (HPV_PSCC, etc.) se ejecutaron en el proyecto **REDACTOR** y REVISOR
  recibió el RIS ya construido. Esta es la primera revisión cuya búsqueda se
  ejecuta íntegramente dentro de REVISOR.
- **Decisión:** ejecutar el Canal B contra la **API REST pública de Europe
  PMC** (EBI, sin autenticación), que da consultas reproducibles con
  sintaxis `TITLE:`/`ABSTRACT:`. Alternativa a mover la búsqueda a REDACTOR.
- **Scite descartado** como sustituto de Europe PMC: busca texto completo,
  baja precisión, muestreo no reproducible. Reservado para rastreo de vacíos.
- **Embase / Scopus / Web of Science:** omitidos por falta de acceso
  institucional; aceptable para revisión integrativa, documentado como
  limitación.
- **Preprints** (65 del Canal B): se retienen para cribado; regla de
  sustitución preprint→publicado documentada en el protocolo.

## 2026-09-09 — Cierre de la búsqueda en bases de datos

- Se evaluó ampliar a OpenAlex / Semantic Scholar / SciELO / arXiv.
- **Decisión (usuario):** cerrar la búsqueda en dos canales (PubMed + Europe
  PMC), a la par del estándar de HPV_PSCC / ctDNA_GU / GLP1. Motivos:
  elegibilidad restringida a cohorte humana GU + derivación de biomarcador
  (literatura indexada en MEDLINE/PMC); solapamiento Canal A/B del 89 %
  (saturación); revisión integrativa (listón multi-base más bajo).
- **Canal C = rastreo de citas** (hacia atrás y adelante) sobre incluidos y
  revisiones recuperadas, tras el cribado de texto completo, más literatura
  del usuario. Se declarará como "otros métodos" en PRISMA.
- Limitación documentada: Embase/Scopus/WoS (sin acceso) y agregadores no
  ejecutados.

## 2026-09-13 — Consenso de conflictos T/A (105 discrepancias R1/R2)

- **Consultado al usuario** antes de resolver, por tratarse de decisiones
  metodológicas que afectan bloques grandes de registros:
  1. **Regla para el cluster de radiómica sin algoritmo de selección de
     features tipo ML explícito** (~35-40 registros, patrón
     R1=`Maybe`/R2=`Included`). **Decisión del usuario:** delegada a
     criterio propio ("decidir según tu mejor criterio y avanzar"). Se
     adoptó: la extracción/firma radiómica en sí satisface el Concepto
     IA/ML del protocolo (consistente con `radiomics[tiab]` como término
     del bloque IA/ML en la ecuación de búsqueda) → `Included` para ese
     patrón. Extendido por analogía a patómica (REC-000387) y a pipelines
     algorítmicos de multiómica sin ML supervisado clásico (REC-000073,
     -000237).
  2. **REC-AIBIOGU-000678** (coautoría Chaux A., nota de COI explícita en
     el HANDOFF-R2, discrepancia mayor R1=`Excluded`/R2=`Included`).
     **Decisión explícita del usuario:** `Excluded`/`wrong intervention` —
     se mantiene el criterio de R1 (la IA solo correlaciona PD-L1/CD8, ya
     establecidos, sin derivar biomarcador nuevo). Documentado como
     resolución de consenso motivada por el conflicto de interés, no por
     desempate automático ni por descarte del registro.
  3. **Resto de las discrepancias (~65 registros):** el usuario pidió
     resolución caso por caso aplicando la "regla de oro" del HANDOFF-R2.
     Detalle completo de cada decisión y su justificación en
     `logs/workflow-log.md` (entrada 2026-09-13) y en el archivo
     `screening/conflicts/2026-09-13__consenso-TA-R1-R2.csv`.
- **Resultado inicial:** de las 105 discrepancias, 52 `Included`, 33
  `Excluded`, 20 `Maybe`.
- **Usuario aprobó avanzar** ("aprobado, avanzar"). Al preparar la
  importación se detectó que `import_ta_screening_decisions.py` exige
  consenso **binario** para los 697 registros (no admite `Maybe`, y el CSV
  de consenso debe cubrir todo el corpus, no solo los 105 conflictos) —
  precedente ya establecido en HPV_PSCC. Se resolvieron a binario los 20
  `Maybe` del archivo de conflictos más 15 registros adicionales donde R1
  y R2 coincidían en `Maybe` (concordantes, sin conflicto). Detalle y
  justificación por registro en `logs/workflow-log.md` (entrada
  "Ajuste a consenso binario e importación a SQLite", 2026-09-13).
- **Totales finales T/A sobre los 697 registros: Included 536 / Excluded
  161.**
- **Importación ejecutada** con backup previo de SQLite.
  `import_batch_id=IMP-SR-AIBIO-GU-2026-09-13-TA-SCREEN`; 536 registros
  pasaron a `full_texts.retrieval_status = pending_retrieval`, 161 a
  `not_required_after_initial_screening`.

## 2026-09-21 — Appraisal: andamiaje + pilot (n=20)

- Usuario aprobó plan andamiaje → pilot → escala.
- Artefactos en `risk_of_bias/`:
  - `AIBIO_GU_appraisal_codebook.md` (MMAT v.2018 + PROBAST dominios +
    TRIPOD-AI reducido)
  - `AIBIO_GU_appraisal_BLANK_400.csv` (400 Included FT)
  - `AIBIO_GU_appraisal_PILOT_20.csv` + `AIBIO_GU_appraisal_PILOT_SUMMARY.md`
- Pilot estratificado (5 próstata / 5 vejiga / 5 riñón / 3 testículo /
  1 pene / 1 other). Appraisal asistido (PDF pp.1–12),
  `reviewer_id=REV-ALCIDES`.
- Resultado piloto: MMAT moderate 12 / low 8; PROBAST aplica 20/20;
  PROBAST overall RoB high 16 / unclear 4; TRIPOD-AI partial 11 /
  adequate 8 / inadequate 1.
- **Calibración congelada en codebook** (sección 5): PROBAST casi
  universal en este corpus; MMAT `high` raro; Analysis `high` esperado
  en firmas omics; PDF chino appraiseable vía abstract EN; concerns de
  aplicabilidad no reclasifican elegibilidad.
- **Siguiente:** escala sobre los 380 restantes en lotes; import SQLite
  `risk_of_bias` al cerrar.

## 2026-09-22 — Appraisal: escala completa (380) + consolidación + import SQLite

- Escala ejecutada en 8 lotes paralelos (agentes IA, ~47-48 registros
  c/u, `reviewer_id=REV-ALCIDES-AI`), sobre PDF completo en
  `screening/full_text/` (400/400 disponibles). 0 `cannot_appraise`.
- **Inconsistencia detectada al fusionar:** la heurística `mmat_overall`
  escrita en el codebook (moderate=≤1 no/cant_tell, low=2+) no
  coincidía con la práctica real del piloto (7/8 casos con 2 no/cant_tell
  calificados `moderate`). Los 8 lotes se dividieron entre ambas
  interpretaciones. **Decisión del usuario:** aplicar la regla escrita
  de forma mecánica a las 400 filas → 46 recalificadas (9 piloto + 37
  escala). Codebook corregido para eliminar la ambigüedad (sección 1).
- Normalización adicional: `probast_overall_applicability` a valores
  canónicos (`low concern`/`high concern`/`unclear`); corrección de 1
  fila (REC-AIBIOGU-000079) con nota desbordada en `tripod_ai_overall`.
- Consolidado en `AIBIO_GU_appraisal_FULL_400.csv` (400 filas únicas,
  verificado sin duplicados). Distribución final: MMAT low 258/moderate
  132/high 10; PROBAST aplica 398/400; PROBAST RoB high 356/unclear 42;
  aplicabilidad low concern 358/high concern 29/unclear 11; TRIPOD-AI
  adequate 201/partial 192/inadequate 5.
- Importado a SQLite vía `reviews/scripts/import_aibio_gu_appraisal.py`:
  400 `studies` (1:1 con records) + 7978 filas `risk_of_bias`
  (`MMAT_v2018` 400×9 dominios, `PROBAST` 398×6, `TRIPOD-AI` 398×5).
  Backup pre-import en `/tmp/reviews.sqlite.bak-before-aibiogu-appraisal-import`.
- 251/400 filas con `appraisal_note` (overfitting/AUC implausible,
  posible solapamiento de cohorte entre papers gemelos, preprints no
  revisados por pares, validación "externa" que es en realidad split
  interno, concerns de aplicabilidad) — relevante para ponderar en la
  extracción/síntesis.
- **Appraisal cerrado. Siguiente: extracción de datos sobre los 400
  Included → Canal C (rastreo de citas) → handoff Sesión 2.**

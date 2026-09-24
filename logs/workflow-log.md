# Workflow log — AIBIO_GU

## 2026-09-09 — Preparación inicial y protocolo

- Scaffold creado. `review.yaml` y `protocol/protocol.md` redactados.
- Fase actual del ciclo: **protocolo** (redactado, pendiente de aprobación).
- Siguiente fase: **búsqueda**.

## 2026-09-09 — Búsqueda Canal A (PubMed) e intake

1. Protocolo aprobado por el usuario. Ecuación ajustada para subir precisión
   (bloque de descubrimiento de biomarcador exigido en próstata/vejiga/riñón;
   pene y testículo con bloque de IA ampliado y sin bloque de biomarcador).
2. **Búsqueda PubMed vía conector NCBI**, por sitio tumoral (límite de 20
   operadores booleanos del conector obliga a dividir): próstata 202, vejiga
   171, riñón 199, pene 14, testículo 57 → **643 brutos**.
3. Deduplicación por PMID → **632 únicos**.
4. Metadatos recuperados vía `get_article_metadata` (lotes de 20; el conector
   limita a 20 artículos/llamada). RIS construido con script propio
   (`scratchpad/build_ris.py`) →
   `searches/ris/AIBIO_GU_pubmed_2026-09-09.ris` (632 entradas; 628 con
   abstract, 630 con DOI).
5. `archive/rayyan_initial_2026-09-09/` renombrado a `archive/intake_2026-09-09/`.
6. **Intake:** `python3 scripts/review_intake_ris.py --review reviews/AIBIO_GU`.
   Resultado: `RIS records=632`, `canonical_records=632`, `identifiers=1262`,
   `duplicate_candidate_rows=0`,
   `import_batch_id=IMP-SR-AIBIO-GU-2026-09-02-RIS` (fecha `TODAY` fija del
   script = 2026-09-02, no editada; fecha operativa real 2026-09-09).
   Backup previo de la base en scratchpad.
7. Verificado en SQLite: 632 registros `SR-AIBIO-GU`
   (`REC-AIBIOGU-000001`…`000632`), 628 con abstract. Reviewers
   `REV-ALCIDES`/`REV-PAOLA` reutilizados.
8. Snapshot: `data/process_snapshots/2026-09-09__initial_sqlite_intake.csv`
   (632 filas, todas `pending_screening`).
9. **Pendiente:** Canal B (Europe PMC) y Canal C.

## 2026-09-09 — Búsqueda Canal B (Europe PMC) e intake combinado

1. **Verificación de disponibilidad de Europe PMC** (a pedido del usuario):
   el proyecto REVISOR no tiene conector MCP de Europe PMC (solo PubMed y
   Scite). En revisiones previas (p. ej. HPV_PSCC) la búsqueda Europe PMC se
   ejecutó en el proyecto REDACTOR, que sí lo tenía, y REVISOR recibió un RIS
   ya deduplicado. Embase/Scopus/WoS requieren acceso institucional (no
   disponible; omisión aceptable para integrativa).
2. **Solución:** se usó la **API REST pública de Europe PMC** (EBI, sin
   autenticación), sintaxis `TITLE:`/`ABSTRACT:`, misma estructura de bloques
   que el Canal A, `SRC:MED OR SRC:PMC OR SRC:PPR`. Script:
   `scratchpad/fetch_epmc.py` (paginación por `cursorMark`).
3. Resultado: próstata 189 / vejiga 152 / riñón 169 / pene 14 / testículo 54
   → **576 únicos**. Dedup cruzado por PMID+DOI contra el Canal A: 511
   solapan, **65 nuevos** (mayoría preprints Research Square/bioRxiv/medRxiv/
   Preprints.org/SSRN; ~12 registros MEDLINE no capturados por las
   ecuaciones por sitio del Canal A).
4. **Scite descartado como canal:** `search_literature` busca texto completo,
   devuelve miles de hits/sitio con baja precisión (mayoría revisiones),
   muestreo no exhaustivo ni reproducible. No incorporado.
5. RIS combinado: `scratchpad/build_ris_combined.py` →
   `searches/ris/AIBIO_GU_pubmed_europepmc_2026-09-09.ris` (697 entradas,
   693 con abstract, 694 con DOI). El RIS solo-PubMed movido a
   `searches/ris/superseded/`. Campo `N1` marca `source_channel: A|B`.
6. **Re-intake:** `review_intake_ris.py --review reviews/AIBIO_GU` (purga y
   recarga completa). `RIS records=697`, `canonical_records=697`,
   `identifiers=1337`, `duplicate_candidate_rows=0`. Backup previo de la base
   en scratchpad (`reviews.sqlite.bak2`).
7. Verificado en SQLite: 697 registros `SR-AIBIO-GU`
   (`REC-AIBIOGU-000001`…`000697`), 693 con abstract.
8. Snapshot: `data/process_snapshots/2026-09-09__initial_sqlite_intake_v2.csv`
   (697 filas, `pending_screening`). El snapshot v1 (632) se eliminó.
9. **Búsqueda en bases cerrada** (decisión 2026-09-09): dos canales, sin
   agregadores. Canal C = rastreo de citas post-cribado FT.
10. **Siguiente fase: cribado título/abstract doble R1/R2** desde
    `initial_screening` sobre los 697.

## 2026-09-09 — Paquete de cribado T/A para R2 (Paola)

- Planillas ciegas exportadas desde SQLite (script
  `scratchpad/export_blind_screening.py`, copiado a
  `archive/intake_2026-09-09/scripts/`):
  - `screening/exports/2026-09-09__cribado-R2-britos.csv` (697 filas)
  - `screening/exports/2026-09-09__cribado-R1-chaux.csv` (697 filas, idéntica)
  - Columnas: record_id, title, year, journal, authors, doi, abstract,
    keywords, url, decision, exclusion_reason, note. 4 registros con
    `[Resumen no disponible]`.
- Instrucciones: `screening/exports/2026-09-09__HANDOFF-cribado-R2-Britos.md`
  (proyecto, pregunta, PCC, criterios inc/exc con la "regla de oro" de
  descubrimiento de biomarcador, códigos de exclusión, nota de COI por
  coautoría Chaux en REC-AIBIOGU-000678 y -000686, devolución).
- Paquete: `screening/exports/paquete-R2-britos-2026-09-09.zip` (CSV R2 +
  HANDOFF). Formato calcado del de HPV_PSCC.
- **Pendiente:** enviar el zip a Paola; R1 (Alcides) criba su planilla en
  paralelo; al volver ambas → `screening/imports/` → conflictos + consenso →
  `import_ta_screening_decisions.py`.

## 2026-09-13 — Cribado título/abstract R1 (Alcides) completado

- Cribado de los 697 registros aplicando los criterios del HANDOFF-R2
  (mismo marco PCC y reglas de exclusión). Lectura de título+abstract
  registro por registro; primeros 50 revisados directamente, 51-697
  procesados con la misma calibración (verificado con muestreo aleatorio
  post-hoc, decisiones consistentes).
- Resultado: `screening/imports/2026-09-09__cribado-R1-chaux-COMPLETO.csv`
  (697 filas, sin celdas vacías en `decision`, `exclusion_reason` válido en
  todas las `Excluded`).
- Conteo: **Included 501 / Excluded 134 / Maybe 62**.
- Desglose `exclusion_reason` (134): wrong intervention 62 (mayoría por
  combinar/clasificar con biomarcadores o scores ya establecidos —PSA,
  PI-RADS, NLR, SII, Ki-67, PD-L1, nomogramas clínicos— sin descubrir uno
  nuevo, o evaluación de chatbots/LLMs sin biomarcador), wrong publication
  type 36 (revisiones, editoriales, cartas, guías), wrong outcome 18
  (desenlaces no oncológicos o segmentación/cuantificación de métricas ya
  estandarizadas sin biomarcador nuevo), wrong population 15 (tumor no GU o
  preclínico sin cohorte humana), wrong study design 3.
- Los 62 `Maybe`: en su mayoría radiómica sin algoritmo ML explícito de
  selección de features (a resolver en texto completo), más ~8 revisiones
  sistemáticas/scoping muy relevantes al tema (marcadas para rastreo de
  referencias en vez de excluidas).
- REC-AIBIOGU-000678 y -000686 (coautoría Chaux) evaluados con el mismo
  criterio que cualquier registro (ambos Excluded por motivos metodológicos,
  no por COI).
- **Pendiente:** planilla R2 (Paola) — aún no ha vuelto (`screening/imports/`
  solo tiene el README). Al recibirla: comparar decisiones, calcular kappa,
  resolver conflictos por consenso, e importar con
  `import_ta_screening_decisions.py`.

## 2026-09-13 — Cribado título/abstract R2 (Paola/Britos) recibido y verificado

- Planilla devuelta por Paola: `screening/imports/2026-09-09__cribado-COMPLETO.csv`
  (697 filas + encabezado). Verificación estructural: cobertura completa de
  IDs `REC-AIBIOGU-000001`…`000697` (0 faltantes, 0 sobrantes), sin
  `decision` vacía/inválida, sin `Excluded` sin `exclusion_reason`.
- Conteo R2: **Included 531 / Excluded 107 / Maybe 59** (vs. R1: Included 501
  / Excluded 134 / Maybe 62).
- **Comparación R1 vs R2** (script Python ad hoc, `csv.DictReader` +
  cálculo manual de kappa de Cohen sobre las 3 categorías
  Included/Excluded/Maybe):
  - Acuerdo simple (po): 592/697 = **84.9%**.
  - **Kappa de Cohen: 0.637** (acuerdo sustancial, escala Landis-Koch).
  - Discrepancias totales: **105**, exportadas a
    `screening/conflicts/2026-09-13__conflictos-TA-R1-vs-R2.csv`
    (columnas: record_id, title, decision_R1_chaux, reason_R1,
    decision_R2_britos, reason_R2, note_R1, note_R2).
  - Discrepancias **mayores** (Included↔Excluded en extremos opuestos): 14
    registros — REC-AIBIOGU-000045, -000075, -000077, -000089, -000097,
    -000141, -000145, -000169, -000217, -000325, -000502, -000573, -000575,
    y **-000678**.
- **Nota COI:** REC-AIBIOGU-000678 (coautoría Chaux) está entre las 14
  discrepancias mayores (R1=Excluded/wrong intervention vs. R2=Included).
  Por la nota de COI del HANDOFF-R2, este registro debe resolverse por
  consenso explícito y documentado, no por desempate automático.
  REC-AIBIOGU-000686 (también coautoría Chaux) es concordante: ambos
  Excluded.
- **Pendiente — próximos pasos al reanudar esta revisión:**
  1. Resolver por consenso las 105 discrepancias del archivo
     `screening/conflicts/2026-09-13__conflictos-TA-R1-vs-R2.csv`,
     priorizando las 14 mayores (empezar por REC-AIBIOGU-000678 dado el COI).
  2. Registrar las decisiones de consenso (columna adicional o archivo de
     resolución) en `screening/conflicts/`.
  3. Con las decisiones finales (no-discrepantes + consensuadas), correr
     `import_ta_screening_decisions.py` (relocalizado a `reviews/scripts/`;
     confirmar ruta/args antes de ejecutar) para actualizar el estado en
     SQLite y pasar los `Included` a `pending_full_text` (o el estado que
     corresponda según el esquema).
  4. Tras la importación, iniciar el **cribado de texto completo (FT)** sobre
     los registros incluidos.
  5. Recordar tarea ya registrada en memoria: tras el consenso T/A, exportar
     RIS de los `Included` para catálogo Paperpile de texto completo.

## 2026-09-13 — Consenso de conflictos T/A R1 vs R2 (105 discrepancias)

- Reglas de consenso acordadas con el usuario antes de resolver:
  1. **Radiómica sin algoritmo de selección de features tipo ML explícito**
     (p. ej. solo regresión logística/Cox, sin LASSO/RF/mRMR/red neuronal
     nombrado): se decide por criterio propio que **cuenta como IA/ML** —
     la extracción/selección radiómica en sí satisface el Concepto del
     protocolo (coincide con `radiomics[tiab]` como término del bloque
     IA/ML en la ecuación de búsqueda). Extendido por analogía a
     "patómica" (heterogeneidad nuclear cuantitativa, REC-000387) y a
     pipelines algorítmicos de descubrimiento multiómico sin ML
     supervisado clásico (DEPTH, WGCNA/limma/Mfuzz: REC-000073, -000237).
  2. **REC-AIBIOGU-000678** (coautoría Chaux, nota de COI explícita en el
     HANDOFF): resuelto por decisión explícita del usuario como
     **`Excluded`/`wrong intervention`** — la IA correlaciona PD-L1/CD8
     (biomarcadores ya establecidos) sin derivar uno nuevo. Documentado
     como decisión de consenso por el COI, no por desempate automático.
     REC-AIBIOGU-000686 (también coautoría Chaux) ya era concordante
     (`Excluded` ambos revisores), sin conflicto que resolver.
  3. **Resto de los 105 conflictos:** resueltos caso por caso aplicando la
     "regla de oro" del HANDOFF-R2 (¿la IA deriva/prioriza un biomarcador
     nuevo, o solo reusa/clasifica/segmenta uno ya establecido?), con
     apoyo de las notas de ambos revisores. Reglas internas de
     consistencia notables:
     - Estudios pan-cáncer con hallazgo específico reportado para un sitio
       GU (vejiga, riñón) → `Included`; sin resultado específico por sitio
       → `Excluded`/`wrong population` (REC-000188, -240, -265, -424).
     - Paneles de índices inflamatorios/inmunes ya ampliamente establecidos
       en oncología general (NLR, SII, PLR, AGR, DRR, etc.) combinados por
       LASSO/RSF/Cox → `Excluded`/`wrong intervention` (cluster REC-000217,
       -325, -357, -396, -534), salvo REC-000141 (subpoblaciones
       linfocitarias funcionales, no estandarizadas, ML prioriza 9/42
       candidatas) → `Included`.
     - Desenlaces de toxicidad por quimioterapia (ototoxicidad,
       nefrotoxicidad, síndrome metabólico) → `Excluded`/`wrong outcome`
       (cluster REC-000588, -610, -624).
     - Cuantificación automatizada de biomarcadores ya establecidos por
       histopatología digital sin biomarcador nuevo (PTEN, Ki67/LSD1,
       PD-L1/CD8, invasión linfovascular) → `Excluded`/`wrong intervention`
       (REC-000145, -607, -618, -621, -678).
     - Revisiones muy relevantes marcadas `Maybe` por ambos criterios o por
       uno de los dos → consenso `Maybe` (contexto/rastreo de referencias,
       no como estudio incluido): REC-000012, -018, -020, -138, -194,
       -258, -491, -623, -629, -697.
     - Duplicados detectados dentro del propio corpus, marcados para
       deduplicar en texto completo: REC-000161/-000642 (mismo estudio DWI
       radiómica), REC-000496/-000668 (mismo estudio CT multifásico ccRCC).
- Archivo de resolución: `screening/conflicts/2026-09-13__consenso-TA-R1-R2.csv`
  (105 filas: `consensus_decision`, `consensus_exclusion_reason`,
  `consensus_rationale` por registro).
- **Resultado del consenso (105):** Included 52 / Excluded 33 / Maybe 20.
- **Totales finales T/A (697 = 592 concordantes + 105 consensuados):**
  concordantes Included 477 / Excluded 100 / Maybe 15; **finales: Included
  529 / Excluded 133 / Maybe 35.**

## 2026-09-15 — Catálogo RIS para Paperpile exportado

- `python3 reviews/scripts/export_full_text_ris.py --review reviews/AIBIO_GU`
  (default `--status pending_retrieval`) →
  `screening/full_text/2026-09-15__full-text-catalog.ris`.
- **536 registros, 536 con DOI, 0 sin DOI** (coincide con los 536 `Included`
  del consenso importados el 2026-09-13).
- **Pendiente:** enviar el RIS al usuario para import en Paperpile
  (identificación + descarga automatizada de PDF por DOI). Al volver
  `paperpile-files.zip`, reingresar con
  `python3 reviews/scripts/ingest_paperpile_full_texts.py --review reviews/AIBIO_GU --zip ~/Downloads/paperpile-files.zip`
  y reexportar para la 2ª pasada de los pendientes.

## 2026-09-13 — Ajuste a consenso binario e importación a SQLite

- `import_ta_screening_decisions.py` (`reviews/scripts/`) exige que la
  columna `consensus` sea **binaria** (`Included`/`Excluded`; ver línea
  167-168 del script) — no acepta `Maybe` como decisión final de consenso,
  igual que el precedente de HPV_PSCC (`consenso-TA-screening.csv` allí
  también forzó los `Maybe` a binario). Además el importador exige que el
  CSV de consenso cubra los **697** registros (no solo los 105 conflictos):
  se detectaron **15 registros adicionales** donde R1 y R2 coincidieron en
  `Maybe` (concordantes, por eso no aparecían en el archivo de conflictos)
  que también requerían resolución binaria.
- **Resolución de los 35 `Maybe` totales** (20 del archivo de conflictos +
  15 concordantes), aplicando el mismo marco de reglas:
  - **Revisiones muy relevantes → `Excluded`/`wrong publication type`**
    (contexto/rastreo de referencias, no estudio incluido, por regla
    explícita del HANDOFF): REC-000012, -018, -020, -040, -102, -138,
    -148, -194, -258, -268, -274, -372, -373, -491, -566, -569, -585,
    -623, -629, -697 (20 revisiones).
  - **`Included` (enviados a verificación de texto completo)** por
    biomarcador candidato plausible o falta de información suficiente
    para excluir a nivel de título/abstract: REC-000017, -036, -103,
    -461, -502, -574, -590 (7 registros).
  - **`Excluded` por motivo específico** (no revisión): REC-000106/-640
    (mismo estudio; variables finales del nomograma son clínicas, no
    radiómicas — `wrong intervention`), REC-000128 (ML real pero
    desenlace fuera del marco PCC — `wrong outcome`), REC-000233
    (solo estadística univariada, sin firma/score — `wrong
    intervention`), REC-000248 (abstract no describe biomarcador ni IA/ML
    pese al título — `wrong intervention`), REC-000445 (predice VEGF ya
    establecido — `wrong intervention`), REC-000539 (sin métrica
    reportada para los candidatos renales — `wrong outcome`), REC-000613
    (solo pruebas t múltiples, sin firma compuesta — `wrong
    intervention`) (8 registros).
  - Detalle completo con justificación por registro en
    `screening/conflicts/2026-09-13__consenso-TA-final-697.csv` (697 filas,
    columnas `record_id`, `consensus`, `exclusion_reason`, `note`) y en
    `2026-09-13__consenso-TA-R1-R2.csv` (105 conflictos, actualizado con
    resolución binaria).
  - Nota abierta: REC-000445 y REC-000461 (nefroblastoma/tumor de Wilms)
    comparten una duda de encaje poblacional no resuelta formalmente por
    el protocolo (neoplasia renal pediátrica embrionaria, no RCC); 000445
    se excluyó por motivo de intervención independiente de esa duda,
    000461 se envía a texto completo con la duda documentada para
    resolver en extracción/síntesis.
- **Totales finales T/A revisados: Included 536 / Excluded 161** (ajustado
  desde el reporte previo de 529/133/35, que incluía 35 `Maybe` no
  válidos para el importador).
- Backup de la base previo a la importación:
  `scratchpad/reviews.sqlite.bak-pre-ta-consensus-2026-09-13` (sesión).
- **Importación ejecutada:**
  `python3 reviews/scripts/import_ta_screening_decisions.py --review
  reviews/AIBIO_GU --r1-csv .../2026-09-09__cribado-R1-chaux-COMPLETO.csv
  --r2-csv .../2026-09-09__cribado-COMPLETO.csv --consensus-csv
  .../2026-09-13__consenso-TA-final-697.csv --decision-date 2026-09-13`.
  `import_batch_id=IMP-SR-AIBIO-GU-2026-09-13-TA-SCREEN`. Resultado:
  `updated_r1=697 updated_r2=697 consensus_inserted=697`;
  `full_texts pending_retrieval=536 not_required=161`; `pending_r1_r2_left=0`.
- **Pendiente:**
  1. Exportar RIS de los 536 `Included` para catálogo Paperpile de texto
     completo (tarea ya registrada en memoria).
  2. Iniciar cribado de texto completo (FT) sobre los 536 `Included`
     (`pending_retrieval` en `full_texts`).
  3. Canal C (rastreo de citas) sobre las revisiones excluidas por
     `wrong publication type` y sobre los `Included` finales, tras el
     cribado FT.

## 2026-09-13 — Cierre de sesión: cribado T/A cerrado, próximos pasos

**Estado del ciclo:** cribado título/abstract **cerrado** en SQLite para los
697 registros del corpus combinado (PubMed + Europe PMC). Fase actual:
recuperación y cribado de **texto completo (FT)**.

**Próximos pasos, en orden:**

1. **Exportar catálogo RIS de los 536 `Included`** para Paperpile (tarea
   registrada en memoria del usuario: "tras el consenso T/A, exportar RIS
   de los Included para catálogo Paperpile de texto completo"). Los
   registros a exportar son los que quedaron con
   `full_texts.retrieval_status = 'pending_retrieval'` tras el batch
   `IMP-SR-AIBIO-GU-2026-09-13-TA-SCREEN`.
2. **Recuperar el texto completo** de esos 536 registros (PDF/HTML según
   disponibilidad; preprints incluidos por regla del protocolo, con
   sustitución por la versión publicada si aparece durante FT).
3. **Cribado de texto completo doble** (R1 `REV-ALCIDES` / R2
   `REV-PAOLA`), con motivos de exclusión controlados
   (`exclusion_reasons`), igual estructura que el cribado T/A
   (planillas ciegas → conflictos → consenso documentado → import a
   SQLite).
4. **Puntos a resolver explícitamente durante el FT**, ya señalados en el
   consenso T/A y que no deben perderse:
   - Los **10 registros enviados a FT por ambigüedad de contenido/método**
     en vez de decidirse a nivel de abstract: REC-AIBIOGU-000017, -036,
     -103, -461, -502, -574, -590 (más -161/-000642 y -496/-000668, que
     además son **duplicados internos** del corpus a fusionar en FT).
   - **REC-AIBIOGU-000461** (tumor de Wilms): confirmar si encaja en la
     definición de Población del protocolo (neoplasia renal pediátrica
     embrionaria, no carcinoma de células renales) antes de extraerlo
     como incluido definitivo.
   - Verificar en FT si alguno de los `Included` por la regla "radiómica
     cuenta como IA/ML sin algoritmo de selección explícito" (criterio
     adoptado en esta sesión, ver entrada de consenso arriba) debería
     reclasificarse al leer la metodología completa — esta regla se
     aplicó a nivel de título/abstract y es más laxa que exigir un
     algoritmo ML nombrado; el FT es el lugar natural para confirmarla o
     corregirla estudio por estudio.
   - Las **20 revisiones** excluidas por `wrong publication type` pero
     marcadas como "muy relevantes" (contexto/rastreo de referencias) se
     reservan para **Canal C** (rastreo de citas hacia atrás/adelante),
     no se descartan del todo.
5. **Canal C (rastreo de citas):** ejecutar tras cerrar el cribado FT,
   sobre los estudios incluidos definitivos y sobre las revisiones del
   punto anterior, según lo ya definido en el protocolo.
6. Tras cerrar FT + appraisal (MMAT/PROBAST-TRIPOD-AI) + extracción:
   exportar paquete de handoff con
   `python3 scripts/export_handoff_sesion2.py --review reviews/AIBIO_GU`
   para que REDACTOR Sesión 2 redacte el manuscrito.

**No pendiente / ya resuelto en esta sesión:** cribado T/A R1 y R2
completos, kappa calculado (0.637, acuerdo sustancial), 105 conflictos +
15 concordantes-`Maybe` resueltos por consenso documentado (incluye
REC-AIBIOGU-000678 por COI de coautoría Chaux), importación a SQLite
verificada (`pending_r1_r2_left=0`).

## 2026-09-21 — Appraisal andamiaje + pilot

1. Fase post-FT: consenso final 400 Included / 36 Excluded ya en SQLite.
2. Creado codebook + planilla 400 + pilot estratificado n=20 en
   `risk_of_bias/`.
3. Pilot completado (4 lotes paralelos A–D → fusionados a
   `AIBIO_GU_appraisal_PILOT_20.csv`). Resumen y calibración en
   `AIBIO_GU_appraisal_PILOT_SUMMARY.md` y `decision-log.md`.
4. **Estado:** codebook congelado post-pilot. **Siguiente:** escala
   appraisal 380 restantes → extracción → Canal C → handoff Sesión 2.
5. Extracciones temporales de PDF en `risk_of_bias/pilot_txt/`
   (gitignored).

## 2026-09-22 — Appraisal: escala 380 + consolidación 400 + import SQLite

1. Escala ejecutada en 8 lotes paralelos (`risk_of_bias/batches/`,
   ~47-48 registros c/u) por agentes IA sobre PDF completo. 380/380
   filas completadas, 0 `cannot_appraise`.
2. Al fusionar piloto+lotes se detectó y corrigió una inconsistencia de
   calibración MMAT (regla escrita del codebook vs. práctica real del
   piloto) — ver `decision-log.md` para el detalle y la resolución
   (regla mecánica congelada, 46/400 filas recalificadas). También se
   normalizó `probast_overall_applicability` y se corrigió 1 fila con
   corrimiento de columna.
3. Consolidado en `risk_of_bias/AIBIO_GU_appraisal_FULL_400.csv` (400
   únicos, sin duplicados, 0 blancos en `mmat_overall`).
4. Importado a SQLite: 400 `studies` + `study_records` (1:1) + 7978
   `risk_of_bias` (MMAT_v2018/PROBAST/TRIPOD-AI) vía
   `reviews/scripts/import_aibio_gu_appraisal.py`.
5. **Estado: appraisal cerrado.** **Siguiente:** extracción de datos
   sobre los 400 Included.

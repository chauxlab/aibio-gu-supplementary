# Protocolo operativo — AIBIO_GU

**Título:** Artificial Intelligence–Driven Biomarker Discovery in Genitourinary
Oncology

**Tipo de revisión:** Integrativa (Whittemore & Knafl)
**Idioma del manuscrito:** Inglés
**Marco utilizado:** PCC (Population – Concept – Context) con desenlaces
declarados
**Fecha de búsqueda:** 2026-09-09 (Canal A PubMed + Canal B Europe PMC)
**review_id:** `SR-AIBIO-GU` · **review_slug:** `AIBIO_GU`
**Registro externo (OSF/PROSPERO):** No registrado. PROSPERO no aplica a
revisiones integrativas. Se ofrece registro OSF opcional; `protocol_ref:
pending` en `review.yaml` hasta que el usuario lo solicite.

## Origen del expediente

Expediente nuevo, sin corpus previo en REDACTOR. La búsqueda se ejecutará
desde este expediente mediante los conectores bibliográficos disponibles en
sesión (PubMed/MEDLINE, Europe PMC vía Scite/Search) más un canal adicional
para literatura ya conocida por el usuario si aplica ("identificados por
otros métodos").

## Justificación

El descubrimiento de biomarcadores en oncología genitourinaria (próstata,
urotelio/vejiga, riñón, pene, testículo) ha incorporado de forma acelerada
métodos de inteligencia artificial y aprendizaje automático: deep learning
sobre patología digital (WSI, H&E, IHC), radiómica y modelos sobre imagen
(mpMRI, TC, PET), y modelos de machine learning sobre datos genómicos,
transcriptómicos, proteómicos y de metilación, así como integración
multiómica. Falta una síntesis integrativa que mapee qué modalidades se han
aplicado, a qué sitios tumorales, con qué propósito clínico (diagnóstico,
pronóstico, predicción de respuesta), y con qué grado de validación y
preparación para traslación clínica.

## Pregunta de revisión

En pacientes con neoplasias genitourinarias, ¿qué biomarcadores se han
descubierto o priorizado mediante métodos de inteligencia artificial /
aprendizaje automático, sobre qué modalidades de datos, con qué propósito
clínico, y cuál es el estado de su validación analítica y clínica?

## PCC

- **P (Población):** Pacientes (o muestras/datos derivados de pacientes) con
  neoplasias genitourinarias: carcinoma de próstata, carcinoma urotelial /
  de vejiga, carcinoma de células renales, carcinoma escamocelular de pene,
  y tumores de células germinales testiculares. Se admite investigación
  traslacional con cohortes humanas y datos públicos de pacientes (TCGA,
  CPTAC, etc.).
- **C (Concepto):** Descubrimiento, identificación o priorización de
  biomarcadores mediante inteligencia artificial o aprendizaje automático
  —incluyendo deep learning, machine learning clásico, modelos de
  fundación, y pipelines de IA para selección de variables— con el fin
  explícito de derivar un biomarcador (molecular, histológico/morfológico,
  radiómico, o firma multiómica).
- **Contexto:** Cualquier modalidad de dato: patología digital / histología,
  radiología / imagen médica, genómica, transcriptómica, proteómica,
  epigenómica/metilación, y multiómica integrada. Cualquier entorno
  (académico, multicéntrico, retrospectivo o prospectivo).
- **Desenlaces (declarados):**
  1. Biomarcador(es) descubierto(s) y su modalidad de dato.
  2. Propósito clínico: diagnóstico, pronóstico, predicción de respuesta a
     terapia, estratificación de riesgo, otros.
  3. Método de IA/ML empleado y datos de entrada.
  4. Desempeño reportado (AUC, C-index, HR, sensibilidad/especificidad,
     según corresponda).
  5. Nivel de validación: interna (hold-out, validación cruzada), externa
     (cohorte independiente), o utilidad clínica demostrada.
  6. Disponibilidad de código/datos y adherencia a guías de reporte
     (TRIPOD-AI, CLAIM, DECIDE-AI, etc.).

## Criterios de elegibilidad

### Inclusión

- Estudios primarios (retrospectivos o prospectivos) y estudios
  traslacionales con cohortes humanas o datos de pacientes.
- Uso explícito de IA/ML como parte del proceso de descubrimiento o
  priorización del biomarcador.
- Neoplasia genitourinaria según la definición de Población.
- Reporta al menos un biomarcador candidato con alguna métrica de desempeño
  o asociación con desenlace clínico.
- Publicado en inglés (u otro idioma si hay texto completo evaluable; se
  documentará).
- Sin límite inferior de fecha; se documentará la ventana efectiva tras la
  búsqueda.

### Exclusión

- Revisiones, editoriales, comentarios, protocolos y actas de congreso sin
  datos primarios (las revisiones se conservan para lectura de contexto y
  rastreo de referencias, no como estudios incluidos).
- Modelos de IA que usan biomarcadores ya establecidos únicamente como
  predictores (sin descubrimiento/priorización de un biomarcador nuevo).
- Estudios puramente metodológicos sin aplicación a una cohorte GU.
- Modelos de imagen o patología orientados solo a detección/segmentación o
  diagnóstico asistido sin derivación de un biomarcador.
- Tumores no genitourinarios; tumores GU no epiteliales salvo tumores
  germinales testiculares (sarcomas, linfomas secundarios, metástasis a
  órgano GU → excluidos).
- Estudios exclusivamente preclínicos (líneas celulares, modelos animales)
  sin componente en tejido/datos humanos.

## Ecuaciones de búsqueda

Estructura de bloques: **(GU neoplasm) AND (AI/ML) AND (biomarker discovery /
signature)**. Por límite del conector PubMed (máx. 20 operadores booleanos
por consulta) la búsqueda se ejecutó **por sitio tumoral** y se unió por
PMID.

### Canal A — MEDLINE (PubMed), ejecutado 2026-09-09

**Próstata / Vejiga-urotelio / Riñón** (mismo patrón de 3 bloques):

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

- Próstata: `("Prostatic Neoplasms"[MeSH] OR "prostate cancer"[tiab])` → **202**
- Vejiga: `("Urinary Bladder Neoplasms"[MeSH] OR "bladder cancer"[tiab] OR
  "urothelial carcinoma"[tiab])` → **171**
- Riñón: `("Carcinoma, Renal Cell"[MeSH] OR "Kidney Neoplasms"[MeSH] OR
  "renal cell carcinoma"[tiab] OR "kidney cancer"[tiab])` → **199**

**Pene / Testículo** (sin bloque de biomarcador, por baja densidad de
literatura; el bloque de IA se amplió con `"digital pathology"` y
`"neural network"`):

- Pene: `("Penile Neoplasms"[MeSH] OR "penile cancer"[tiab] OR
  "penile carcinoma"[tiab] OR "penile squamous cell carcinoma"[tiab]) AND
  (IA ampliada)` → **14**
- Testículo: `("Testicular Neoplasms"[MeSH] OR "testicular cancer"[tiab] OR
  "testicular germ cell tumor"[tiab] OR "testicular germ cell tumour"[tiab])
  AND (IA ampliada)` → **57**

Corpus RIS: `searches/ris/AIBIO_GU_pubmed_2026-09-09.ris` (metadatos vía
conector PubMed/NCBI; abstract presente en 628/632).

### Canal B — Europe PMC, ejecutado 2026-09-09

Ejecutado contra la **API REST pública de Europe PMC**
(`https://www.ebi.ac.uk/europepmc/webservices/rest/search`, sin
autenticación; no se dispone de conector MCP de Europe PMC en la sesión de
REVISOR — en revisiones previas la búsqueda Europe PMC se corría en el
proyecto REDACTOR). Sintaxis `TITLE:` / `ABSTRACT:`, misma estructura de
bloques que el Canal A, restringido a `SRC:MED OR SRC:PMC OR SRC:PPR`
(incluye preprints y registros solo-PMC no indexados en MEDLINE).

- Próstata → 189 · Vejiga → 152 · Riñón → 169 (bloque de biomarcador
  exigido)
- Pene → 14 · Testículo → 54 (bloque de IA ampliado, sin bloque de
  biomarcador)
- **576 únicos** tras dedup interno; 511 ya presentes en el Canal A
  (solapamiento 89 %); **65 registros nuevos** incorporados (mayoría
  preprints de Research Square / bioRxiv / medRxiv / Preprints.org / SSRN;
  ~12 registros MEDLINE que las ecuaciones por sitio del Canal A no
  capturaron).

Corpus RIS combinado: `searches/ris/AIBIO_GU_pubmed_europepmc_2026-09-09.ris`
(el RIS solo-PubMed queda en `searches/ris/superseded/`). Campo `N1` de cada
registro marca `source_channel: A` o `B`.

**Scite (descartado como canal):** se probó `search_literature` de Scite como
sustituto de Europe PMC; su búsqueda es sobre texto completo (no
título/abstract), devuelve miles de hits por sitio con baja precisión
(mayoría revisiones/panorámicas) y el muestreo no es exhaustivo ni
reproducible como consulta de base de datos. No se incorpora al corpus; Scite
queda reservado para rastreo dirigido de vacíos durante la síntesis.

### Preprints

Los preprints se retienen para cribado título/abstract. Regla de
sustitución: si durante el cribado o la extracción se identifica una versión
publicada del mismo trabajo, se sustituye el preprint por el registro
publicado y se documenta en `decision-log.md`.

### Canal C — "identificados por otros métodos" (rastreo de citas)

Decisión 2026-09-09: la estrategia de bases de datos se **cierra en dos
canales** (PubMed + Europe PMC), a la par del estándar de las revisiones
previas del repositorio (HPV_PSCC, ctDNA_GU, GLP1). No se ejecutan
OpenAlex / Semantic Scholar / SciELO / arXiv: rendimiento marginal esperado
bajo (la elegibilidad exige cohorte humana GU + derivación de biomarcador →
literatura clínica/traslacional indexada en MEDLINE/PMC) y solapamiento
Canal A/B del 89 % que sugiere saturación. Limitación documentada.

Canal C = **rastreo de citas** tras el cribado de texto completo:
referencias hacia atrás de los estudios incluidos y de las revisiones
sistemáticas/narrativas recuperadas, más citas hacia adelante de los
incluidos, más literatura que aporte el usuario (RIS/DOI). Los hallazgos se
declaran aparte en la contabilidad PRISMA como "otros métodos".

Ejecución 2026-09-23 (Europe PMC): citas hacia atrás de las 20 revisiones
reservadas en el cribado de título/abstract, y citas hacia adelante de los
400 incluidos de la búsqueda en bases. No hubo RIS ni DOI aportado por el
usuario. Las citas hacia atrás de esos 400 estudios primarios quedan fuera
de esta pasada.

### Bases sin conector ni API abierta

Embase, Web of Science, Scopus, IEEE Xplore, Cochrane CENTRAL: requieren
acceso institucional del que no se dispone. Su omisión es aceptable para una
revisión integrativa; se documenta como limitación. Si se obtiene acceso, se
incorporarán como fuente adicional declarada.

## Contabilidad PRISMA (n reales — 2026-09-09)

| Etapa | n |
|---|---|
| Canal A PubMed — próstata / vejiga / riñón / pene / testículo | 202 / 171 / 199 / 14 / 57 |
| Canal A — subtotal bruto | 643 |
| Canal A — únicos tras dedup por PMID | 632 |
| Canal B Europe PMC — próstata / vejiga / riñón / pene / testículo | 189 / 152 / 169 / 14 / 54 |
| Canal B — únicos tras dedup interno | 576 |
| Canal B — solapan con Canal A (dedup cruzado por PMID/DOI) | 511 |
| Canal B — nuevos incorporados | 65 |
| **Corpus combinado ingresado a SQLite (búsqueda en bases cerrada)** | **697** |

Registros de la búsqueda en bases: 697 (`REC-AIBIOGU-000001`…`000697`; 693 con
abstract), verificado en intake (ver `logs/workflow-log.md`).

### Búsqueda en bases — texto completo (2026-09-21)

Los 536 incluidos de título y abstract pasaron a recuperación
(`pending_retrieval` el 2026-09-13). El 2026-09-21 el estado ya no es
pendiente.

| Etapa | n |
|---|---|
| Texto buscado | 536 |
| Texto no recuperado (`retrieval_status = unavailable`; decisión aún Pending, no se evaluaron) | 100 |
| Texto recuperado y evaluado | 436 |
| Incluidos | 400 |
| Excluidos | 36 |
| — intervención | 19 |
| — tipo de publicación | 15 |
| — desenlace | 1 |
| — población | 1 |

De los 100 no recuperados: 94 sin suscripción institucional y sin copia en
acceso abierto; 1 embargo de PMC hasta 2026-12; 1 repositorio solo con
metadatos; 1 paywall de ScienceDirect; 1 paywall de Wiley; 1 descarga
bloqueada en preprints.org; 1 handle roto en Helda. El motivo está en
`full_texts.note` de cada registro.

### Canal C — otros métodos (2026-09-23)

Fuente: Europe PMC. Enlaces crudos 4953; registros únicos 4185.

| Etapa | n |
|---|---|
| Ya presentes en los 697 | 215 |
| Sin título | 1 |
| Registros nuevos | 3969 |
| Excluidos por filtro de título (sitio GU y método de IA/radiómica/biomarcador) | 3279 |
| Cribados en título y abstract | 690 |
| Excluidos en título y abstract | 348 |
| — tipo de publicación | 199 |
| — desenlace | 71 |
| — intervención | 70 |
| — población | 8 |
| Texto no recuperado; elegibilidad sin resolver | 5 |
| Incluidos en título y abstract, texto buscado | 337 |
| Texto no recuperado entre esos incluidos | 230 |
| Texto recuperado e incluido como estudio | 107 |
| **Estudios incluidos por otros métodos** | **107** |
| Estudios incluidos por búsqueda en bases (FT 2026-09-21) | 400 |
| **Estudios incluidos en la revisión** | **507** |

Los 5 sin resolver son: *J Urol* 2000 (10.1016/s0022-5347(05)67948-7),
*IEEE TBME* 2015 (10.1109/tbme.2015.2485779), arquitectura nuclear prostática
2017 (sin PMID ni DOI), *Radiology: AI* 2025 (10.1148/ryai.230555) y
*The Prostate* 2026 (10.1002/pros.70088). Los 230 restantes quedan como
incluidos de título y abstract sin PDF en acceso abierto.

## Deduplicación e intake

- Deduplicación cruzada por DOI/PMID antes del conteo final.
- Intake a SQLite con `python3 scripts/review_intake_ris.py --review
  reviews/AIBIO_GU` (prefijo `REC-AIBIOGU-000001`…).
- `import_batch_id` según lo genera el script, sin edición manual.

## Cribado

- Título/abstract doble e independiente: R1 = `REV-ALCIDES`, R2 =
  `REV-PAOLA`.
- Consenso de conflictos documentado; kappa reportado.
- Texto completo: doble, con motivos de exclusión controlados
  (`exclusion_reasons`).
- Catálogo RIS del corpus `Included` para Paperpile al pasar de consenso a
  recuperación de texto completo (ver `manual/full-text-catalog-ris.md`).

## Evaluación crítica (appraisal)

- **MMAT v.2018** (Mixed Methods Appraisal Tool) como instrumento primario,
  por la heterogeneidad de diseños esperada (igual que HPV_PSCC).
- Complemento específico para estudios de modelos predictivos: dominios
  **PROBAST** / **TRIPOD-AI** como appraisal dirigido de riesgo de sesgo y
  aplicabilidad de los modelos de IA. Se registrará por estudio.

## Extracción

Formulario ancho en `extraction/`. Campos mínimos: sitio tumoral, diseño,
n, fuente de datos (cohorte propia / TCGA / CPTAC / otra), modalidad de
dato, tarea de IA, arquitectura/algoritmo, features de entrada, biomarcador
resultante, propósito clínico, métrica y valor de desempeño, validación
(interna/externa/utilidad), disponibilidad de código y datos, guía de
reporte declarada, financiamiento y conflictos.

## Síntesis

Síntesis integrativa narrativa (Whittemore & Knafl): reducción, despliegue,
comparación y conclusiones, estratificada por **sitio tumoral** y por
**modalidad de dato**. Sin metaanálisis previsto (heterogeneidad de
desenlaces y métricas). Tablas de mapeo modalidad × propósito × sitio.

## Handoff

Al cerrar corpus FT + appraisal + extracción, exportar paquete con
`python3 scripts/export_handoff_sesion2.py --review reviews/AIBIO_GU`
(ver `manual/handoff-sesion2.md`). REDACTOR Sesión 2 redacta el manuscrito
en inglés.

## Estado

Protocolo aprobado (2026-09-09). Búsqueda en bases cerrada (697). Cribado
T/A cerrado (536 Included). FT de esa cohorte cerrado 2026-09-21
(**100 no recuperados / 400 Included / 36 Excluded**). Appraisal y
extracción de esos 400 cerrados. Canal C cerrado 2026-09-23: **107** estudios añadidos con texto,
**230** incluidos de título y abstract sin PDF, **5** elegibilidades sin
texto. Corpus para síntesis: **507** estudios. Síntesis de mapeo en
`analysis/AIBIO_GU_synthesis_summary_2026-09-23.md`.

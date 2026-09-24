# Síntesis de mapeo — AIBIO_GU

Fecha: 2026-09-23. Corpus: 507 estudios con texto (400 de la búsqueda en bases y 107 de Canal C). Extracción: 338 `complete`, 169 `needs_review`. Sin metaanálisis.

## Reducción

El corpus se concentra en tres sitios y dos modalidades. Riñón 181, próstata 174 y urotelio/vejiga 133 suman 488 de 507. Testículo aporta 13, pene 1 (`REC-AIBIOGU-000677`), un estudio abarca los tres subtipos de carcinoma renal (`REC-AIBIOGU-000544`) y 4 son pan-cáncer con un componente genitourinario.

La modalidad dominante es imagen radiológica (217) y transcriptómica (164). Siguen multiómica (66), patología digital (23), otras (19), proteómica (8), metilación (6) y genómica (4).

El propósito clínico es pronóstico en 258, diagnóstico en 187, estratificación de riesgo en 21, predicción de respuesta en 19, subtipado en 17 y otro en 5.

## Despliegue: sitio × modalidad

| Sitio | Radiología | Transcriptómica | Multiómica | Patología digital | Otra | Proteómica | Metilación | Genómica | Total |
|---|---:|---:|---:|---:|---:|---:|---:|---:|---:|
| Riñón | 75 | 62 | 29 | 5 | 5 | 1 | 1 | 3 | 181 |
| Próstata | 86 | 48 | 17 | 8 | 6 | 6 | 2 | 1 | 174 |
| Urotelio/vejiga | 47 | 52 | 16 | 9 | 5 | 1 | 3 | 0 | 133 |
| Testículo | 9 | 0 | 2 | 1 | 1 | 0 | 0 | 0 | 13 |
| Otro / pan-cáncer | 0 | 1 | 1 | 0 | 2 | 0 | 0 | 0 | 4 |
| Pene | 0 | 0 | 1 | 0 | 0 | 0 | 0 | 0 | 1 |
| Múltiple (subtipos renales) | 0 | 1 | 0 | 0 | 0 | 0 | 0 | 0 | 1 |
| Total | 217 | 164 | 66 | 23 | 19 | 8 | 6 | 4 | 507 |

## Despliegue: sitio × propósito

| Sitio | Pronóstico | Diagnóstico | Riesgo | Respuesta | Subtipado | Otro | Total |
|---|---:|---:|---:|---:|---:|---:|---:|
| Riñón | 106 | 57 | 5 | 4 | 6 | 3 | 181 |
| Próstata | 78 | 74 | 12 | 5 | 4 | 1 | 174 |
| Urotelio/vejiga | 71 | 43 | 4 | 9 | 5 | 1 | 133 |
| Testículo | 1 | 11 | 0 | 0 | 1 | 0 | 13 |
| Otro / pan-cáncer | 2 | 1 | 0 | 1 | 0 | 0 | 4 |
| Pene | 0 | 1 | 0 | 0 | 0 | 0 | 1 |
| Múltiple (subtipos renales) | 0 | 0 | 0 | 0 | 1 | 0 | 1 |
| Total | 258 | 187 | 21 | 19 | 17 | 5 | 507 |

## Comparación

La imagen pesa más en próstata (86/174) y riñón (75/181). En urotelio la transcriptómica (52) supera a la radiología (47). El testículo es casi solo diagnóstico por imagen (9 de 13 radiológicos; 11 de 13 diagnósticos). La predicción de respuesta es el propósito más delgado en los tres sitios grandes (9 vejiga, 5 próstata, 4 riñón).

Validación del número que sostiene el desempeño: cohorte externa 206, multicéntrica externa 52, hold-out interno 142, validación cruzada 61, un solo split 38, ninguna 7, utilidad clínica demostrada 1 (`REC-AIBIOGU-000451`). Hay alguna validación externa en 258 de 507.

Código disponible en 21, a pedido en 4, no disponible en 3, no reportado en 479. Datos disponibles en 77, a pedido en 64, no disponibles en 4, no reportados en 362. Guía de reporte declarada en 15 (TRIPOD 7, TRIPOD-AI 1, STARD 1, CLAIM 1, otra 5); 492 no declaran ninguna.

MMAT global (n=507): bajo 317, moderado 177, alto 13. PROBAST riesgo global: alto 463, unclear 42. Dos estudios no tienen bloque PROBAST ni TRIPOD-AI (`REC-AIBIOGU-000079`, `REC-AIBIOGU-000502`). TRIPOD-AI en los 505 restantes: adecuado 236, parcial 263, inadecuado 6.

## Conclusión

Lo que la literatura ha producido es un mapa de firmas pronósticas y diagnósticas, sobre todo de imagen y transcriptómica, en riñón, próstata y urotelio. La preparación para traslación es estrecha: una sola utilidad clínica demostrada, el código casi nunca está disponible y el riesgo PROBAST es alto en 463 de los 505 juicios. Pene y testículo siguen siendo huecos de evidencia. Quedan fuera de este mapa 230 incluidos de Canal C sin PDF y 5 registros cuya elegibilidad no se pudo cerrar sin el texto.

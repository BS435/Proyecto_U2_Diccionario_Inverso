# Diccionario de datos

## `nap_normas.csv` — Normas de asociación de palabras para el español de México (NAP)

Una fila por par **estímulo → respuesta**, derivada de `NAP.xls` (Arias-Trejo et al.; ver README para las condiciones de uso de NAP).

| Columna | Tipo | Descripción |
|---|---|---|
| `estimulo` | texto | Palabra estímulo (lema, minúsculas). 232 estímulos. |
| `respuesta` | texto | Respuesta lematizada. Las expresiones de varias palabras se unen con guion bajo (`cepillo_de_dientes`). |
| `frecuencia` | entero | Número de veces que los participantes dieron esa respuesta al estímulo. Si dos respuestas distintas comparten lema (*calentar*, *calentarse*), se suman. |
| `tiempo_medio` | segundos | Tiempo medio de respuesta, **promedio ponderado por frecuencia** cuando se fusionaron varias respuestas. |
| `fuerza` | % | Fuerza de asociación: `100 · frecuencia / total de respuestas del estímulo` (en NAP, `% ASOCIACIÓN`). Se suma al fusionar respuestas. |

**Limpieza aplicada** (cifras exactas en la sección 1.1 del cuaderno): se descartan respuestas inválidas (`*`, lema `=`), números y marcas de tiempo, y
autoasociaciones (estímulo = respuesta). El último estímulo (`zapato`) no trae lema en NAP y se recupera de la forma original.

## `evaluacion.csv` — Conjunto de evaluación

Una fila por descripción. Esquema pedido en el proyecto: `concepto, definicion, fuente, particion`.

| Columna | Tipo | Descripción |
|---|---|---|
| `concepto` | texto | Palabra objetivo, como aparece en el corpus de clase (`Abeja`). Debe ser nodo de NAP (excepciones y alias en el cuaderno: `Aspiradora`→`aspirador`, `Papas`→`papa`). |
| `definicion` | texto | Descripción libre escrita por una persona **sin nombrar la palabra**. |
| `fuente` | categoría | `equipo` (recolectada por el equipo, conjunto principal) · `llm` (generada por un modelo de lenguaje; solo complementaria y declarada) · `sintetico_prueba` (para depurar código; **nunca** se reporta). |
| `particion` | categoría | La asigna el cuaderno **por concepto** con semilla 42: `desarrollo` (40 % de los conceptos) · `prueba` (60 %) · `excluida` (sin nodo en el grafo o contiene la palabra objetivo). Dejar vacía al entregar descripciones nuevas. |

## `conceptos.csv` — Conceptos evaluados (generado por el cuaderno)

| Columna | Descripción |
|---|---|
| `concepto`, `nodo` | Palabra objetivo y nodo del grafo con que se compara |
| `descripciones` | Número de descripciones del concepto |
| `en_grafo` | `sí` si el concepto es nodo de NAP |
| `particion` | `desarrollo`, `prueba` o `excluida` |

## `evaluacion_PRUEBA_sintetico.csv`

Descripciones **generadas por una IA** para depurar el pipeline mientras se recolectaban las reales. No provienen de personas, **no son datos válidos para los resultados**
y el cuaderno muestra una advertencia siempre que se usan. Se eliminan del flujo en cuanto existe `evaluacion.csv`.

## Carpeta `resultados/`

| Archivo | Contenido |
|---|---|
| `resultados.json` | Todas las cifras del cuaderno (alimenta el reporte) |
| `tabla_comparativa.csv` | Tabla única de modelos en el conjunto de prueba (p@1, p@3, p@5, MRR, cobertura, IC 95 %, tiempo) |
| `analisis_errores.csv`, `analisis_errores_15_casos.csv` | Errores del mejor modelo con causa sugerida y columna `causa_revisada` para el equipo |
| `fig_*.png` | Figuras del cuaderno |
| `embeddings/` | Embeddings node2vec entrenados (reproducibles con la semilla; se pueden borrar) |
| `cache_rankings_*.pkl` | Caché de rankings (se puede borrar; se regenera) |

## Fórmulas

* **Distancia** (para caminos más cortos): frecuencia `1/f` · tiempo `t` · asociación `100 − FA`. **Similitud** (PageRank, vector propio, node2vec): `f` · `1/t` · `FA`.
* **p@k** = fracción de descripciones con la palabra objetivo en los `k` primeros candidatos. **MRR** = media de `1/posición` (0 si no aparece en los 100 candidatos).
  **Cobertura** = fracción de descripciones para las que el modelo devuelve al menos un candidato.
* **RRF** = `Σ 1 / (k + rango)` sobre las listas fusionadas.

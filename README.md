# Diccionario inverso con normas de asociación (NAP) y grafos

Proyecto de la Unidad 2 · Procesamiento de Lenguaje Natural · UADY, Facultad de Matemáticas · **Modalidad C** (extensión metodológica sobre NAP).

Un diccionario inverso recibe una **descripción en lenguaje natural** y devuelve palabras candidatas ordenadas. Aquí el conocimiento léxico son las
normas de asociación de palabras del español de México (NAP) representadas como grafo.

**Qué se compara**

| Familia | Modelos |
|---|---|
| Centralidad en grafos (clase) | Intermediación por subconjunto · intermediación · cercanía · grado · vector propio · Katz · comunicabilidad, con 3 ponderaciones (frecuencia, tiempo, asociación) |
| Híbrido | node2vec con ≥ 3 configuraciones; suma vectorial de los lemas y similitud coseno |
| **Extensión 1** | PageRank personalizado: grafo no dirigido frente a dirigido (estímulo → respuesta) |
| **Extensión 2** | Fusión de rankings RRF entre el mejor modelo de grafos, node2vec y PageRank |

Métricas: p@1, p@3, p@5, MRR y cobertura, con conjunto de **desarrollo** (ajuste) y **prueba** (cifras finales) separados por concepto.

## Contenido

```
diccionario_inverso_nap.ipynb   Cuaderno principal (ejecutable de principio a fin)
Reporte_Diccionario_Inverso_BORRADOR.docx   Reporte técnico (borrador con datos de prueba; se regenera con el script)
datos/
  NAP.xls                       Normas originales de clase
  nap_normas.csv                estimulo, respuesta, frecuencia, tiempo_medio, fuerza
  evaluacion.csv                concepto, definicion, fuente, particion   ← descripciones reales
  evaluacion_PRUEBA_sintetico.csv  descripciones generadas por IA, SOLO para depurar
  conceptos.csv                 conceptos evaluados y su partición
  diccionario_de_datos.md       descripción de columnas y fórmulas
recoleccion/                    instrucciones del formulario, consentimiento y plantilla
resultados/                     resultados.json, tabla_comparativa.csv, figuras, análisis de errores
herramientas/                   Scripts que regeneran el cuaderno y el reporte (ver herramientas/README.md)
requirements.txt
```

## Cómo reproducir

**Local (Python 3.12, Windows/macOS/Linux)**

```bash
python -m venv .venv
.venv\Scripts\activate            # en macOS/Linux: source .venv/bin/activate
pip install -r requirements.txt
jupyter lab diccionario_inverso_nap.ipynb      # Run all
```

O sin interfaz: `jupyter nbconvert --to notebook --execute --inplace diccionario_inverso_nap.ipynb`.

**Google Colab.** Subir la carpeta a `MyDrive/PLN 26/Diccionario inverso/` (con `datos/`), abrir el cuaderno y ejecutar todo. Si Colab pide reiniciar la sesión tras instalar
dependencias, reiniciar y ejecutar de nuevo.

**Reproducibilidad.** `SEMILLA = 42` (partición, node2vec con `workers = 1` y hash estable, bootstrap). Las versiones fijas están en `requirements.txt`; node2vec 0.5.0 requiere
`numpy<2` y `gensim<5`. Los rankings se guardan en `resultados/cache_rankings_*.pkl` (se pueden borrar; tardan unos minutos en regenerarse).

**Cómo reproducir cada tabla**

| Tabla / figura | Sección del cuaderno |
|---|---|
| Estadísticas del grafo y distribución de grado | 2 |
| Cobertura del preprocesamiento | 4.2 |
| Centralidades (desarrollo y prueba) | 5.4 |
| node2vec: hiperparámetros y prueba | 6.4–6.5 |
| PageRank personalizado y RRF | 7 |
| **Tabla comparativa única** y comparación con la Tabla 1 | 8.1–8.2 |
| Análisis de errores (15 casos) | 9 |

## Datos de evaluación: descripciones reales y de prueba

* El conjunto **válido para reportar** es `datos/evaluacion.csv` (descripciones escritas por personas participantes, `fuente = equipo`).
  Las instrucciones de recolección están en `recoleccion/`.
* Mientras no exista, el cuaderno usa `datos/evaluacion_PRUEBA_sintetico.csv` (descripciones **generadas por una IA** para depurar el código) y muestra
  un aviso naranja en cada resultado. **Esas cifras no son resultados del proyecto.**
* Las descripciones generadas por modelos de lenguaje solo se aceptan como conjunto complementario identificado (`fuente = llm`); nunca como único conjunto de evaluación.

## Licencias y créditos

* **NAP** (Arias-Trejo et al.) se usa con fines académicos; el archivo original `NAP.xls` lo proporcionó el curso y **no se redistribuye** con licencia abierta.
  Consultar las condiciones de uso de NAP antes de publicarlo.
* El conjunto de evaluación recolectado y su documentación se pueden publicar con licencia **CC BY 4.0** (anónimo, sin datos personales) — confirmarlo con el equipo antes de subirlo.
* El enfoque base y los resultados de referencia son de Reyes-Magaña et al. (2019). Referencias completas en la sección 12 del cuaderno.
* **Uso de IA generativa:** como apoyo para programar y redactar; no para simular participantes ni fabricar normas o respuestas.

## Equipo

*[Nombres de los integrantes, roles y responsable de la presentación]*

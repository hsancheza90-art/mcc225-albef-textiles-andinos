# MCC225 - ALBEF aplicado a textiles andinos

Repositorio desarrollado para el examen parcial virtual del curso **MCC225A - IA Generativa y Aprendizaje Multimodal**.

El trabajo toma como referencia el modelo **ALBEF: Align Before Fuse** y desarrolla una línea base reproducible de recuperación imagen-texto aplicada a un conjunto controlado de imágenes de prueba inspiradas en textiles andinos.

---

## 1. Tema del trabajo

**Modelo asignado:** ALBEF: Align Before Fuse
**Cuadernos relacionados:** C5, C6 y C10
**Dominio aplicado:** análisis computacional de textiles andinos

ALBEF propone una idea central: antes de fusionar profundamente imagen y texto, conviene alinear ambas modalidades en un espacio semántico compartido. Luego, una etapa de fusión multimodal puede analizar relaciones más finas entre regiones visuales y lenguaje.

---

## 2. Enfoque del experimento

Este repositorio **no implementa ALBEF completo desde cero**.

El experimento implementa una línea base contrastiva usando **OpenCLIP**, con el objetivo de reproducir la primera etapa conceptual de ALBEF: el alineamiento imagen-texto.

En términos del paper:

* Se reproduce de manera aproximada la lógica de **ITC — Image-Text Contrastive Learning**.
* No se implementa **MLM — Masked Language Modeling**.
* No se implementa **ITM — Image-Text Matching** completo.
* La fusión profunda y el re-ranking crossmodal se plantean como mejora futura.

La finalidad del notebook es mostrar un flujo reproducible:

1. Generación/carga de imágenes de prueba.
2. Carga de textos candidatos.
3. Extracción de embeddings con OpenCLIP.
4. Cálculo de similitud coseno.
5. Retrieval imagen → texto.
6. Retrieval texto → imagen.
7. Cálculo de Recall@K.
8. Análisis de errores y hard negatives.
9. Relación técnica con ALBEF, C5, C6 y C10.

---

## 3. Resultado principal

El resultado principal es una evaluación de recuperación multimodal sobre un conjunto pequeño y controlado:

* **10 imágenes de prueba** inspiradas en patrones textiles andinos.
* **15 textos candidatos**:

  * 10 textos correctos.
  * 5 textos distractores.
* Modelo base: **OpenCLIP ViT-B/32**.
* Checkpoint: **laion2b_s34b_b79k**.
* Métrica principal: **Recall@1, Recall@3 y Recall@5**.

Resultados obtenidos:

| Dirección      | Recall@1 | Recall@3 | Recall@5 |
| -------------- | -------: | -------: | -------: |
| Imagen → texto |     0.20 |     0.50 |     0.70 |
| Texto → imagen |     0.30 |     0.80 |     0.80 |

La lectura principal es que OpenCLIP recupera señal semántica útil, especialmente en Top-5, pero no siempre ubica el par correcto en la primera posición. Esto justifica una mejora futura mediante re-ranking o fusión crossmodal inspirada en ALBEF.

---

## 4. Estructura del repositorio

```text
mcc225-albef-textiles-andinos/
│
├── notebooks/
│   └── 01_albef_openclip_textiles_retrieval.ipynb
│
├── data/
│   ├── images/
│   └── texts/
│       └── candidate_texts.csv
│
├── results/
│   ├── figures/
│   ├── similarity_matrix.csv
│   ├── image_to_text_rankings_top5.csv
│   ├── text_to_image_rankings_top5.csv
│   ├── recall_metrics.csv
│   ├── recall_metrics_pivot.csv
│   ├── error_analysis_image_to_text.csv
│   ├── error_analysis_text_to_image.csv
│   └── experiment_summary.csv
│
├── docs/
│
├── evaluacion/
│   ├── trazabilidad.md
│   ├── defensa_tecnica.md
│   └── analisis_critico.md
│
├── src/
├── requirements.txt
├── .gitignore
└── README.md
```

---

## 5. Instalación del entorno

Se recomienda crear un entorno virtual propio para el proyecto.

En Windows PowerShell:

```powershell
python -m venv .venv
.\.venv\Scripts\activate
python -m pip install --upgrade pip
pip install -r requirements.txt
```

Luego registrar el kernel para usarlo en VS Code o Jupyter:

```powershell
python -m ipykernel install --user --name mcc225-albef-textiles --display-name "Python (mcc225-albef-textiles)"
```

---

## 6. Ejecución del notebook

Abrir el notebook principal:

```text
notebooks/01_albef_openclip_textiles_retrieval.ipynb
```

Seleccionar el kernel:

```text
Python (mcc225-albef-textiles)
```

Luego ejecutar:

```text
Restart Kernel + Run All
```

El notebook utiliza rutas relativas al repositorio, por lo que no depende de Google Drive, Colab ni rutas absolutas de una computadora específica.

---

## 7. Archivos de entrada

El archivo principal de textos candidatos se encuentra en:

```text
data/texts/candidate_texts.csv
```

Este archivo contiene:

* identificador del texto;
* descripción textual;
* tipo de texto: correcto o distractor;
* imagen correcta asociada, cuando corresponde.

Las imágenes de prueba se generan o verifican dentro del notebook y se guardan en:

```text
data/images/
```

con la nomenclatura:

```text
T01.png, T02.png, ..., T10.png
```

---

## 8. Archivos de salida

El notebook exporta resultados a la carpeta:

```text
results/
```

Entre los principales archivos generados están:

| Archivo                            | Contenido                           |
| ---------------------------------- | ----------------------------------- |
| `similarity_matrix.csv`            | Matriz de similitud imagen-texto    |
| `image_to_text_rankings_top5.csv`  | Ranking Top-5 de textos por imagen  |
| `text_to_image_rankings_top5.csv`  | Ranking Top-5 de imágenes por texto |
| `recall_metrics.csv`               | Métricas Recall@K en formato largo  |
| `recall_metrics_pivot.csv`         | Tabla compacta de métricas          |
| `error_analysis_image_to_text.csv` | Análisis de errores imagen → texto  |
| `error_analysis_text_to_image.csv` | Análisis de errores texto → imagen  |
| `experiment_summary.csv`           | Resumen final del experimento       |

Las figuras se guardan en:

```text
results/figures/
```

---

## 9. Relación con los cuadernos del curso

| Cuaderno | Relación con el trabajo                                                                                   |
| -------- | --------------------------------------------------------------------------------------------------------- |
| C5       | Aporta el marco conceptual de fusión profunda, transformers y atención crossmodal.                        |
| C6       | Aporta la base conceptual de aprendizaje contrastivo, embeddings compartidos, retrieval y hard negatives. |
| C10      | Aporta la base práctica de OpenCLIP, zero-shot, prompts y recuperación imagen-texto.                      |

El notebook implementa principalmente conceptos vinculados a C6 y C10. C5 se utiliza para explicar la mejora futura hacia fusión profunda y re-ranking, coherente con la arquitectura de ALBEF.

---

## 10. Limitaciones

El experimento tiene alcance controlado y no debe interpretarse como una validación final sobre textiles andinos reales.

Principales limitaciones:

* Se usan imágenes sintéticas de prueba, no una base patrimonial curada.
* No se implementa ALBEF completo.
* No se entrena ningún modelo desde cero.
* No se implementa MLM ni ITM completo.
* La similitud por embeddings no equivale a interpretación cultural.
* Recall@K evalúa recuperación, pero no validez técnica, histórica o cultural.

---

## 11. Mejora propuesta

La mejora natural del experimento es agregar una segunda etapa de **re-ranking crossmodal**.

Flujo propuesto:

1. Usar OpenCLIP para recuperar candidatos Top-K.
2. Aplicar un modelo con interacción imagen-texto más profunda sobre esos candidatos.
3. Reordenar los resultados.
4. Comparar Recall@K antes y después del re-ranking.

Esta mejora se relaciona con la idea central de ALBEF:

```text
alinear primero → fusionar después
```

---

## 12. Conclusión

El repositorio demuestra una línea base reproducible de retrieval multimodal usando OpenCLIP.

Los resultados muestran que el alineamiento contrastivo permite recuperar candidatos razonables, aunque no siempre en la primera posición. Esta limitación justifica la lógica de ALBEF: primero construir correspondencias imagen-texto y luego aplicar fusión profunda para revisar relaciones más finas.

En el contexto de textiles andinos, esta ruta es metodológicamente prudente porque evita saltar directamente a una interpretación cultural automática y permite construir primero evidencia técnica trazable.

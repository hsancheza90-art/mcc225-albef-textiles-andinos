# Trazabilidad técnica del experimento

## Tema asignado

El tema asignado es **ALBEF: Align Before Fuse**, aplicado a un experimento piloto de recuperación multimodal en el dominio de textiles andinos.

## Notebook ejecutado

El notebook principal del experimento es:

`01_albef_openclip_textiles_retrieval.ipynb`

Este notebook contiene el flujo completo del experimento: carga de imágenes, carga de textos, uso de OpenCLIP, generación de embeddings, cálculo de similitud, recuperación texto-imagen, recuperación imagen-texto, métricas simples y análisis de errores.

## Resultado concreto reproducible

El resultado concreto que se puede reproducir o verificar en vivo es un pipeline de **retrieval multimodal imagen-texto**.

El experimento permite mostrar:

* imágenes textiles cargadas desde el repositorio;
* descripciones textuales asociadas;
* embeddings visuales y textuales generados por OpenCLIP;
* matriz de similitud imagen-texto;
* rankings top-k;
* recuperación texto-imagen;
* recuperación imagen-texto;
* métrica Recall@K;
* identificación de errores y posibles hard negatives.

## Cuadernos del curso utilizados

El experimento se relaciona principalmente con los siguientes cuadernos del curso:

### C5 — Deep Fusion multimodal

Se utiliza como referencia conceptual para explicar la diferencia entre alineamiento, late fusion y deep fusion. El notebook no implementa un transformer cross-modal completo, pero identifica dónde entraría una etapa posterior de fusión profunda tipo ALBEF.

### C6 — Aprendizaje contrastivo y retrieval multimodal

Es el cuaderno más directamente relacionado con la implementación. El experimento usa embeddings de imagen y texto, normalización, similitud coseno, ranking, recuperación multimodal y análisis de hard negatives.

### C10 — Zero-shot y recuperación imagen-texto

Se relaciona con el uso de OpenCLIP como modelo zero-shot. El experimento permite cambiar prompts, comparar consultas textuales, mostrar top-k y evaluar recuperación texto-imagen e imagen-texto.

## Código adaptado

Respecto a los cuadernos originales, se adaptaron principalmente los siguientes componentes:

1. Carga de un conjunto propio de imágenes textiles.
2. Definición de descripciones textuales asociadas al dominio andino.
3. Uso de OpenCLIP como baseline contrastivo.
4. Generación de embeddings visuales y textuales.
5. Cálculo de matriz de similitud.
6. Funciones de recuperación texto-imagen e imagen-texto.
7. Visualización de rankings top-k.
8. Cálculo de Recall@K.
9. Separación de resultados correctos e incorrectos.
10. Identificación de hard negatives.

## Celdas o bloques clave

Los bloques principales del notebook son:

1. Configuración del entorno e instalación de dependencias.
2. Importación de librerías.
3. Carga de imágenes y descripciones.
4. Verificación visual del dataset.
5. Carga del modelo OpenCLIP.
6. Codificación de imágenes.
7. Codificación de textos.
8. Normalización de embeddings.
9. Cálculo de matriz de similitud.
10. Recuperación texto-imagen.
11. Recuperación imagen-texto.
12. Cálculo de Recall@K.
13. Análisis de errores.
14. Discusión final y relación con ALBEF.

## Evidencia usada

La evidencia usada para sustentar el experimento incluye:

* matriz de similitud imagen-texto;
* rankings top-5;
* tablas de recuperación;
* visualización de imágenes recuperadas;
* valores de similitud coseno;
* Recall@K;
* ejemplos de errores;
* análisis de hard negatives.

## Relación con ALBEF

La relación principal con ALBEF está en la idea de **alinear antes de fusionar**.

En el notebook, el alineamiento se observa cuando las imágenes y los textos se transforman en embeddings comparables dentro de un espacio compartido. Luego se calcula similitud coseno para recuperar los pares más cercanos.

La fusión profunda no se implementa completamente en este experimento. Se plantea como una mejora futura: usar el ranking inicial obtenido por OpenCLIP y luego aplicar una segunda etapa de re-ranking mediante un modelo cross-modal tipo ALBEF.

## Resultado principal defendible

El resultado defendible es que el pipeline permite realizar recuperación multimodal inicial sobre textiles andinos usando embeddings contrastivos.

Esto permite verificar que ciertas consultas textuales recuperan imágenes visualmente relacionadas y que ciertas imágenes recuperan descripciones semánticamente cercanas.

## Limitación principal

La principal limitación es que el experimento valida una etapa de alineamiento, pero no implementa ALBEF completo con fusión profunda. Además, el dataset es pequeño y no permite asegurar generalización sobre todo el universo de textiles andinos.

## Mejora propuesta

La mejora principal sería convertir el experimento en un pipeline de dos etapas:

1. **Retrieval contrastivo inicial** con OpenCLIP.
2. **Re-ranking con fusión profunda** usando un modelo tipo ALBEF o cross-encoder multimodal.

De esta manera se seguiría de forma más completa la lógica de ALBEF: primero alinear, luego fusionar.

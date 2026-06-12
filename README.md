# MCC225 - ALBEF aplicado a textiles andinos

Repositorio del examen parcial virtual del curso **MCC225A - IA Generativa y Aprendizaje Multimodal**.

## Tema

**ALBEF: Align Before Fuse**

El trabajo estudia ALBEF como arquitectura visión-lenguaje que primero alinea imagen y texto mediante aprendizaje contrastivo y luego fusiona ambas modalidades mediante atención crossmodal.

## Enfoque del experimento

Este repositorio no implementa ALBEF completo desde cero.

Se usa **OpenCLIP** como línea base contrastiva reproducible para estudiar la etapa de alineamiento imagen-texto, equivalente al componente **ITC** de ALBEF.

La fusión profunda, **MLM** e **ITM** se discuten como componentes propios de ALBEF y como mejoras futuras.

## Cuadernos relacionados

- **C5:** deep fusion multimodal con transformers.
- **C6:** aprendizaje contrastivo, embeddings compartidos, hard negatives y retrieval.
- **C10:** OpenCLIP, zero-shot, prompts y recuperación imagen-texto.

## Resultado principal

Retrieval multimodal sobre imágenes de prueba inspiradas en textiles andinos:

- Imagen → texto.
- Texto → imagen.
- Similitud coseno.
- Rankings Top-K.
- Recall@1, Recall@3 y Recall@5.

## Estructura del repositorio

```text
notebooks/   Notebook principal ejecutado
src/         Funciones auxiliares del experimento
data/        Imágenes y textos candidatos
results/     Métricas, rankings y gráficos
docs/        Ficha, presentación y documentos finales
defense/     Hoja de trazabilidad y respuestas para defensa oral
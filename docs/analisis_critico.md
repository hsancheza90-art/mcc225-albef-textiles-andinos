# Análisis crítico del experimento

## Alcance del experimento

Este experimento implementa una prueba piloto de recuperación multimodal aplicada a textiles andinos. El objetivo no es entrenar un modelo ALBEF completo desde cero, sino reproducir de forma controlada la etapa de alineamiento imagen-texto usando OpenCLIP como baseline contrastivo.

La idea central defendida es que, antes de aplicar fusión profunda, es necesario verificar si imagen y texto pueden ser representados en un espacio compartido. Esta lógica está alineada con el principio de ALBEF: **Align Before Fuse**.

## Fortalezas del experimento

Una primera fortaleza es que el experimento es reproducible. El notebook permite ejecutar el flujo completo desde la carga de datos hasta la obtención de rankings y métricas.

Una segunda fortaleza es que el resultado no depende solo de una explicación conceptual. Se muestran evidencias concretas: embeddings, matriz de similitud, recuperación texto-imagen, recuperación imagen-texto, Recall@K y análisis de errores.

Una tercera fortaleza es que el experimento adapta conceptos del curso a un dominio propio: textiles andinos. Esto permite conectar los modelos multimodales con una línea de investigación aplicada.

Una cuarta fortaleza es que el notebook permite modificaciones en vivo. Se puede cambiar una consulta textual, variar el top-k, probar prompts alternativos, recalcular similitud coseno, identificar errores y analizar hard negatives.

## Limitaciones técnicas

La primera limitación es el tamaño del dataset. Al trabajar con un conjunto pequeño de imágenes y descripciones, los resultados deben interpretarse como una prueba piloto y no como una evaluación definitiva.

La segunda limitación es que OpenCLIP no fue entrenado específicamente para reconocer iconografía andina. Por ello, puede reconocer patrones generales como geometría, color o textura, pero no necesariamente significados culturales específicos.

La tercera limitación es que el experimento implementa principalmente alineamiento contrastivo, no fusión profunda completa. ALBEF incluye una etapa posterior de interacción entre tokens visuales y textuales, la cual no se entrena ni se reproduce completamente en este notebook.

La cuarta limitación está en las descripciones textuales. Si los textos son demasiado generales, el modelo puede recuperar imágenes visualmente parecidas pero no necesariamente correctas. Si los textos son demasiado específicos, puede fallar porque el modelo no conoce algunos términos culturales.

La quinta limitación corresponde a la métrica. Recall@K es útil para recuperación, pero puede ser rígida si existen varias descripciones válidas para una misma imagen. En textiles andinos, dos piezas pueden compartir patrones, colores o composiciones visuales similares.

## Posibles sesgos

El experimento puede presentar sesgo visual si las imágenes comparten fondos, colores, encuadres o estilos de captura. En ese caso, el modelo podría estar recuperando imágenes por similitud superficial y no por comprensión real del textil.

También puede existir sesgo textual si las descripciones usan palabras muy repetidas, como “geométrico”, “andino” o “tradicional”. Esto puede hacer que varias descripciones sean semánticamente cercanas aunque correspondan a imágenes distintas.

Otro posible sesgo es cultural. Un modelo generalista puede no distinguir adecuadamente entre motivos textiles andinos específicos, porque su entrenamiento original no necesariamente contiene suficiente información especializada sobre este dominio.

## Análisis de errores

Los errores observados pueden interpretarse en tres niveles.

Primero, puede haber errores de alineamiento. Esto ocurre cuando la imagen correcta no aparece entre los primeros resultados. En ese caso, el espacio compartido no logró acercar adecuadamente la imagen y su descripción.

Segundo, puede haber errores de ranking. Esto ocurre cuando la respuesta correcta aparece dentro del top-k, pero no en el primer lugar. En este caso, el modelo sí tiene una señal semántica, pero no suficiente precisión para ordenar correctamente los candidatos.

Tercero, puede haber errores por falta de fusión profunda. Esto ocurre cuando la diferencia entre dos textiles depende de detalles finos que un dual encoder no captura bien. En estos casos sería útil una segunda etapa tipo ALBEF, donde los tokens visuales y textuales interactúen directamente.

## Hard negatives

Un hard negative es un resultado incorrecto pero difícil de separar del correcto. En este experimento, un hard negative puede ser una imagen textil con patrones geométricos, colores o simetrías parecidas a las de la imagen correcta.

Estos casos son importantes porque permiten evaluar mejor el modelo. Si solo se usan negativos fáciles, el sistema puede parecer mejor de lo que realmente es. Al introducir hard negatives, se prueba si el modelo distingue diferencias más finas.

## Qué pasaría si se cambia el prompt

Si se cambia el prompt, el ranking puede cambiar. Esto ocurre porque OpenCLIP trabaja en modo zero-shot y depende de cómo se formula la consulta textual.

Por ejemplo, una consulta como “Andean textile” puede ser muy general. En cambio, una consulta como “traditional Andean woven textile with geometric patterns and natural wool colors” puede orientar mejor la búsqueda.

Por ello, una mejora razonable es probar varios prompts y usar prompt ensemble, promediando los embeddings de diferentes formulaciones.

## Qué pasaría si se cambia el checkpoint

Si se cambia el checkpoint, los resultados pueden variar porque cada modelo fue preentrenado con datos, tamaños y configuraciones distintas.

Un checkpoint puede capturar mejor texturas generales, mientras otro puede responder mejor a descripciones semánticas. Por eso, una comparación justa debería usar el mismo dataset, las mismas consultas, la misma métrica y solo cambiar el modelo o checkpoint.

## Qué pasaría si se cambia el dataset

Si se amplía el dataset, la tarea se vuelve más realista. También aumenta la probabilidad de errores, porque habrá más imágenes parecidas entre sí.

Un dataset más grande permitiría evaluar mejor la generalización, pero también exigiría una organización más cuidadosa: metadatos, etiquetas, descripciones normalizadas y separación entre entrenamiento, validación y prueba si se decide entrenar o ajustar un modelo.

## Qué resultado no se puede asegurar todavía

No se puede asegurar todavía que el modelo comprenda el significado cultural profundo de los textiles andinos.

El experimento demuestra recuperación multimodal inicial, pero no comprensión simbólica. Para afirmar comprensión cultural sería necesario incorporar conocimiento experto, descripciones más ricas, anotaciones especializadas y una evaluación diseñada con criterios antropológicos o textiles.

Tampoco se puede asegurar que ALBEF completo supere automáticamente a OpenCLIP en este dominio, porque esa comparación no fue implementada de forma experimental. Lo que sí se puede defender es que ALBEF ofrece una ruta técnica razonable para mejorar los casos donde el alineamiento contrastivo no es suficiente.

## Mejora futura

La mejora principal sería implementar un pipeline de dos etapas.

En la primera etapa se usaría OpenCLIP para recuperar los top-k candidatos mediante embeddings contrastivos. Esta etapa sería rápida y eficiente.

En la segunda etapa se usaría un modelo con fusión profunda tipo ALBEF para reordenar esos candidatos. Esta etapa sería más costosa, pero permitiría analizar interacciones más finas entre imagen y texto.

Además, se podrían agregar mejoras como:

* ampliar el dataset;
* incluir descripciones expertas;
* probar prompts en español e inglés;
* comparar checkpoints;
* incorporar FAISS para búsqueda eficiente;
* guardar embeddings para reproducibilidad;
* separar errores por color, geometría, motivo y composición;
* documentar mejor las rutas, dependencias y resultados esperados.

## Conclusión crítica

El experimento es suficiente como prueba piloto reproducible de alineamiento imagen-texto aplicado a textiles andinos. Su valor principal está en mostrar un flujo técnico completo: datos, embeddings, similitud, ranking, métrica y análisis de errores.

Sin embargo, todavía no debe presentarse como un sistema final de comprensión cultural ni como una implementación completa de ALBEF. La defensa correcta es ubicarlo como una primera etapa contrastiva, coherente con la idea de **Align Before Fuse**, y proponer la fusión profunda como siguiente paso técnico.

# Tarea 6: Corrección de salidas de OCR para líneas de documentos del pasado reciente

[← Volver a tareas](../README.md) · [← Cronograma](../../README.md#cronograma)

## La tarea

El objetivo es corregir errores de transcripciones de OCR (Reconocimiento Óptico de Caracteres) realizadas sobre documentos del pasado reciente, específicamente del archivo Berrutti, relacionados a la dictadura cívico-militar en Uruguay.

A partir de líneas de texto extraídas automáticamente utilizando el OCR Tesseract, se busca obtener versiones corregidas que sean lo más fieles posible al contenido original. Se podrá explorar distintos enfoques, incluyendo prompting o fine-tuning de modelos de lenguaje, entre otros.

## Evaluación

Para evaluar los métodos de corrección, se deberá comparar las salidas generadas a partir de las transcripciones de OCR con las transcripciones de referencia realizadas por humanos. Se recomienda utilizar principalmente las siguientes métricas:

- **Character Error Rate (CER):** mide la proporción de caracteres incorrectos en la transcripción corregida en comparación con la referencia.
- **Word Error Rate (WER):** mide la proporción de palabras incorrectas en la transcripción corregida en comparación con la referencia.

## Datos

El conjunto de datos incluye 21611 ejemplos de entrenamiento y 2163 ejemplos de test. Cada partición se compone de dos archivos CSV, _preds_ y _gts_, que contienen las transcripciones de Tesseract y las transcripciones de referencia, respectivamente. A su vez, se cuenta con las imágenes originales de las líneas de los documentos.

Se recomienda realizar una partición de validación a partir del conjunto de entrenamiento, y utilizar el conjunto de test únicamente para la evaluación final de los métodos desarrollados.

**Link al dataset:** [Luisa Tesseract Lines](https://gitlab.fing.edu.uy/memoria/datasets-publicos/-/tree/main/luisa_tesseract_lines)

## Referencias

- Artículo relacionado: [Post-OCR Correction Using Large Language Models with Constrained Decoding](https://hdl.handle.net/20.500.12008/56413)

# Tarea 6: Generación de preguntas críticas

> **Propuesta adicional, a evaluar.**

[← Volver a tareas](../README.md) · [← Cronograma](../../README.md#cronograma)

Basada en la tarea compartida **Critical Questions Generation** (12th Workshop on Argument Mining, ACL 2025): <https://hitz-zentroa.github.io/shared-task-critical-questions-generation/>

## Propuesta

Generar preguntas críticas para que las personas puedan interrogar un argumento y tener una idea de su confiabilidad. La tarea y su evaluación no están muy bien definidas, y eso abre la oportunidad de pensar cómo se podría encarar y evaluar de otras formas.

## La tarea original

Las **preguntas críticas** son las que habría que hacerse para juzgar si un argumento es aceptable o falaz: buscan desenmascarar los supuestos de las premisas y atacar la inferencia.

- **Entrada:** una intervención de un debate real.
- **Salida:** exactamente 3 preguntas críticas, que deberían ser útiles para cuestionar los argumentos del texto.

En el dataset, las intervenciones están anotadas con esquemas argumentativos y con un conjunto de preguntas críticas de referencia, etiquetadas según su utilidad:

- **Useful:** la respuesta podría cuestionar alguno de los argumentos del texto.
- **Unhelpful:** la pregunta es válida, pero es poco probable que cuestione algún argumento.
- **Invalid:** no sirve para cuestionar los argumentos, por ejemplo porque el razonamiento es incorrecto, no se relaciona con el texto, introduce conceptos nuevos, es demasiado general o no es crítica.

## Evaluación original

Cada pregunta generada se empareja con una pregunta de referencia por similitud semántica y recibe su etiqueta. Cada pregunta útil suma 1/3, de modo que cada intervención obtiene un puntaje entre 0 y 1.

## Datos

| Partición | Acceso |
|-----------|--------|
| Muestra | [sample.json](https://github.com/hitz-zentroa/shared-task-critical-questions-generation/blob/main/shared_task/data_splits/sample.json) |
| Validación | [validation.json](https://github.com/hitz-zentroa/shared-task-critical-questions-generation/blob/main/shared_task/data_splits/validation.json) |
| Test (sin preguntas de referencia) | [test.json](https://github.com/hitz-zentroa/shared-task-critical-questions-generation/blob/main/shared_task/data_splits/test.json) |

## Herramientas

- [Repositorio de la tarea](https://github.com/hitz-zentroa/shared-task-critical-questions-generation)
- [Scripts de evaluación](https://github.com/hitz-zentroa/shared-task-critical-questions-generation/tree/main/shared_task/eval_scripts)
- [Ejemplo de sistema y salida](https://github.com/hitz-zentroa/shared-task-critical-questions-generation/tree/main/shared_task/trial_submission)
- [Utilidades](https://github.com/hitz-zentroa/shared-task-critical-questions-generation/tree/main/shared_task/utils)

## Lecturas

- Calvo Figueras y Agerri (2024). *Critical Questions Generation: Motivation and Challenges.* CoNLL 2024. <https://aclanthology.org/2024.conll-1.9/>

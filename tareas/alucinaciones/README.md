# Tarea 5: Detección de alucinaciones en salidas de LLMs (Mu-SHROOM)

[← Volver a tareas](../README.md) · [← Cronograma](../../README.md#cronograma)

Basada en la tarea compartida **Mu-SHROOM** (*Multilingual Shared-task on Hallucinations and Related Observable Overgeneration Mistakes*), SemEval-2025 Task 3: <https://helsinki-nlp.github.io/shroom/2025>

## La tarea

Dada una respuesta generada por un LLM ajustado por instrucciones, detectar **qué fragmentos del texto son alucinaciones**.

- Para cada salida se dispone del texto (como cadena de caracteres), la lista de tokens y sus logits.
- Hay que predecir, para cada carácter de la salida, la probabilidad de que forme parte de una alucinación.
- Los datos son multilingües (14 lenguas, entre ellas español e inglés) y fueron generados por distintos LLMs de pesos abiertos.
- Se puede usar cualquier enfoque, incluidos recursos externos, y trabajar con cualquier subconjunto de lenguas.

## Evaluación

Se evalúa a nivel de caracteres, por separado para cada lengua, con dos métricas:

- **IoU:** intersección sobre unión entre los caracteres marcados como alucinación en la referencia y en la predicción.
- **Correlación:** entre la probabilidad asignada por el sistema y la proporción de anotadores que marcaron cada carácter.

## Datos

| Partición | Descarga |
|-----------|----------|
| Muestra | [sample.zip](https://a3s.fi/mickusti-2007780-pub/sample.zip) |
| Validación | [val.zip](https://a3s.fi/mickusti-2007780-pub/val.zip) |
| Entrenamiento (sin etiquetas) | [train.zip](https://a3s.fi/mickusti-2007780-pub/train.zip) |
| Test (sin etiquetas) | [test-unlabeled.zip](https://a3s.fi/mickusti-2007780-pub/test-unlabeled.zip) |
| Test (etiquetado) | [test_labeled.zip](https://a3s.fi/mickusti-2007780-pub/test_labeled.zip) |

Metadatos adicionales (anotaciones crudas y la página de Wikipedia usada como referencia por los anotadores): [mushroom-extra-info.tar.gz](https://a3s.fi/mickusti-2007780-pub/mushroom-extra-info.tar.gz)

## Herramientas

- [Kit para participantes](https://a3s.fi/mickusti-2007780-pub/participant_kit.zip): programa de evaluación, verificador de formato y dos baselines (aleatorio y uno basado en XLM-R).
- [scorer.py](https://helsinki-nlp.github.io/shroom/scorer.py): programa de evaluación.

## Referencias

- Sitio de la tarea: <https://helsinki-nlp.github.io/shroom/2025>
- Resultados oficiales: [ranking por IoU](https://helsinki-nlp.github.io/shroom/2025/iou_rankings) · [ranking por correlación](https://helsinki-nlp.github.io/shroom/2025/cor_rankings)
- [Preguntas frecuentes](https://helsinki-nlp.github.io/shroom/2025/FAQ_2025)
- Edición anterior: [SHROOM, SemEval-2024 Task 6](https://helsinki-nlp.github.io/shroom/2024.html)

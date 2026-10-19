# Tarea 3: ¿Los modelos de lenguaje predicen cómo leemos?

[← Volver a tareas](../README.md) · [← Cronograma](../../README.md#cronograma)

Los modelos de lenguaje asignan probabilidades a las palabras posibles dado un contexto. Estas probabilidades pueden usarse para estimar el **surprisal** de una palabra:

$$S(w_t)=-\log P(w_t\mid w_1,\ldots,w_{t-1})$$

La idea general de este proyecto es explorar hasta qué punto esta medida permite predecir distintas medidas del procesamiento humano del lenguaje: movimientos oculares, tiempos de lectura o actividad cerebral.

El proyecto admite varias variantes. No se espera que un grupo explore todas; la idea es elegir un dataset, formular una pregunta concreta y desarrollar el análisis a partir de ahí.

## Alcance para una semana

El documento presenta una agenda de investigación, no una lista de tareas que deba completarse. Cada equipo deberá definir su propio resultado mínimo y, si el tiempo lo permite, una extensión. La elección del dataset, la medida, el modelo y el nivel de análisis forma parte del problema.

Al final del primer día, el equipo debería poder expresar:

- una pregunta principal;
- una unidad de análisis;
- un resultado mínimo que pueda completarse aunque falle la extensión;
- dos o tres riesgos técnicos o metodológicos;
- un plan alternativo.

No es necesario obtener un efecto significativo. Un resultado nulo, una comparación que no mejora el baseline o un pipeline incompleto pueden ser resultados valiosos si el equipo identifica con precisión qué pudo evaluar, qué no y por qué.

## Decisiones que conviene documentar

Entre otras, este problema obliga a decidir:

- qué cuenta como contexto previo para el modelo;
- si el modelo permite calcular probabilidad izquierda a derecha o si se está utilizando otra aproximación;
- cómo se combinan los tokens cuando una palabra se divide en varias partes;
- cómo se alinean las palabras del estímulo, las medidas humanas y los tokens;
- qué controles léxicos y posicionales incluir;
- cómo representar la estructura repetida por participante e ítem;
- qué observaciones se excluyen y por qué;
- qué comparación constituye evidencia a favor o en contra de la hipótesis.

No hay que resolver todas estas decisiones del mismo modo. Sí es importante hacerlas visibles.

## Orientación de factibilidad

Las alternativas no tienen el mismo costo. Los corpus de eye-tracking con medidas ya agregadas por palabra permiten llegar relativamente rápido a un primer análisis. Procesar fijaciones desde los registros crudos, armonizar varias lenguas o trabajar con EEG desde la señal requieren bastante más tiempo.

En **Cuentos**, los datos son públicos y existe código oficial para extraer las medidas. El paquete procesado es manejable, pero todavía conserva una estructura de archivos por participante, ensayo y pantalla: producir la tabla final por palabra es una parte sustantiva del trabajo. Las señales crudas son mucho más voluminosas y no son necesarias para la pregunta mínima.

En **Provo**, las medidas de eye-tracking y predictibilidad están disponibles en archivos tabulares, por lo que constituye una alternativa de contingencia especialmente práctica. **MECO** también ofrece datos abiertos y reportes por áreas de interés, pero una comparación entre lenguas exige armonizar cuidadosamente muestras y variables.

Calcular surprisal para decenas de miles de posiciones con un modelo causal pequeño es factible en una computadora razonable, especialmente si se procesa cada texto una sola vez. No se debe volver a calcular el surprisal para cada participante. Comparar muchos modelos grandes o muchas configuraciones de contexto puede convertir el cómputo en el principal cuello de botella.

Las alternativas con EEG son viables si se parte de medidas derivadas a nivel de palabra o de ventanas temporales ya definidas. Partir de EEG crudo, realizar preprocesamiento completo y resolver comparaciones múltiples debería considerarse una extensión de alto riesgo para una sola semana.

## Algunas preguntas posibles

- ¿El surprisal predice los tiempos de lectura o la duración de las fijaciones?
- ¿Cuánto agrega surprisal una vez que controlamos por frecuencia y longitud?
- ¿Qué relación hay entre surprisal y medidas humanas de predictibilidad, como cloze probability?
- ¿Distintos modelos de lenguaje producen estimaciones de surprisal igualmente buenas para predecir datos humanos?
- ¿La relación entre surprisal y procesamiento es lineal?
- ¿Es mejor predictor $p$, $-\log p$, o alguna función intermedia?
- ¿Una relación encontrada en inglés se reproduce en español?
- ¿El mismo modelo permite explicar datos de distintas lenguas?
- ¿Surprisal predice de forma similar movimientos oculares y N400?

## Alternativa 1: eye-tracking en español — Cuentos

**Cuentos** es un corpus de eye-tracking obtenido durante la lectura de textos narrativos por hablantes de español latinoamericano.

Incluye aproximadamente 40.000 palabras, más de 940.000 fijaciones y datos de 113 participantes.

Artículo y datos:

https://www.nature.com/articles/s41597-026-06798-z

El punto de partida más directo sería estimar el surprisal de cada palabra con un modelo de lenguaje y evaluar su relación con alguna medida de eye-tracking, por ejemplo:

- first fixation duration;
- gaze duration;
- total reading time;
- refijaciones;
- regresiones.

Un primer modelo podría ser:

$$
RT \sim Surprisal + Frequency + Length
$$

y luego extenderse incorporando otros controles, efectos por participante o palabra, spillover, etc.

Para parte de los materiales existen además medidas humanas de **cloze probability**. Esto permite comparar directamente predictibilidad humana y predictibilidad del modelo:

$$
RT \sim Cloze
$$

$$
RT \sim Surprisal
$$

$$
RT \sim Cloze + Surprisal
$$

Una posible pregunta es si el surprisal del modelo captura básicamente la misma información que cloze o si explica variabilidad adicional en los movimientos oculares.

---

## Alternativa 2: generalización entre lenguas — MECO

El **Multilingual Eye-Movement Corpus (MECO)** contiene datos de eye-tracking durante lectura en múltiples lenguas, entre ellas inglés, español y portugués.

https://meco-read.com/

Parte de los textos son traducciones entre lenguas, lo que permite hacer comparaciones relativamente controladas.

Una posibilidad es calcular surprisal con un mismo modelo multilingüe y estimar, por separado, algo del estilo:

$$
RT \sim Surprisal + Frequency + Length
$$

en inglés, español, portugués u otras lenguas.

La pregunta sería en qué medida la relación entre surprisal y procesamiento se mantiene entre lenguas.

También se puede restringir el análisis a los textos paralelos y comparar las mismas unidades de contenido en distintos idiomas.

Esta alternativa se puede combinar con Cuentos: desarrollar primero el análisis en español y después evaluar si los resultados se reproducen en otras lenguas.

---

## Alternativa 3: expectativas humanas y expectativas de los modelos

Otra posibilidad es concentrarse en la relación entre las probabilidades producidas por el modelo y medidas humanas de predictibilidad.

Además de Cuentos, el **Provo Corpus** contiene eye-tracking y medidas de predictibilidad en inglés.

Esto permite comparar directamente:

$$
P_{\mathrm{human}}(w_t\mid context)
$$

con

$$
P_{\mathrm{LLM}}(w_t\mid context)
$$

y estudiar cuál de las dos medidas predice mejor los movimientos oculares, cuánto se solapan y qué información aporta cada una.

Una variante sería comparar varios modelos de lenguaje y evaluar cuáles producen distribuciones de probabilidad más cercanas a las estimaciones humanas.

---

## Alternativa 4: eye-tracking y EEG

Este dataset reúne medidas de predictibilidad, self-paced reading, eye-tracking y EEG para las mismas 205 oraciones y 1726 palabras:

https://link.springer.com/article/10.3758/s13428-023-02261-8

Datos y código: https://github.com/Andrea-de-Varda/prediction-resource

Esto permite calcular una única medida de surprisal para cada palabra y relacionarla con distintas medidas humanas:

$$
Surprisal \rightarrow RT
$$

$$
Surprisal \rightarrow EyeTracking
$$

$$
Surprisal \rightarrow N400
$$

Una pregunta posible es si la misma medida computacional de expectativa permite explicar las distintas respuestas, o si la relación depende de qué aspecto del procesamiento estamos midiendo.

Otra alternativa es **ZuCo**, que contiene EEG y eye-tracking registrados simultáneamente durante lectura:

https://www.nature.com/articles/sdata2018291

Esta opción puede requerir algo más de trabajo con los datos, sobre todo si se parte de las señales de EEG.

---

## La forma de la relación entre probabilidad y procesamiento

No es necesario asumir que surprisal sea la transformación correcta de la probabilidad.

La hipótesis estándar utiliza:

$$
-\log(p)
$$

pero podemos comparar esa relación con:

$$
p
$$

o estimar alguna función más flexible:

$$
f(p)
$$

Esto permite estudiar si el costo de encontrar una palabra inesperada crece aproximadamente con surprisal en todo el rango de probabilidades o si la relación cambia para palabras muy poco probables.

La comparación puede hacerse usando tiempos de lectura, movimientos oculares o N400.

---

## Comparar modelos

También se puede mantener fijo el dataset humano y calcular surprisal con distintos modelos.

Las comparaciones pueden involucrar:

- modelos de distinto tamaño;
- modelos monolingües y multilingües;
- distintas arquitecturas;
- distintos tamaños de contexto;
- LLMs frente a modelos más sencillos, por ejemplo n-grams.

La pregunta en este caso sería si los modelos que predicen mejor texto son también los que mejor predicen el procesamiento humano.

---

## Análisis mínimo

Para empezar alcanza con elegir:

1. un dataset;
2. una medida dependiente;
3. un modelo de lenguaje;
4. una forma de calcular surprisal.

El objetivo inicial es llegar a una tabla a nivel de palabra que combine las medidas humanas con las estimaciones del modelo:

| subject | word | surprisal | frequency | length | gaze_duration |
|---|---|---:|---:|---:|---:|
| S01 | children | 3.42 | 4.71 | 8 | 214 |
| S01 | went | 1.93 | 5.82 | 4 | 183 |
| S01 | outside | 4.17 | 4.26 | 7 | 247 |

A partir de ahí se puede empezar con visualizaciones y modelos relativamente sencillos, y agregar complejidad cuando sea necesario.

Hay algunos problemas técnicos que habrá que resolver. En particular, los modelos trabajan con tokens que no necesariamente corresponden a palabras, por lo que habrá que definir cómo obtener surprisal a nivel palabra. También hay que decidir cuánto contexto darle al modelo: una oración, un párrafo o todo el texto previo.

Por otro lado, frecuencia, longitud y otras propiedades léxicas afectan los tiempos de lectura independientemente de la predictibilidad. Finalmente, los datos tienen una estructura repetida por participante y por ítem, que deberá tenerse en cuenta en el análisis estadístico.

## Para empezar

Antes de elegir el modelo o escribir código, conviene definir una pregunta relativamente acotada. Por ejemplo:

> **¿El surprisal de un modelo de lenguaje predice gaze duration en español, por encima de frecuencia y longitud?**

o:

> **¿El efecto de surprisal sobre gaze duration es similar en inglés y español?**

o:

> **¿Surprisal y cloze probability explican la misma variabilidad en los tiempos de lectura?**

o:

> **¿La relación entre probabilidad y N400 se describe mejor mediante $p$ o mediante $-\log(p)$?**

A partir de esa pregunta se puede elegir el dataset, el modelo de lenguaje, las variables de control y el análisis.

## Datasets

### Cuentos

Eye-tracking durante lectura de textos narrativos en español.

https://www.nature.com/articles/s41597-026-06798-z

Datos: https://doi.org/10.6084/m9.figshare.28311908

Código: https://github.com/NeuroLIAA/reading-et

### MECO — Multilingual Eye-Movement Corpus

Eye-tracking durante lectura en múltiples lenguas, con una parte de los materiales compartidos entre idiomas.

https://meco-read.com/

### Predictibilidad, eye-tracking y EEG

Medidas de cloze, predictibilidad, self-paced reading, eye-tracking y EEG sobre los mismos materiales.

https://link.springer.com/article/10.3758/s13428-023-02261-8

### ZuCo

EEG y eye-tracking simultáneos durante lectura.

https://www.nature.com/articles/sdata2018291

### Provo Corpus

Eye-tracking y medidas de predictibilidad en inglés.

Datos: https://osf.io/sjefs/

---

El objetivo general es usar las probabilidades producidas por modelos de lenguaje como una medida computacional de expectativa y evaluar hasta qué punto esa medida permite explicar datos de procesamiento humano. A partir de ahí, cada grupo puede concentrarse en una comparación entre modelos, lenguas, medidas conductuales o medidas neurofisiológicas.

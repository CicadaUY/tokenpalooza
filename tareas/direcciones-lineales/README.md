# Tarea 4: ¿Qué dimensiones del significado están representadas linealmente en los modelos de lenguaje?

[← Volver a tareas](../README.md) · [← Cronograma](../../README.md#cronograma)

Los modelos de lenguaje representan palabras y textos como vectores de alta dimensión. Una pregunta bastante general es qué tipo de información semántica o psicolingüística está organizada de forma simple dentro de esos espacios.

Una posibilidad es que algunas propiedades estén representadas, al menos aproximadamente, como **direcciones lineales**.

Si tenemos una representación $h_i$ para una palabra y un rating humano $y_i$, podemos ajustar algo tan simple como:

$$
y_i \approx w^\top h_i + b
$$

donde $w$ define una dirección en el espacio de representaciones.

La idea general de este proyecto es explorar qué propiedades humanas pueden recuperarse de esta forma y, sobre todo, hasta qué punto esas direcciones **generalizan**.

No interesa solamente preguntar si un probe tiene buen desempeño en un conjunto de palabras. La pregunta más fuerte es si la dimensión aprendida se mantiene entre categorías, datasets, contextos, capas, modelos o lenguas.

## Alcance para una semana

El documento ofrece muchas direcciones posibles; no se espera recorrerlas todas. Cada equipo deberá definir qué entiende por un resultado mínimo y qué prueba adicional intentaría si el pipeline básico funciona.

Al final del primer día, el equipo debería poder expresar:

- una propiedad humana concreta;
- qué palabras se utilizarán para ajustar y para evaluar;
- qué representación se extraerá del modelo;
- qué resultado mínimo sería interpretable;
- una extensión y un plan alternativo;
- los principales riesgos de sobreajuste, fuga de información o confusión.

No es necesario que el probe funcione bien. Descubrir que una dimensión no generaliza, que un baseline sencillo obtiene el mismo desempeño o que el resultado depende de una decisión de representación puede ser el resultado principal.

## Decisiones que conviene documentar

Entre otras, este problema obliga a decidir:

- si la palabra se representa aislada o en contexto;
- cómo se combinan varios subtokens;
- qué capa o capas se utilizan;
- cómo se separan entrenamiento, validación y evaluación;
- cómo se elige la regularización sin utilizar el conjunto de evaluación;
- qué métrica es apropiada para ratings con escalas diferentes;
- qué baseline permite saber si las representaciones agregan información;
- cómo se examinan frecuencia, longitud, categoría gramatical u otros posibles confusores;
- qué clase de generalización permitiría sostener la conclusión.

Que una variable sea linealmente decodificable no demuestra por sí solo que el modelo utilice esa variable. El alcance de la interpretación debe corresponder al diseño de evaluación.

## Orientación de factibilidad

La versión con palabras aisladas, un único modelo y una única capa es técnicamente directa. Los datasets principales son públicos y tienen tamaños manejables: Glasgow contiene alrededor de 5.500 palabras, iconicidad unas 14.000 y Lancaster cerca de 40.000 conceptos. Extraer representaciones con un modelo base y ajustar regresiones ridge cabe razonablemente en una semana.

La generalización entre Glasgow y Warriner o entre Glasgow y Brysbaert también es factible y deja miles de palabras no vistas para una evaluación externa. Hay que excluir del test las palabras que aparecieron durante el entrenamiento y considerar que las escalas de ratings pueden ser diferentes.

Comparar varias capas de un mismo modelo sigue siendo manejable si las representaciones se guardan una sola vez. Comparar muchos modelos, estudiar palabras contextualizadas o transferir directamente una dirección entre lenguas requiere más decisiones de alineación y puede consumir buena parte de la semana. Recuperar geometrías perceptuales completas es una extensión más abierta que un probe escalar.

## Algunas preguntas posibles

- ¿Qué variables psicolingüísticas pueden recuperarse mediante un probe lineal?
- ¿Algunas propiedades tienen una representación más claramente lineal que otras?
- ¿Una dirección aprendida en una categoría semántica funciona en otras?
- ¿Una dirección aprendida en un dataset generaliza a otro conjunto de ratings de la misma propiedad?
- ¿Las mismas dimensiones aparecen en distintas capas del modelo?
- ¿Las direcciones son similares entre modelos?
- ¿Una dimensión aprendida en inglés puede aplicarse directamente a palabras en español?
- ¿Propiedades sensoriomotoras como visión, tacto o interocepción aparecen como direcciones separables?
- ¿Dimensiones como iconicidad, que involucran relaciones entre forma y significado, están representadas de forma lineal?
- ¿Las relaciones geométricas entre varias direcciones reproducen la organización observada en los ratings humanos?

---

## Alternativa 1: varias dimensiones sobre el mismo vocabulario — Glasgow Norms

Las **Glasgow Norms** contienen ratings para unas 5.500 palabras en varias dimensiones psicolingüísticas:

- valence;
- arousal;
- dominance;
- concreteness;
- imageability;
- familiarity;
- age of acquisition;
- semantic size;
- gender association.

Este dataset permite empezar con una pregunta relativamente sencilla:

> ¿Qué dimensiones humanas pueden recuperarse linealmente de las representaciones de un modelo?

Para cada variable se puede ajustar un modelo del tipo:

$$
y = w^\top h + b
$$

y evaluar su desempeño sobre palabras que no fueron utilizadas para estimar $w$.

Esto permite comparar, por ejemplo, si valence, concreteness o semantic size tienen una estructura más claramente lineal que age of acquisition o familiarity.

La comparación entre variables es solamente un punto de partida. También se puede estudiar:

- en qué capa aparece mejor cada dimensión;
- cuánto cambia la dirección entre capas;
- cuánto se mantiene entre modelos;
- qué pasa si se controla por frecuencia o longitud;
- cuánto del desempeño depende de categorías particulares de palabras.

---

## Alternativa 2: dimensiones sensoriomotoras — Lancaster Sensorimotor Norms

Las **Lancaster Sensorimotor Norms** contienen ratings para casi 40.000 conceptos en varias modalidades perceptuales y motoras.

Entre otras:

- visual;
- auditory;
- haptic;
- olfactory;
- gustatory;
- interoceptive;
- hand/arm action;
- foot/leg action;
- mouth/throat action;
- head action;
- torso action.

Sitio del dataset:

https://www.lancaster.ac.uk/psychology/lsnorms/

Este dataset permite estudiar si diferentes modalidades corresponden a direcciones distintas dentro del espacio del modelo.

Por ejemplo:

$$
w_{\text{visual}}
$$

$$
w_{\text{auditory}}
$$

$$
w_{\text{haptic}}
$$

$$
w_{\text{interoceptive}}
$$

Una primera pregunta puede ser simplemente qué tan bien se recupera cada dimensión.

Pero también se puede mirar la geometría entre las propias direcciones:

- ¿visual y haptic son más similares entre sí que visual e interoceptive?
- ¿las direcciones son aproximadamente independientes?
- ¿las relaciones entre dimensiones en el modelo se parecen a las correlaciones observadas entre ratings humanos?
- ¿hay modalidades que aparecen más claramente que otras?

Otra posibilidad es entrenar el probe usando solamente una categoría de palabras y evaluar su generalización a otras.

Por ejemplo:

$$
w_{\text{visual}}^{animals}
$$

y luego aplicarlo a objetos, alimentos, lugares, etc.

Esto permite distinguir una dimensión relativamente general de una correlación específica de una categoría.

---

## Alternativa 3: iconicidad

Existen ratings de iconicidad para más de 15.000 palabras inglesas.

Repositorio:

https://github.com/bodowinter/iconicity_ratings

La iconicidad resulta interesante porque no es una dimensión puramente semántica. Involucra una relación entre la forma de una palabra y su significado.

Una pregunta posible es:

> ¿La iconicidad aparece como una dirección lineal en las representaciones de un modelo de lenguaje?

Si aparece, hay varias posibilidades para estudiar qué está capturando realmente el probe.

Por ejemplo:

- semántica;
- ortografía;
- estructura subléxica;
- frecuencia;
- familias particulares de palabras;
- onomatopeyas o fonestemas.

Se pueden comparar representaciones de distintos modelos o capas, y evaluar cuánto cambia el resultado según el tipo de tokenización.

También se puede excluir grupos evidentes, como onomatopeyas, y preguntar si la dimensión sigue siendo recuperable.

---

## Alternativa 4: generalización entre datasets

Una dirección lineal puede funcionar bien dentro de un dataset simplemente porque está aprovechando regularidades particulares de ese conjunto.

Una prueba más fuerte consiste en entrenar la dirección con un dataset y evaluarla en otro.

Por ejemplo, para valence:

**Entrenamiento:** Glasgow Norms

**Evaluación:** Warriner et al.

o para concreteness:

**Entrenamiento:** Glasgow Norms

**Evaluación:** Brysbaert et al.

La idea sería estimar:

$$
w_{\text{valence}}^{Glasgow}
$$

y aplicarlo directamente a las representaciones de las palabras de Warriner, sin volver a estimar el probe.

Lo mismo puede hacerse con concreteness.

Esto permite preguntar si estamos recuperando una dimensión relativamente estable o simplemente ajustando bien un conjunto particular de ratings.

Datasets útiles para esta alternativa:

### Warriner et al.

Ratings de valence, arousal y dominance para unas 14.000 palabras.

### Brysbaert et al.

Ratings de concreteness para unas 40.000 palabras.

Estos datasets también pueden utilizarse al revés: entrenar con ellos y evaluar sobre Glasgow.

---

## Alternativa 5: palabras en contexto

La mayoría de las normas psicolingüísticas asignan un único valor a cada palabra. Esto ignora que una misma palabra puede tener sentidos diferentes según el contexto.

Las **Contextualized Sensorimotor Norms** contienen ratings sensorimotores para palabras ambiguas presentadas en contextos diferentes.

Repositorio:

https://github.com/seantrott/cs_norms

Esto permite comparar:

$$
h(\text{word})
$$

con:

$$
h(\text{word}\mid context)
$$

y preguntar si los cambios contextuales en las activaciones del modelo se corresponden con cambios en los ratings humanos.

Por ejemplo, si un sentido de una palabra es más visual o más manipulable que otro, ¿la representación contextualizada se desplaza en la dirección correspondiente?

Esta alternativa permite pasar de una representación fija de la palabra a una representación del significado en contexto.

---

## Alternativa 6: generalización entre lenguas

Si se utiliza un modelo multilingüe, una pregunta posible es si una dirección aprendida en una lengua funciona directamente en otra.

Por ejemplo, estimamos una dirección de valence usando ratings ingleses:

$$
w_{\text{valence}}^{EN}
$$

y luego la aplicamos a representaciones de palabras españolas:

$$
\hat y_{ES} = w_{\text{valence}}^{EN\top} h_{ES}
$$

Si existen ratings humanos en español, podemos evaluar directamente cuánto generaliza.

Lo mismo se puede intentar con otras dimensiones:

- concreteness;
- imageability;
- valence;
- arousal;
- dominance;
- sensorimotor strength;
- iconicity.

La pregunta en este caso es si la dirección corresponde a una dimensión conceptual compartida o si está ligada a la organización específica de una lengua dentro del modelo.

También se pueden comparar:

- EN → EN;
- ES → ES;
- EN → ES;
- ES → EN.

---

## Alternativa 7: de ratings a geometrías

No todas las propiedades humanas se expresan naturalmente como una única escala.

En algunos dominios existen juicios de similitud entre estímulos, por ejemplo:

- colores;
- pitch;
- timbre;
- consonantes;
- sabores;
- olores.

Trabajos recientes muestran que los modelos de lenguaje pueden reproducir bastante bien algunas de estas estructuras mediante juicios explícitos.

Una pregunta distinta es si esa estructura ya está presente en las representaciones internas.

Una posibilidad sería aprender una proyección lineal:

$$
z_i = W h_i
$$

y evaluar si las distancias entre los puntos $z_i$ reproducen las distancias perceptuales humanas.

La pregunta deja entonces de ser si una variable escalar es linealmente decodificable y pasa a ser:

> ¿Puede una transformación lineal recuperar la geometría perceptual humana?

Esto puede estudiarse con herramientas como correlaciones entre matrices de distancia o RSA.

---

## Comparar capas

En transformers no hay una única representación.

Para cada palabra podemos obtener:

$$
h_i^{(1)}, h_i^{(2)}, \ldots, h_i^{(L)}
$$

y estimar el mismo probe en distintas capas.

Esto permite estudiar dónde aparece mejor una dimensión y si su organización cambia a lo largo del modelo.

No necesariamente todas las variables deberían comportarse igual.

Por ejemplo, puede ocurrir que propiedades más relacionadas con forma léxica aparezcan temprano, mientras que dimensiones semánticas o experienciales sean más claras en capas posteriores.

---

## Comparar modelos

También se puede mantener fijo el dataset humano y comparar distintos espacios de representación:

- GloVe;
- fastText;
- BERT;
- RoBERTa;
- modelos multilingües;
- modelos generativos;
- modelos de distintos tamaños.

Una pregunta posible es si representaciones más complejas producen necesariamente dimensiones más claras o más generalizables.

Los modelos estáticos sirven además como baseline importante: varias dimensiones semánticas ya aparecen de forma bastante sistemática en espacios relativamente simples como GloVe.

---

## Análisis mínimo

Una versión mínima del proyecto requiere:

1. elegir una dimensión humana;
2. elegir un dataset;
3. obtener una representación vectorial para cada palabra;
4. separar palabras en train y test;
5. estimar una dirección lineal;
6. evaluar esa dirección sobre palabras no vistas.

El dataset final podría verse aproximadamente así:

| word | dim1 | dim2 | ... | rating |
|---|---:|---:|---:|---:|
| dog | 0.32 | -0.81 | ... | 5.9 |
| stone | -0.14 | 0.27 | ... | 2.1 |
| music | 0.64 | 0.11 | ... | 6.3 |

El modelo más sencillo puede ser una regresión lineal o ridge:

$$
y = H w + \epsilon
$$

A partir de ahí se pueden agregar análisis más exigentes.

---

## Qué significa que algo sea linealmente decodificable

Un buen resultado de un probe no implica automáticamente que el modelo “use” esa dimensión de la misma manera que una persona.

Un espacio de alta dimensión puede contener suficiente información como para reconstruir muchas variables.

Por eso, además del desempeño dentro de un dataset, conviene incorporar alguna prueba de generalización:

- nuevas palabras;
- nuevas categorías;
- otro dataset;
- otra tarea;
- otra capa;
- otro modelo;
- otra lengua.

Cuanto más se mantenga una misma dirección bajo estos cambios, más fuerte es la evidencia de que estamos frente a una organización relativamente estable del espacio.

---

## Para empezar

Conviene definir primero qué tipo de generalización interesa estudiar.

Por ejemplo:

> **¿Una dirección de valence aprendida en Glasgow generaliza a los ratings de Warriner?**

o:

> **¿Una dirección visual aprendida con animales generaliza a objetos y alimentos?**

o:

> **¿La dimensión de iconicidad aparece en distintas capas y modelos?**

o:

> **¿Una dirección sensorimotora cambia de la forma esperada cuando una palabra cambia de sentido en contexto?**

o:

> **¿Una dirección de concreteness aprendida en inglés funciona sobre palabras en español en un modelo multilingüe?**

A partir de esa pregunta se puede elegir el dataset, el modelo, la representación y la forma de validación.

## Datasets

### Glasgow Norms

Ratings en múltiples dimensiones psicolingüísticas para unas 5.500 palabras.

Artículo y materiales: https://eprints.gla.ac.uk/165799/

### Lancaster Sensorimotor Norms

Ratings perceptuales y motores para casi 40.000 conceptos.

https://www.lancaster.ac.uk/psychology/lsnorms/

### Iconicity Ratings

Ratings de iconicidad para más de 15.000 palabras.

https://github.com/bodowinter/iconicity_ratings

### Warriner et al.

Valence, arousal y dominance para unas 14.000 palabras.

Artículo: https://doi.org/10.3758/s13428-012-0314-x

### Brysbaert et al.

Concreteness para unas 40.000 palabras.

Artículo: https://doi.org/10.3758/s13428-013-0403-5

### Contextualized Sensorimotor Norms

Ratings sensorimotores de palabras ambiguas en contexto.

https://github.com/seantrott/cs_norms

### psychNorms

Colección de múltiples bases de normas psicológicas y psicolingüísticas, útil para buscar otras dimensiones.

https://github.com/Zak-Hussain/psychNorms

---

La idea general del proyecto es usar ratings humanos como una forma de interrogar la geometría de las representaciones de los modelos. El objetivo no es solamente mostrar que cierta variable puede predecirse, sino evaluar hasta qué punto esa variable corresponde a una dirección relativamente estable y reutilizable dentro del espacio representacional.

# Tarea 1: Biotokens — LLMs para secuencias de RNA y proteínas

**Área:** Biología / bioinformática\
**Responsables:** _a completar_\
**Equipo:** _a completar_

[← Volver a tareas](../README.md) · [← Cronograma](../../README.md#cronograma)

## Motivación

Las secuencias biológicas pueden verse como textos escritos en un alfabeto de nucleótidos (RNA) o aminoácidos (proteínas). Siguiendo la idea de los modelos de lenguaje, se han entrenado LLMs de forma auto-supervisada sobre enormes colecciones de secuencias sin anotar. Estos modelos asignan a cada base o residuo un vector (*embedding*) que puede reutilizarse, mediante *transfer learning*, en tareas supervisadas que cuentan con pocos datos anotados.

La tarea propone explorar esta idea en dos problemas:

- **Predicción de estructura secundaria de RNA:** a partir de la secuencia, predecir qué bases se aparean. Un desafío central es la generalización a familias de RNA no vistas durante el entrenamiento.
- **Clasificación de proteínas en familias (Pfam):** asignar dominios proteicos a familias usando embeddings de modelos de proteínas, como alternativa a los modelos ocultos de Markov que se usan habitualmente.

## Propuesta

_A completar por los responsables._ Algunas líneas posibles:

- Comparar los embeddings de distintos RNA-LLMs para predecir estructura secundaria, con conjuntos de datos de dificultad creciente (baja homología entre entrenamiento y test).
- Entrenar clasificadores de familias Pfam a partir de embeddings ESM2 y analizar el desempeño en familias con pocos ejemplos.
- Visualizar e interpretar los embeddings: ¿qué información biológica capturan?

## Datos

| Problema | Conjuntos de datos | Embeddings | Acceso |
|----------|--------------------|------------|--------|
| RNA | ArchiveII, bpRNA + bpRNA-new, PDB-bpRNA | RNABERT, RNA-FM, RNA-MSM, ERNIE-RNA, RiNALMo | [Zenodo](https://zenodo.org/records/13821093) |
| Proteínas | Pfam mini | ESM2 | [Google Drive](https://drive.google.com/drive/folders/1uv4IfmJR_SmGgKrDGLVFnqRx6_9BupKJ?usp=drive_link) |

También en [RNA data.txt](RNA%20data.txt) y [Protein data.txt](Protein%20data.txt).

## Código de referencia

- [sinc-lab/rna-llm-folding](https://github.com/sinc-lab/rna-llm-folding): benchmark de RNA-LLMs para estructura secundaria.
- [sinc-lab/llm4pfam](https://github.com/sinc-lab/llm4pfam): anotación de proteínas con embeddings de LLMs.
- [sinc-lab/ET-Pfam](https://github.com/sinc-lab/ET-Pfam): ensambles con transfer learning para predicción de familias Pfam.

## Lecturas

### Introducción al RNA

- Dotu, Stich y Aguirre. *Estructura, plegamiento y evolución del RNA* (capítulo 14). [PDF](RNA%20estructura%20plegamiento%20y%20evolucion.pdf)
- Sacco, Bussi y Sanguinetti (2026). *Machine learning for RNA secondary structure prediction: a review of current methods and challenges.* RNA 32:443–456. [PDF](RNA%20secondary%20structure%20prediction%202026.pdf)

### LLMs para RNA

- Zablocki et al. (2025). *Comprehensive benchmarking of large language models for RNA secondary structure prediction.* Briefings in Bioinformatics 26(2), bbaf137. [doi:10.1093/bib/bbaf137](https://doi.org/10.1093/bib/bbaf137) · [PDF](LLM%20RNA%20secondary%20structure%20bbaf137.pdf)

### LLMs para proteínas

- Vitale et al. (2024). *Evaluating large language models for annotating proteins.* Briefings in Bioinformatics 25(3), bbae177. [doi:10.1093/bib/bbae177](https://doi.org/10.1093/bib/bbae177) · [PDF](LLM%20protein%20annotation%20bbae177.pdf)
- Duarte et al. (2026). *ET-Pfam: ensemble transfer learning for protein family prediction.* Bioinformatics 42(4), btag121. [doi:10.1093/bioinformatics/btag121](https://doi.org/10.1093/bioinformatics/btag121) · [PDF](LLM%20transfer%20learning%20protein%20family%20prediction%20btag121.pdf)

## Charlas relacionadas

- [Charla 2 — Diego Milone (introducción)](../../charlas/02-milone-intro.md)
- [Charla 5 — Diego Milone](../../charlas/05-milone.md)

## Recursos de cómputo

_A completar._

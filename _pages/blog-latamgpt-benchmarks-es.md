---
permalink: /posts/2026/blog-latamgpt-benchmarks-es/
title: 'Lo que LatamGPT sabe (y lo que no): una primera mirada a sus benchmarks'
author_profile: true
---

<p style="text-align:center;"><em><a href="/posts/2026/blog-latamgpt-benchmarks/">Read in English</a></em></p>

<p style='text-align: justify;'>
En febrero de 2026, <a href="https://www.france24.com/en/live-news/20260210-latam-gpt-a-latin-american-ai-to-combat-us-centric-bias">LatamGPT fue presentado</a> en medio de un gran entusiasmo regional: el primer modelo de lenguaje grande y abierto construido <em>desde y para</em> América Latina y el Caribe, coordinado por el CENIA de Chile junto a más de sesenta instituciones de la región. La promesa era soberanía y relevancia cultural: pasar de consumir modelos extranjeros a producir los propios. Pero cuando los <a href="https://www.linkedin.com/posts/omar-u-florez_latamgpt-share-7467314002256257025-L45F/">pesos abiertos se liberaron el 1 de junio de 2026</a>, llegaron sin una sola cifra de benchmark, y el informe técnico aún no aparece. Como alguien que lleva años trabajando con modelos de lenguaje, tuve la curiosidad de no esperar—así que hice una evaluación temprana e independiente sobre un conjunto de tareas solo en español más los benchmarks culturales que el CENIA acaba de publicar. Tómalo como una vista previa, a confirmar cuando lleguen las cifras oficiales. La pregunta que quería responder es simple: ¿qué sabe LatamGPT que otros modelos no?
</p>

## Qué es LatamGPT, y con qué lo comparé

<p style='text-align: justify;'>
<a href="https://huggingface.co/latam-gpt/Llama-3.1-70B-LatamGPT-SFT-1.0">LatamGPT</a> parte de Llama 3.1 70B de Meta (el modelo base) y lo adapta en dos etapas: <em>pre-entrenamiento continuo</em> (CPT) sobre unas <a href="https://aibusiness.com/generative-ai/the-new-open-source-ai-model-for-latin-america">230 mil millones de palabras</a> de texto regional con licencia—español, portugués y lenguas indígenas—seguido de ajuste supervisado (SFT) para seguir instrucciones. El modelo liberado, entonces, es un modelo instruct—y eso define la comparación: la vara justa no es GPT-5 ni un ideal abstracto, sino otros modelos instruct de tamaño similar o menor. Lo enfrento a tres: el propio <strong>Llama 3.1 70B Instruct</strong> de Meta (construido desde la misma base), y dos modelos más nuevos y pequeños, <strong>Gemma 4 31B</strong> y <strong>Qwen3.6 27B</strong>. Los cuatro son modelos instruct, evaluados de la misma manera.
</p>

<p style='text-align: justify;'>
Así se comparan los cuatro modelos en todos los benchmarks que evalué:
</p>

<p align="center">
    <img src="/images/latamgpt-benchmarks-es.png" width="760">
	<center>
	<figcaption>En los benchmarks generales en español (izquierda), LatamGPT (rojo) queda por debajo del Llama 3.1 70B Instruct de Meta (azul) y de los dos modelos más nuevos y pequeños—salvo en HeadQA, donde empata (y el puntaje de Gemma parece un artefacto de evaluación). En los benchmarks culturales del CENIA (derecha) la imagen cambia: LatamGPT encabeza CHOCLO de forma rotunda, aunque Gemma gana el Trueque, más pequeño y revisado por humanos.</figcaption>
	</center>
</p>

## Donde rinde frutos: el conocimiento latinoamericano

<p style='text-align: justify;'>
Si LatamGPT sabe algo que otros modelos no, es aquí donde debería notarse: en los benchmarks construidos para la región. CHOCLO—<a href="https://www.linkedin.com/posts/te-invitamos-a-nuestro-pr%C3%B3ximo-webinar-donde-ugcPost-7450985197631791105-XLKo/">un lanzamiento del CENIA dentro del mismo esfuerzo de Latam-GPT</a>, disponible abiertamente <a href="https://huggingface.co/datasets/latam-gpt/CHOCLO">en Hugging Face</a>—es un benchmark de 104,847 preguntas sobre conocimiento cultural latinoamericano (comida, flora y fauna, geografía, tradiciones, figuras públicas). Lo evalúo con un juez LLM que decide si cada respuesta es semánticamente equivalente o no a la respuesta de referencia; el número es la proporción de respuestas juzgadas equivalentes (los detalles del juez están en la nota al final). Aquí LatamGPT queda primero: el 39.5% de sus respuestas se juzgan equivalentes, por delante de Llama 3.1 Instruct (37.6) y bastante por delante de los modelos más nuevos (Gemma 4 31B con 31.3, Qwen3.6 27B con 27.1)—y esa ventaja se mantiene en todos los niveles de dificultad.
</p>

<p style='text-align: justify;'>
Este es el resultado alentador. El conocimiento cultural latinoamericano no surge de la escala ni de la novedad—Gemma y Qwen son más nuevos y potentes, y aun así quedan por detrás aquí, porque los datos sobre un plato chileno o una tradición peruana simplemente nunca estuvieron en su entrenamiento. No se puede razonar hasta un conocimiento que nunca se vio. El corpus regional metió ese conocimiento, y CHOCLO lo detecta.
</p>

<p style='text-align: justify;'>
Pero no gana <em>todas</em> las pruebas culturales. <a href="https://huggingface.co/datasets/latam-gpt/Trueque-Benchmark-beta-0.1">Trueque</a>—un benchmark de QA latinoamericano más pequeño, colaborativo y revisado por humanos (500 preguntas en esta beta, evaluado de la misma manera)—cuenta una historia más mixta. LatamGPT vuelve a superar a Llama 3.1 Instruct (54.2 vs. 51.6) y a Qwen (46.0), pero Gemma 4 31B queda arriba, 67.0 contra 54.2. En los datos de cola larga generados automáticamente de CHOCLO, los datos regionales son decisivos; en las preguntas más amplias y revisadas por humanos de Trueque, un modelo general más potente puede imponerse.
</p>

<p style='text-align: justify;'>
Ambos benchmarks culturales lado a lado, con CHOCLO desglosado por dificultad:
</p>

| Equivalencia semántica binaria (%) | LatamGPT 70B | Llama 3.1 70B | Gemma 4 31B | Qwen3.6 27B |
| --- | --- | --- | --- | --- |
| CHOCLO — General (n = 104,847) | **39.5** | 37.6 | 31.3 | 27.1 |
| — Fácil | **59.8** | 57.1 | 48.4 | 45.1 |
| — Intermedia | **33.9** | 32.3 | 24.3 | 20.7 |
| — Difícil | **24.9** | 23.6 | 21.3 | 15.6 |
| Trueque — General (n = 500, beta) | 54.2 | 51.6 | **67.0** | 46.0 |

## Donde se queda atrás: la capacidad general

<p style='text-align: justify;'>
Ahora el otro lado. A diferencia del inglés, no existe un conjunto de benchmarks estándar y consensuado para los LLM en español, así que armé uno con tareas disponibles abiertamente que reflejan lo que normalmente se reporta para los modelos en inglés—y en estas, LatamGPT queda por debajo de los tres modelos de comparación. En MMLU-ProX (es)—el benchmark de razonamiento más limpio y completamente en español del conjunto—obtiene 45.8, frente al 61.5 de Llama 3.1 Instruct y más de 80 de Gemma. La matemática escolar (MGSM) llega a 78.8 (vs. 88.8 de Llama Instruct); el razonamiento emocional de EQ-Bench, a 69.0 (vs. 75.4). La única tarea general donde empata es HeadQA, un examen médico tipo QA (51.2 vs. 50.4).
</p>

<p style='text-align: justify;'>
Las cifras exactas de los benchmarks generales:
</p>

| Benchmark general (español) | Shots | n | Métrica | LatamGPT 70B | Llama 3.1 70B | Gemma 4 31B | Qwen3.6 27B |
| --- | --- | --- | --- | --- | --- | --- | --- |
| MMLU-ProX (es) | 5 | 11,759 | EM | 45.81 | 61.48 | **82.86** | 77.82 |
| MGSM native-CoT (es) | 8 | 250 | EM | 78.80 | 88.80 | **90.80** | 86.80 |
| EQ-Bench (es) | 0 | 168 | EQ-Bench | 68.95 | 75.35 | **78.06** | 77.70 |
| HeadQA (es) | 0 | 2,742 | Acc | 51.17 | 50.40 | 20.97 | **52.04** |

<p style='text-align: justify;'>
EM = exact match (coincidencia exacta), Acc = accuracy (exactitud); EQ-Bench usa su propia escala 0–100. En negrita, el mejor puntaje de cada fila. Los cuatro son modelos instruct; la columna "Llama 3.1 70B" es el checkpoint Instruct (ver la nota al final). El puntaje de Gemma en HeadQA se deja de lado como artefacto.
</p>

## ¿Por qué la brecha?

<p style='text-align: justify;'>
Entonces el modelo liberado conoce la región mejor que cualquiera de los demás, pero queda por detrás en lo académico general. ¿Por qué? El pre-entrenamiento continuo es engañosamente difícil: empujas los pesos de un modelo ya terminado hacia una nueva distribución de datos, y no tiene ninguna obligación de conservar intactas sus habilidades anteriores mientras lo haces. El modo de fallo clásico es el <em>olvido catastrófico</em>—un modelo que pierde lo que sabía a medida que aprende algo nuevo—un riesgo real y bien estudiado siempre que sigues entrenando un modelo terminado, y uno que he estudiado de primera mano, en <a href="https://aclanthology.org/2024.findings-emnlp.38/">aprendizaje continuo</a> y <a href="https://aclanthology.org/2021.emnlp-main.240/">pre-entrenamiento continuo</a>. Eso lo convierte en el principal sospechoso de la caída de LatamGPT en capacidad general, ya sea que se haya colado en la etapa de CPT, la de SFT, o ambas.
</p>

<p style='text-align: justify;'>
Pero no puedo probarlo. Atribuir la brecha al olvido de forma limpia requeriría el checkpoint solo-CPT—antes del ajuste por instrucciones—y ese no se ha liberado, así que por ahora queda como hipótesis. Aun así, un detalle sugiere que la caída es al menos en parte real. El ajuste por instrucciones, si acaso, eleva ligeramente los puntajes tipo MMLU en lugar de bajarlos—por ejemplo, Llama 3.1 70B pasa de 79.3 (base) a <a href="https://huggingface.co/meta-llama/Llama-3.1-70B-Instruct">83.6 (instruct)</a>. Así que el ajuste por instrucciones de LatamGPT debería haber subido su puntaje, no bajarlo. Sin embargo, en MMLU-ProX queda en 45.8—unos 16 puntos por debajo del 61.5 de Llama 3.1 Instruct, desde la misma base. Una diferencia tan grande, en la dirección opuesta a lo que hace el ajuste, apunta de vuelta a la etapa de pre-entrenamiento continuo. Frente a los modelos más nuevos es en parte otra historia: Gemma 4 y Qwen3.6 son unos dos años más nuevos que la base Llama de 2024 de LatamGPT, así que parte de esa brecha es solo cuestión de época. El informe técnico—e idealmente los pesos intermedios, solo-CPT—lo resolverían.
</p>

## Entonces, ¿cuál es la contribución real?

<p style='text-align: justify;'>
Demos un paso atrás, y las cifras tempranas plantean una pregunta que me parece más interesante que cualquier puntaje aislado: <strong>¿cuál es la contribución duradera aquí—el modelo o los datos?</strong> Los pesos abiertos quedan superados rápido—<a href="https://aiflashreport.com/model-releases.html">no dejan de aparecer modelos abiertos potentes</a>—así es el ritmo del campo. Pero el corpus regional y benchmarks como CHOCLO son el tipo de activo que no caduca. Algunas preguntas que vale la pena considerar como comunidad: ¿necesita la región <em>entrenar</em> un modelo, o <em>curar</em> datos que puedan moldear <em>cualquier</em> modelo? CHOCLO ya muestra dónde se concentra el valor—el conocimiento latinoamericano que codifica es justo donde LatamGPT saca ventaja. Esa señal puede ser más poderosa como recurso portátil—usable mediante <a href="https://arxiv.org/abs/2511.01649">generación aumentada por recuperación</a>, ejemplos en contexto, <a href="https://arxiv.org/abs/2402.10946">ajuste fino más económico</a>, o como una <a href="https://platform.claude.com/docs/en/agents-and-tools/agent-skills/overview">habilidad</a> de competencia cultural invocable por sistemas de agentes—que congelada en un único conjunto de pesos.
</p>

<p style='text-align: justify;'>
Nada de esto es un veredicto—es una lectura temprana, y el informe oficial puede afinar o revertir partes de ella. Lo que ya está claro es lo que LatamGPT logró: una colaboración grande y multinacional que reunió datos regionales con licencia a escala—incluyendo portugués y lenguas indígenas que los modelos comerciales suelen ignorar—construyó capacidad real de entrenamiento en la región, y produjo el modelo que mejor captura el conocimiento cultural latinoamericano hoy. Mucho de eso nunca aparece en un puntaje de benchmark, y sí importa. Es un primer lanzamiento, una versión 1.0 con mucho espacio para crecer, y los datos detrás son un bien público real. Si los modelos son los motores, los datos definen el terreno—y lo más duradero que puede hacer la región es codificar su terreno con la suficiente profundidad como para que todo modelo, el de este año o el del próximo, tenga que aprenderlo.
</p>

## Una nota sobre los números

<p style='text-align: justify;'>
Algunas salvedades. Todos los benchmarks están en español—tanto los prompts de generación de respuestas como los de evaluación. Los benchmarks académicos se corrieron con <a href="https://github.com/EleutherAI/lm-evaluation-harness/">lm-evaluation-harness de EleutherAI</a>; CHOCLO y Trueque se evaluaron con <a href="https://github.com/confident-ai/deepeval">DeepEval</a> usando un único juez externo (gpt-5.4-mini) aplicado de forma idéntica a cada modelo, así que esas columnas son comparables. La evaluación nativa de CHOCLO es híbrida (léxica, embeddings, juez LLM); yo reporto solo la parte de juez LLM, como una decisión binaria equivalente/no equivalente, y como los repositorios no documentan del todo su evaluación, conviene leer los puntajes absolutos de CHOCLO/Trueque como internamente consistentes más que oficiales. Los cuatro son modelos instruct; la base de comparación de Llama es <code>Llama-3.1-70B-Instruct</code>. LatamGPT en sí es <code>Llama-3.1-70B</code> (base) + CPT + SFT; el checkpoint solo-CPT no es público, así que comparo los modelos instruct liberados y no puedo aislar la etapa de CPT. Uso MMLU-ProX (es) como número de MMLU porque está completamente en español. Gemma 4 31B y Qwen3.6 27B se ejecutaron con el razonamiento ("thinking") desactivado. Cada resultado proviene de una sola corrida, salvo Gemma 4 31B en HeadQA, que corrí tres veces—≈21 (por debajo del azar) cada vez, casi con certeza un artefacto de evaluación.
</p>

## Referencias

- Ficha del modelo LatamGPT: [latam-gpt/Llama-3.1-70B-LatamGPT-SFT-1.0](https://huggingface.co/latam-gpt/Llama-3.1-70B-LatamGPT-SFT-1.0) y la [organización Latam-GPT](https://huggingface.co/latam-gpt) en Hugging Face.
- Dataset CHOCLO: [latam-gpt/CHOCLO](https://huggingface.co/datasets/latam-gpt/CHOCLO).
- Dataset Trueque: [latam-gpt/Trueque-Benchmark-beta-0.1](https://huggingface.co/datasets/latam-gpt/Trueque-Benchmark-beta-0.1).
- Omar U. Flórez (2026). [*Anuncio del lanzamiento de los pesos abiertos de LatamGPT (1 de junio de 2026)*](https://www.linkedin.com/posts/omar-u-florez_latamgpt-share-7467314002256257025-L45F/).
- CENIA (2026). [*Benchmarks CHOCLO y Trueque — anuncio de lanzamiento*](https://www.linkedin.com/posts/te-invitamos-a-nuestro-pr%C3%B3ximo-webinar-donde-ugcPost-7450985197631791105-XLKo/) (LinkedIn).
- France24 (2026). [*Latam-GPT: a Latin American AI to combat US-centric bias*](https://www.france24.com/en/live-news/20260210-latam-gpt-a-latin-american-ai-to-combat-us-centric-bias).
- AI Business (2026). [*Meet Latam-GPT, the New Open Source AI Model for Latin America*](https://aibusiness.com/generative-ai/the-new-open-source-ai-model-for-latin-america) (fuente de la cifra de ~230 mil millones de palabras / 300 mil millones de tokens, atribuida al CENIA).
- Brookings (2026). [*Latam-GPT and the search for AI sovereignty*](https://www.brookings.edu/articles/latam-gpt-and-the-search-for-ai-sovereignty/).
- Rest of World (2025). [*Latin America is building LatamGPT to rival ChatGPT*](https://restofworld.org/2025/chatgpt-latin-america-alternative-latamgpt/).
- Vladimir Araujo, Andrés Villa, Marcelo Mendoza, Marie-Francine Moens, and Alvaro Soto. 2021. [*Augmenting BERT-style Models with Predictive Coding to Improve Discourse-level Representations*](https://aclanthology.org/2021.emnlp-main.240/). EMNLP 2021.
- Vladimir Araujo, Marie-Francine Moens, and Tinne Tuytelaars. 2024. [*Learning to Route for Dynamic Adapter Composition in Continual Learning with Language Models*](https://aclanthology.org/2024.findings-emnlp.38/). Findings of EMNLP 2024.
- EleutherAI. [*lm-evaluation-harness*](https://github.com/EleutherAI/lm-evaluation-harness/) (evaluación de benchmarks académicos).
- Confident AI. [*DeepEval*](https://github.com/confident-ai/deepeval) (evaluación con LLM como juez, usada para CHOCLO y Trueque).

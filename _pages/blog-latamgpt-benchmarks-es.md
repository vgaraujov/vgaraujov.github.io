---
permalink: /posts/2026/blog-latamgpt-benchmarks-es/
title: 'Lo que LatamGPT sabe (y lo que no): una primera mirada a sus benchmarks'
author_profile: true
---
<p style="text-align:center;"><em><a href="/posts/2026/blog-latamgpt-benchmarks/">Read in English</a></em></p>

<p style='text-align: justify;'>
En febrero de 2026, <a href="https://www.france24.com/en/live-news/20260210-latam-gpt-a-latin-american-ai-to-combat-us-centric-bias">se presentó LatamGPT</a> en medio de mucho entusiasmo en la región: el primer modelo de lenguaje grande y abierto hecho <em>desde y para</em> América Latina y el Caribe, coordinado por el CENIA de Chile junto a más de sesenta instituciones. La promesa era soberanía y relevancia cultural: dejar de ser solo consumidores de modelos extranjeros para pasar a producir los propios. Pero cuando los <a href="https://www.linkedin.com/posts/omar-u-florez_latamgpt-share-7467314002256257025-L45F/">pesos abiertos salieron el 1 de junio de 2026</a>, llegaron sin una sola cifra de benchmark, y el reporte técnico todavía no aparece. Como alguien que lleva años trabajando con modelos de lenguaje, me ganó la curiosidad y no quise esperar: corrí una evaluación temprana e independiente sobre un conjunto de tareas en español, más los benchmarks culturales que el CENIA acaba de publicar. Tómalo como un adelanto, a confirmar cuando salgan las cifras oficiales. La pregunta que quería responder es sencilla: ¿qué sabe LatamGPT que los demás modelos no?
</p>

## Qué es LatamGPT y contra qué lo comparé

<p style='text-align: justify;'>
<a href="https://huggingface.co/latam-gpt/Llama-3.1-70B-LatamGPT-SFT-1.0">LatamGPT</a> parte del Llama 3.1 70B de Meta (el modelo base) y lo adapta en dos etapas: <em>continued pre-training</em> (CPT) sobre unas <a href="https://aibusiness.com/generative-ai/the-new-open-source-ai-model-for-latin-america">230 mil millones de palabras</a> de texto regional—en español, portugués y lenguas indígenas—y después un <em>supervised fine-tuning</em> (SFT) para que siga instrucciones. Es decir, el modelo que se liberó es un modelo instruct, y eso marca la comparación: la vara justa no es GPT-5 ni un ideal abstracto, sino otros modelos instruct de tamaño parecido o menor. Lo comparo contra tres: el propio <strong>Llama 3.1 70B Instruct</strong> de Meta (que sale de la misma base) y dos modelos más nuevos y pequeños, <strong>Gemma 4 31B</strong> y <strong>Qwen3.6 27B</strong>. <strong>Los cuatro son modelos instruct</strong>, evaluados de la misma forma.
</p>

<p style='text-align: justify;'>
Así se comparan los cuatro modelos en todos los benchmarks que corrí:
</p>

<p align="center">
    <img src="/images/latamgpt-benchmarks-es.png" width="760">
	<center>
	<figcaption>Los benchmarks de conocimiento latinoamericano a la izquierda; los benchmarks generales en español a la derecha. LatamGPT encabeza CHOCLO y supera a Llama 3.1 Instruct en los dos sets culturales —aunque Gemma se queda con Trueque, más pequeño y revisado por humanos— y queda por detrás de los modelos de comparación en los benchmarks generales (salvo en HeadQA, donde el puntaje de Gemma parece un artefacto de evaluación).</figcaption>
	</center>
</p>

## Donde rinde: el conocimiento latinoamericano

<p style='text-align: justify;'>
Si LatamGPT sabe algo que los demás no, es aquí donde tendría que notarse: en los benchmarks hechos para la región. CHOCLO—<a href="https://www.linkedin.com/posts/te-invitamos-a-nuestro-pr%C3%B3ximo-webinar-donde-ugcPost-7450985197631791105-XLKo/">un dataset que el CENIA liberó dentro del mismo esfuerzo de Latam-GPT</a>, disponible abiertamente <a href="https://huggingface.co/datasets/latam-gpt/CHOCLO">en Hugging Face</a>—tiene 104,847 preguntas de conocimiento cultural latinoamericano (comida, flora y fauna, geografía, tradiciones, figuras públicas). Lo evalúo con un LLM como juez que decide si cada respuesta es o no semánticamente equivalente a la de referencia; el número es la proporción de respuestas que da por equivalentes (los detalles del juez van en la nota al final). Aquí LatamGPT queda primero: el 39.5% de sus respuestas se juzgan equivalentes, por encima de Llama 3.1 Instruct (37.6) y bastante por delante de los modelos más nuevos (Gemma 4 31B con 31.3, Qwen3.6 27B con 27.1), y esa ventaja se sostiene en todos los niveles de dificultad.
</p>

<p style='text-align: justify;'>
Este es el resultado alentador. El conocimiento cultural latinoamericano no aparece solo por escala ni por ser más reciente: Gemma y Qwen son más nuevos y potentes, y aun así se quedan atrás aquí, porque los datos sobre un plato chileno o una tradición peruana simplemente nunca estuvieron en su entrenamiento. No se puede razonar hacia un conocimiento que nunca se vio. El corpus regional metió ese conocimiento, y CHOCLO lo detecta.
</p>

<p style='text-align: justify;'>
Pero no gana <em>todas</em> las pruebas culturales. <a href="https://huggingface.co/datasets/latam-gpt/Trueque-Benchmark-beta-0.1">Trueque</a>—un benchmark de QA latinoamericano más pequeño, colaborativo y revisado por humanos (500 preguntas en esta beta, evaluado igual)—cuenta una historia más matizada. LatamGPT otra vez le gana a Llama 3.1 Instruct (54.2 vs. 51.6) y a Qwen (46.0), pero Gemma 4 31B se queda con el primer lugar, 67.0 contra 54.2. En los datos de cola larga y generados automáticamente de CHOCLO, lo regional es decisivo; en las preguntas más amplias y revisadas por humanos de Trueque, un modelo general más fuerte todavía se puede imponer.
</p>

<p style='text-align: justify;'>
Los dos benchmarks culturales, lado a lado, con CHOCLO desglosado por dificultad:
</p>

| Equivalencia semántica binaria (%) | LatamGPT 70B   | Llama 3.1 70B | Gemma 4 31B    | Qwen3.6 27B |
| ----------------------------------- | -------------- | ------------- | -------------- | ----------- |
| CHOCLO — Total (n = 104,847)       | **39.5** | 37.6          | 31.3           | 27.1        |
| — Fácil                           | **59.8** | 57.1          | 48.4           | 45.1        |
| — Intermedia                       | **33.9** | 32.3          | 24.3           | 20.7        |
| — Difícil                         | **24.9** | 23.6          | 21.3           | 15.6        |
| Trueque — Total (n = 500, beta)    | 54.2           | 51.6          | **67.0** | 46.0        |

## Donde se queda atrás: la capacidad general

<p style='text-align: justify;'>
Ahora el otro lado. A diferencia del inglés, no hay un conjunto de benchmarks estándar y consensuado para LLMs en español, así que armé uno con tareas disponibles abiertamente que reflejan lo que se suele reportar para los modelos en inglés—y en estas, LatamGPT queda por debajo de los tres modelos de comparación. En MMLU-ProX (es)—el benchmark de razonamiento más limpio y completamente en español del conjunto—saca 45.8, contra 61.5 de Llama 3.1 Instruct y más de 80 de Gemma. En matemática escolar (MGSM) llega a 78.8 (vs. 88.8 de Llama Instruct); en razonamiento emocional (EQ-Bench), a 69.0 (vs. 75.4). La única tarea general donde quedan parejos es HeadQA, un examen médico tipo QA (51.2 vs. 50.4).
</p>

<p style='text-align: justify;'>
Las cifras exactas de los benchmarks generales:
</p>

| Benchmark general (español) | Shots | n      | Métrica | LatamGPT 70B | Llama 3.1 70B | Gemma 4 31B     | Qwen3.6 27B     |
| ---------------------------- | ----- | ------ | -------- | ------------ | ------------- | --------------- | --------------- |
| MMLU-ProX (es)               | 5     | 11,759 | EM       | 45.81        | 61.48         | **82.86** | 77.82           |
| MGSM native-CoT (es)         | 8     | 250    | EM       | 78.80        | 88.80         | **90.80** | 86.80           |
| EQ-Bench (es)                | 0     | 168    | EQ-Bench | 68.95        | 75.35         | **78.06** | 77.70           |
| HeadQA (es)                  | 0     | 2,742  | Acc      | 51.17        | 50.40         | 20.97           | **52.04** |

<p style='text-align: justify;'>
EM = exact match, Acc = accuracy; EQ-Bench usa su propia escala 0–100. En negrita, el mejor puntaje de cada fila. Los cuatro son modelos instruct; la columna "Llama 3.1 70B" es el checkpoint Instruct (ver la nota al final). El puntaje de Gemma en HeadQA queda de lado, como artefacto.
</p>

## ¿Por qué la brecha?

<p style='text-align: justify;'>
Entonces el modelo liberado conoce la región mejor que cualquiera de los otros, pero queda atrás en lo académico general. ¿Por qué? El continued pre-training es engañosamente difícil: uno empuja los pesos de un modelo ya terminado hacia una nueva distribución de datos, y el modelo no tiene ninguna obligación de conservar intactas sus habilidades anteriores mientras tanto. El modo de falla clásico es el <em>catastrophic forgetting</em>—un modelo que va perdiendo lo que sabía a medida que aprende algo nuevo—, un riesgo real y muy estudiado cada vez que uno sigue entrenando un modelo ya terminado, y algo que estudié de primera mano, en <a href="https://aclanthology.org/2024.findings-emnlp.38/">continual learning</a> y <a href="https://aclanthology.org/2021.emnlp-main.240/">continued pre-training</a>. Eso lo vuelve el principal sospechoso de la caída de LatamGPT en capacidad general, ya sea que se haya colado en la etapa de CPT, en la de SFT, o en ambas.
</p>

<p style='text-align: justify;'>
Aunque no lo puedo probar. Para atribuir la brecha al forgetting de forma limpia haría falta el checkpoint de solo-CPT—antes del instruction tuning—, y ese no se liberó, así que por ahora queda en hipótesis. Aun así, un detalle sugiere que la caída es, al menos en parte, real. El instruction tuning, si acaso, sube un poco los puntajes tipo MMLU en lugar de bajarlos: por ejemplo, <a href="https://huggingface.co/meta-llama/Llama-3.1-70B-Instruct">Llama 3.1 70B pasa de 79.3 (base) a 83.6 (instruct) en MMLU</a>. Así que el instruction tuning de LatamGPT debería haberle subido el puntaje, no bajárselo. Y sin embargo, en MMLU-ProX queda en 45.8—unos 16 puntos por debajo del 61.5 de Llama 3.1 Instruct, partiendo de la misma base. Una diferencia tan grande, y en sentido contrario a lo que hace el tuning, apunta de vuelta a la etapa de continued pre-training. Frente a los modelos más nuevos la historia es en parte otra: Gemma 4 y Qwen3.6 son como dos años más nuevos que la base Llama de 2024 de la que parte LatamGPT, así que parte de esa brecha es pura cuestión de época. El reporte técnico—e idealmente los pesos intermedios, los de solo-CPT—lo dejarían claro.
</p>

## Entonces, ¿cuál es la contribución real?

<p style='text-align: justify;'>
Si damos un paso atrás, las cifras tempranas plantean una pregunta que me parece más interesante que cualquier puntaje suelto: <strong>¿cuál es el aporte que perdura aquí—el modelo o los datos?</strong> Los pesos abiertos quedan obsoletos rápido—<a href="https://aiflashreport.com/model-releases.html">no dejan de salir modelos abiertos potentes</a>—, así avanza el campo. Pero el corpus regional y benchmarks como CHOCLO son el tipo de activo que no caduca. Algunas preguntas que vale la pena dejar dando vueltas, como comunidad: ¿la región necesita <em>entrenar</em> un modelo, o <em>curar</em> datos que puedan moldear a <em>cualquier</em> modelo? CHOCLO ya muestra dónde se concentra el valor: el conocimiento latinoamericano que codifica es justo donde LatamGPT saca ventaja. Esa señal puede rendir más como recurso portátil—usable vía <a href="https://arxiv.org/abs/2511.01649">retrieval-augmented generation</a>, ejemplos en contexto, un <a href="https://arxiv.org/abs/2402.10946">fine-tuning más económico</a>, o como una <a href="https://platform.claude.com/docs/en/agents-and-tools/agent-skills/overview">skill</a> de competencia cultural que un sistema de agentes pueda invocar—que congelada en un solo conjunto de pesos.
</p>

<p style='text-align: justify;'>
Nada de esto es un veredicto: es una lectura temprana, y el reporte oficial puede afinarla o desmentir partes. Lo que ya está claro es lo que logró LatamGPT: una colaboración grande y multinacional que juntó datos regionales con licencia a gran escala—incluyendo portugués y lenguas indígenas que los modelos comerciales suelen ignorar—, que construyó capacidad real de entrenamiento en la región y que produjo el modelo que mejor captura el conocimiento cultural latinoamericano hoy. Mucho de eso no aparece en ningún puntaje de benchmark, y sí que importa. Es un primer lanzamiento, una versión 1.0 con mucho margen para crecer, y los datos detrás son un bien público de verdad. Si los modelos son los motores, los datos definen el terreno—y lo más perdurable que puede hacer la región es dejar su terreno tan bien codificado que todo modelo, el de este año o el del próximo, no tenga más remedio que aprenderlo.
</p>

## Una nota sobre los números

<p style='text-align: justify;'>
Algunas aclaraciones. Todos los benchmarks están en español—tanto los prompts para generar las respuestas como los de evaluación. Los benchmarks académicos los corrí con <a href="https://github.com/EleutherAI/lm-evaluation-harness/">lm-evaluation-harness de EleutherAI</a>; CHOCLO y Trueque los evalué con <a href="https://github.com/confident-ai/deepeval">DeepEval</a>, usando un único juez externo (gpt-5.4-mini) aplicado igual a todos los modelos, así que esas columnas son comparables entre sí. La evaluación nativa de CHOCLO es híbrida (léxica, por embeddings y con un LLM como juez); yo reporto solo la parte del LLM como juez, como una decisión binaria de equivalente/no equivalente, y como los repos no documentan del todo cómo evalúan, conviene leer los puntajes absolutos de CHOCLO/Trueque como internamente consistentes más que como oficiales. Los cuatro son modelos instruct; la base de comparación de Llama es <code>Llama-3.1-70B-Instruct</code>. LatamGPT, en sí, es <code>Llama-3.1-70B</code> (base) + CPT + SFT; el checkpoint de solo-CPT no es público, así que comparo los modelos instruct ya liberados y no puedo aislar la etapa de CPT. Uso MMLU-ProX (es) como número de MMLU porque está completamente en español. Gemma 4 31B y Qwen3.6 27B corrieron con el reasoning ("thinking") desactivado. Cada resultado viene de una sola corrida, salvo Gemma 4 31B en HeadQA, que corrí tres veces—≈21 (por debajo del azar) cada vez, quizás un artefacto de evaluación.
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

<p style="text-align:center; color:#999; font-size:0.85em;"><em>Co-Authored-By: Claude</em></p>

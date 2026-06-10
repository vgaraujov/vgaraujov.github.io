---
permalink: /posts/2026/blog-latamgpt-pre-entrenamiento-continuo/
title: 'Conocimiento latinoamericano a un precio: una lectura de LatamGPT a través de sus benchmarks'
author_profile: true
published: false
---

<p style="text-align:center;"><em><a href="/posts/2026/blog-latamgpt-continued-pretraining/">Read in English</a></em></p>

<p style='text-align: justify;'>
En febrero de 2026, <a href="https://www.france24.com/en/live-news/20260210-latam-gpt-a-latin-american-ai-to-combat-us-centric-bias">LatamGPT fue presentado</a> en medio de un gran entusiasmo regional: el primer modelo de lenguaje grande y abierto construido <em>desde y para</em> América Latina y el Caribe, coordinado por el CENIA de Chile junto a más de sesenta instituciones de la región. La promesa era soberanía y relevancia cultural: pasar de consumir modelos extranjeros a producir los propios. Pero cuando los <a href="https://www.linkedin.com/posts/omar-u-florez_latamgpt-share-7467314002256257025-L45F/">pesos abiertos del modelo se liberaron el 1 de junio de 2026</a>, llegaron sin una sola cifra de benchmark. Como alguien que lleva años trabajando con modelos de lenguaje, esa ausencia me quedó dando vueltas. Así que decidí hacer la comparación yo mismo y plantear la pregunta que el lanzamiento se saltó: ¿qué compró realmente todo ese entrenamiento, y cuánto costó?
</p>

## Qué es realmente LatamGPT

<p style='text-align: justify;'>
El dato más importante para leer cualquiera de sus resultados es este: LatamGPT <strong>no</strong> se entrenó desde cero. Es <a href="https://huggingface.co/latam-gpt/Llama-3.1-70B-LatamGPT-SFT-1.0">Llama 3.1 70B de Meta</a>, adaptado mediante <em>pre-entrenamiento continuo</em> (CPT, por sus siglas en inglés) sobre unas <a href="https://aibusiness.com/generative-ai/the-new-open-source-ai-model-for-latin-america">230 mil millones de palabras</a> de texto regional con licencia —en español, portugués y lenguas indígenas— y luego ajustado por instrucciones (fine-tuning). Varias notas de prensa lo describieron como “construido enteramente en la región”, lo cual es cierto del <em>proceso</em> —la recolección de datos, el entrenamiento y el post-entrenamiento se hicieron localmente— pero no de los <em>pesos</em>, que parten del modelo de Meta. Esa distinción no es un tecnicismo: es justamente por eso que la vara justa para LatamGPT no es GPT-4 ni un ideal abstracto, sino el modelo base del que partió. Si el pre-entrenamiento continuo funciona, el modelo regional debería saber cosas que Llama 3.1 no sabe, sin perder lo que Llama 3.1 ya sabía.
</p>

## El costo oculto: el pre-entrenamiento continuo degrada lo que el modelo ya sabía

<p style='text-align: justify;'>
El pre-entrenamiento continuo es engañosamente difícil. Uno empuja los pesos de un modelo ya terminado hacia una nueva distribución de datos, y el modelo no tiene ninguna obligación de conservar intactas sus habilidades anteriores mientras lo hace. Esto es el <em>olvido catastrófico</em> (catastrophic forgetting), un dolor de cabeza central en el aprendizaje continuo desde hace años; yo mismo he trasteado con versiones de esto, desde seguir pre-entrenando un modelo tipo BERT mientras le agregaba nuevos objetivos (<a href="https://aclanthology.org/2021.emnlp-main.240/">EMNLP 2021</a>) hasta estudiar estrategias de adaptadores que permiten a los modelos aprender en secuencia sin colapsar (<a href="https://aclanthology.org/2024.findings-emnlp.38/">Findings of EMNLP 2024</a>). A escala de pre-entrenamiento, con un modelo de 70B y cientos de miles de millones de tokens nuevos, esa tensión no desaparece: se vuelve más cara de manejar.
</p>

<p style='text-align: justify;'>
Los números muestran el costo con claridad. En los benchmarks académicos generales <em>en español</em>, LatamGPT queda <strong>por debajo de su propio modelo base</strong> en casi todos los casos. En MMLU-ProX (es) —el benchmark de razonamiento más limpio y completamente en español del conjunto— obtiene 45.8 frente al 61.5 de Llama 3.1, una caída de casi dieciséis puntos. La matemática escolar (MGSM) baja de 88.8 a 78.8; el razonamiento emocional de EQ-Bench, de 75.4 a 69.0. La única tarea general donde se mantiene más o menos es HeadQA, un examen médico tipo QA (51.2 vs. 50.4). En otras palabras, la adaptación pensada para hacer el modelo <em>más</em> útil en la región lo dejó mediblemente <em>más débil</em> en razonamiento general que la base de la que partió. Es un costo real —aunque, en honor a la verdad, encabezar benchmarks académicos nunca fue la meta aquí; la relevancia cultural, la soberanía y el acceso en las lenguas de la región sí lo eran.
</p>

<p align="center">
    <img src="/images/latamgpt-benchmarks-es.png" width="760">
	<center>
	<figcaption>En los benchmarks académicos (izquierda), LatamGPT (rojo) queda por debajo de su propia base, Llama 3.1 70B (azul), y los dos modelos más nuevos y pequeños superan a ambos —salvo en HeadQA, donde apenas supera a su base (y el puntaje de Gemma parece un artefacto de evaluación)—. En los benchmarks de conocimiento latinoamericano (derecha) le gana a su base en CHOCLO y Trueque y encabeza CHOCLO de forma rotunda, aunque Gemma todavía gana el Trueque, más pequeño y revisado por humanos.</figcaption>
	</center>
</p>

<p style='text-align: justify;'>
Las cifras exactas detrás del lado izquierdo del gráfico:
</p>

| Benchmark general (español) | Shots | n | Métrica | LatamGPT 70B | Llama 3.1 70B | Gemma 4 31B | Qwen3.6 27B |
| --- | --- | --- | --- | --- | --- | --- | --- |
| MMLU-ProX (es) | 5 | 11,759 | EM | 45.81 | 61.48 | **82.86** | 77.82 |
| MGSM native-CoT (es) | 8 | 250 | EM | 78.80 | 88.80 | **90.80** | 86.80 |
| EQ-Bench (es) | 0 | 168 | EQ-Bench | 68.95 | 75.35 | **78.06** | 77.70 |
| HeadQA (es) | 0 | 2,742 | Acc | 51.17 | 50.40 | 20.97 | **52.04** |

<p style='text-align: justify;'>
EM = exact match (coincidencia exacta), Acc = accuracy (exactitud); EQ-Bench usa su propia escala 0–100. En negrita, el mejor puntaje de cada fila. LatamGPT queda por debajo de su base Llama 3.1 en todas las filas salvo HeadQA, y bastante por detrás de los dos modelos más nuevos y pequeños (dejando de lado el puntaje de Gemma en HeadQA; ver la nota al final).
</p>

## Dónde rinde frutos: el conocimiento latinoamericano

<p style='text-align: justify;'>
Ahora el otro lado de la balanza. CHOCLO —<a href="https://www.linkedin.com/posts/te-invitamos-a-nuestro-pr%C3%B3ximo-webinar-donde-ugcPost-7450985197631791105-XLKo/">otro lanzamiento del CENIA dentro del mismo esfuerzo de Latam-GPT</a>, disponible abiertamente <a href="https://huggingface.co/datasets/latam-gpt/CHOCLO">en Hugging Face</a>— es un benchmark de 104,847 preguntas sobre conocimiento cultural latinoamericano (comida, flora y fauna, geografía, tradiciones, figuras públicas) que abarca los países de la región. Lo evalúo con un juez LLM que decide si cada respuesta es semánticamente equivalente o no a la respuesta de referencia; el número es la proporción de respuestas juzgadas equivalentes (el detalle del juez y sus salvedades están en la nota al final). Aquí, LatamGPT es el <strong>mejor modelo</strong>: el 39.5% de sus respuestas se juzgan equivalentes, por encima de su base (37.6) y claramente por delante de los modelos más nuevos (Gemma 4 31B con 31.3, Qwen3.6 27B con 27.1), y esa ventaja se mantiene en todos los niveles de dificultad.
</p>

<p style='text-align: justify;'>
Este es el resultado que justifica el esfuerzo. El conocimiento cultural latinoamericano no surge de la escala ni de la novedad: Gemma y Qwen son más nuevos y potentes, y aun así pierden aquí, porque los datos sobre un plato chileno o una tradición peruana simplemente nunca estuvieron en su entrenamiento. No se puede razonar hasta un conocimiento que nunca se vio. El corpus regional metió ese conocimiento, y CHOCLO lo detecta.
</p>

<p style='text-align: justify;'>
Pero no gana <em>todas</em> las pruebas culturales. <a href="https://huggingface.co/datasets/latam-gpt/Trueque-Benchmark-beta-0.1">Trueque</a> —un benchmark de QA latinoamericano más pequeño, colaborativo y revisado por humanos (500 preguntas en esta beta, evaluado de la misma manera)— cuenta una historia más mixta. LatamGPT vuelve a superar a su propia base (54.2 vs. 51.6) y a Qwen (46.0), así que la ventaja que el corpus regional le da sobre Llama 3.1 se mantiene. Pero aquí Gemma 4 31B gana de forma rotunda, y no por poco: 67.0 contra 54.2 de LatamGPT. En los datos de cola larga generados automáticamente de CHOCLO, los datos regionales son decisivos; en las preguntas más amplias y revisadas por humanos de Trueque, un modelo general más potente y nuevo se impone a pesar de no haber sido entrenado nunca con la región.
</p>

<p style='text-align: justify;'>
Ambos benchmarks lado a lado, con CHOCLO desglosado por dificultad:
</p>

| Equivalencia semántica binaria (%) | LatamGPT 70B | Llama 3.1 70B | Gemma 4 31B | Qwen3.6 27B |
| --- | --- | --- | --- | --- |
| CHOCLO — General (n = 104,847) | **39.5** | 37.6 | 31.3 | 27.1 |
| — Fácil | **59.8** | 57.1 | 48.4 | 45.1 |
| — Intermedia | **33.9** | 32.3 | 24.3 | 20.7 |
| — Difícil | **24.9** | 23.6 | 21.3 | 15.6 |
| Trueque — General (n = 500, beta) | 54.2 | 51.6 | **67.0** | 46.0 |

<p style='text-align: justify;'>
El resumen honesto de todo esto: el pre-entrenamiento continuo le dio una ventaja consistente sobre el modelo base en conocimiento latinoamericano —y una victoria rotunda en CHOCLO— pero no la suficiente para superar a un modelo mucho más potente y nuevo en todos los benchmarks culturales, y la pagó con una regresión amplia en capacidad general.
</p>

## La comparación incómoda

<p style='text-align: justify;'>
Hay un hecho más duro escondido en el mismo gráfico. Gemma 4 31B y Qwen3.6 27B tienen <em>menos de la mitad del tamaño</em> de LatamGPT y son unos dos años más nuevos, y dominan los benchmarks académicos centrales, a menudo por veinte puntos o más. Este es el problema estructural de adaptar un modelo base: heredas su techo. El CPT sobre un Llama de 2024 no puede superar a las recetas de pre-entrenamiento de 2026, por muy buenos que sean tus datos regionales. Y el blanco se mueve: en 2026 la frontera de pesos abiertos lanza un modelo importante cada pocas semanas (<a href="https://huggingface.co/blog/daya-shankar/open-source-llms">un conteo registró cinco LLMs abiertos de primer nivel en un solo mes</a>). Para cuando terminas de adaptar la base del año pasado, el modelo más pequeño de este año ya te superó en los benchmarks académicos —y, como muestra Trueque, hasta puede llevarse algunos de los culturales—, dejando el conocimiento latinoamericano de cola larga de CHOCLO como su ventaja más clara y distintiva.
</p>

## Entonces, ¿cuál es la contribución real?

<p style='text-align: justify;'>
Aquí quiero ser un crítico amistoso, porque admiro genuinamente la ambición del proyecto y conozco a algunas de las personas detrás de él. Los resultados invitan a una pregunta estratégica más que a un veredicto: <strong>¿cuál es la contribución duradera aquí, el modelo o los datos?</strong> Los pesos quedarán superados en cuestión de meses; así es el ritmo del campo hoy. Pero el corpus regional y un benchmark como CHOCLO son el tipo de activo que <em>no</em> caduca. Lo que lleva a tres preguntas que vale la pena considerar:
</p>

<p style='text-align: justify;'>
<strong>¿Necesita América Latina <em>entrenar</em> un modelo, o <em>curar</em> datos?</strong> Un modelo base regional es una apuesta de varios millones de dólares que los laboratorios de frontera igualan en un día de cómputo. Datos culturales de alta calidad, con licencia abierta, son más baratos, duraderos y —crucialmente— utilizables por <em>cualquier</em> modelo que importe. <strong>¿Qué están aportando realmente los datos?</strong> CHOCLO ya lo muestra: el conocimiento latinoamericano que codifica es donde realmente se nota la ventaja de LatamGPT. Esa señal es más valiosa como recurso portátil que congelada en un único conjunto de pesos. <strong>¿Y está el verdadero punto de apalancamiento en el despliegue más que en el pre-entrenamiento?</strong> Ese mismo conocimiento curado puede inyectarse mediante <a href="https://arxiv.org/abs/2511.01649">generación aumentada por recuperación (RAG)</a>, ejemplos en contexto, o como una “<a href="https://platform.claude.com/docs/en/agents-and-tools/agent-skills/overview">habilidad</a>” de competencia cultural invocable por sistemas de agentes, a menudo superando al pre-entrenamiento por fuerza bruta en costo y vigencia, y mejorando todos los modelos en lugar de uno solo.
</p>

<p style='text-align: justify;'>
Nada de esto le quita mérito a lo que LatamGPT logró: una colaboración grande y multinacional que reunió datos regionales con licencia a escala —incluyendo portugués y lenguas indígenas que los modelos comerciales suelen ignorar—, construyó capacidad real de entrenamiento en la región, y produjo el modelo que mejor captura el conocimiento cultural latinoamericano hoy. Mucho de eso nunca aparece en un puntaje de benchmark, y sí importa. Es un primer paso genuino —y una versión 1.0, con mucho espacio para crecer—, y los datos detrás son un bien público real. Mi argumento es más acotado y, espero, constructivo: que el valor duradero del proyecto quizá viva menos en los 70B de pesos que en el corpus y la evaluación que creó. Si los modelos son los motores, los datos definen el terreno; y lo más estratégicamente duradero que puede hacer la región es asegurarse de que su terreno quede codificado lo suficientemente a fondo como para que todo modelo, el de este año o el del próximo, tenga que aprenderlo.
</p>

## Una nota sobre los números

<p style='text-align: justify;'>
Algunas salvedades. Todos los benchmarks están en español, tanto los prompts de generación de respuestas como los de evaluación. Los benchmarks académicos se corrieron con <a href="https://github.com/EleutherAI/lm-evaluation-harness/">lm-evaluation-harness de EleutherAI</a>; CHOCLO y Trueque se evaluaron con <a href="https://github.com/confident-ai/deepeval">DeepEval</a> usando un único juez externo (gpt-5.4-mini) aplicado de forma idéntica a cada modelo, así que esas columnas son comparables. La evaluación nativa de CHOCLO es híbrida (solapamiento léxico, similitud por embeddings y juez LLM); yo reporto solo la parte de juez LLM, como una decisión binaria equivalente/no equivalente —y como los repositorios no documentan del todo su metodología, conviene leer los puntajes absolutos de CHOCLO/Trueque como internamente consistentes más que oficiales—. Uso MMLU-ProX (es) como número de MMLU porque está completamente en español. Gemma 4 31B y Qwen3.6 27B se ejecutaron con el razonamiento (“thinking”) desactivado. Cada resultado proviene de una sola corrida, salvo Gemma 4 31B en HeadQA, que corrí tres veces —≈21 (por debajo del azar) cada vez, casi con certeza un artefacto de evaluación.
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

---
title: 'Local Knowledge at a Price: Reading LatamGPT Through Its Benchmarks'
date: 2026-06-08
permalink: /posts/2026/blog-latamgpt-continued-pretraining/
tags:
  - LatamGPT
  - continued pre-training
  - language models
  - benchmarks
  - Spanish NLP
---
<p style='text-align: justify;'>
In February 2026, <a href="https://www.france24.com/en/live-news/20260210-latam-gpt-a-latin-american-ai-to-combat-us-centric-bias">LatamGPT was presented</a> to a wave of regional enthusiasm: the first open large language model built <em>from and for</em> Latin America and the Caribbean, coordinated by Chile's CENIA and more than sixty institutions across the region. The promise was sovereignty and cultural relevance—moving Latin America from a consumer of foreign models to a producer of its own. When the model's <a href="https://www.linkedin.com/posts/omar-u-florez_latamgpt-share-7467314002256257025-L45F/?utm_source=social_share_send&utm_medium=member_desktop_web&rcm=ACoAAChwVRIBWACWa4EEAL8junUYPOgV6dVR1yI">open weights were released on June 1, 2026</a>, though, they arrived without a single benchmark number. As someone who has spent years working with language models, that absence nagged at me. So I decided to run the comparison myself, and to ask the question the release skipped: what did all that training actually buy, and what did it cost?
</p>

## What LatamGPT actually is

<p style='text-align: justify;'>
The single most important fact for reading any of its results is this: LatamGPT is <strong>not</strong> trained from scratch. It is <a href="https://huggingface.co/latam-gpt/Llama-3.1-70B-LatamGPT-SFT-1.0">Meta's Llama 3.1 70B</a>, adapted through <em>continued pre-training</em> (CPT) on roughly <a href="https://aibusiness.com/generative-ai/the-new-open-source-ai-model-for-latin-america">230 billion words</a> of permissioned, regionally-sourced text in Spanish, Portuguese and Indigenous languages, followed by supervised fine-tuning. Several news pieces described it as "built entirely in the region," which is true of the <em>pipeline</em>—data collection, training and post-training were all done locally—but not of the <em>weights</em>, which start life as Meta's model. That distinction is not a technicality. It is exactly why the fair yardstick for LatamGPT is not GPT-4 or some abstract ideal, but the base model it began from. If continued pre-training is working, the regional model should know things Llama 3.1 does not—without losing what Llama 3.1 already knew.
</p>

## The hidden cost: continued pre-training taxes what the model already knew

<p style='text-align: justify;'>
Continued pre-training is deceptively hard. You are nudging a finished model's weights toward a new data distribution, and the model has no obligation to keep its old abilities intact while you do. This is <em>catastrophic forgetting</em>, and it has been a central headache in continual learning for years—I've poked at versions of it myself, from keeping a BERT-style model pre-training while bolting on new objectives (<a href="https://aclanthology.org/2021.emnlp-main.240/">EMNLP 2021</a>) to studying adapter strategies that let models learn in sequence without collapsing (<a href="https://aclanthology.org/2024.findings-emnlp.38/">Findings of EMNLP 2024</a>). At pre-training scale, with a 70B model and hundreds of billions of new tokens, that tension does not disappear—it gets more expensive to manage.
</p>

<p style='text-align: justify;'>
The numbers show the tax plainly. On general academic benchmarks <em>in Spanish</em>, LatamGPT lands <strong>below its own base model</strong> almost across the board. On MMLU-ProX (es)—the cleanest, fully-Spanish reasoning benchmark in the set—it scores 45.8 against Llama 3.1's 61.5, a drop of nearly sixteen points. Grade-school math (MGSM) falls from 88.8 to 78.8; emotional-reasoning EQ-Bench from 75.4 to 69.0. The only general task where it roughly holds is HeadQA, a medical-exam QA set (51.2 vs. 50.4). In other words, the adaptation that was supposed to make the model <em>more</em> useful in the region made it measurably <em>worse</em> at general reasoning than the model it was carved from.
</p>

<p align="center">
    <img src="/images/latamgpt-benchmarks.png" width="760">
	<center>
	<figcaption>On the academic benchmarks (left), LatamGPT (red) trails its own base, Llama 3.1 70B (blue), and the two newer, smaller models lead both—except on HeadQA, where it edges its base (and Gemma's score looks like an evaluation artifact). On CHOCLO (right), the benchmark it was built for, the picture flips.</figcaption>
	</center>
</p>

<p style='text-align: justify;'>
The exact numbers behind the left side of that chart:
</p>

| General benchmark (Spanish) | Shots | n      | Metric   | LatamGPT 70B | Llama 3.1 70B | Gemma 4 31B     | Qwen3.6 27B     |
| --------------------------- | ----- | ------ | -------- | ------------ | ------------- | --------------- | --------------- |
| MMLU-ProX (es)              | 5     | 11,759 | EM       | 45.81        | 61.48         | **82.86** | 77.82           |
| MGSM native-CoT (es)        | 8     | 250    | EM       | 78.80        | 88.80         | **90.80** | 86.80           |
| EQ-Bench (es)               | 0     | 168    | EQ-Bench | 68.95        | 75.35         | **78.06** | 77.70           |
| HeadQA (es)                 | 0     | 2,742  | Acc      | 51.17        | 50.40         | 20.97           | **52.04** |

<p style='text-align: justify;'>
EM = exact match, Acc = accuracy; EQ-Bench uses its own 0–100 scale. Bold marks the best score per row. LatamGPT lands below its Llama 3.1 base on every row except HeadQA—and well behind the two newer, smaller models (Gemma's HeadQA score aside; see the note at the end).
</p>

## Where it pays off: local knowledge

<p style='text-align: justify;'>
Now the other side of the ledger. CHOCLO—<a href="https://www.linkedin.com/posts/andres-carvallo-b5836a33a_latam-gptchoclo-datasets-at-hugging-face-share-7449581534673633280-elg4/">another CENIA release from the same Latam-GPT effort</a>, openly available <a href="https://huggingface.co/latam-gpt">on Hugging Face</a>—is a 104,847-question benchmark of Latin American cultural knowledge (food, flora and fauna, geography, traditions, public figures) spanning the region's countries. CHOCLO ships a hybrid evaluation (lexical overlap, embedding similarity, and an LLM-as-judge); I report the LLM-as-judge part as a binary call—a single external judge decides, for each answer, whether it is semantically equivalent to the reference, and the score is simply the share judged equivalent. Because the same judge grades all four models, the numbers are directly comparable. Here, LatamGPT is the <strong>best model in the table</strong>: 39.5% of its answers are judged equivalent to the gold reference, edging out its base (37.6) and clearly ahead of the newer models (Gemma 4 31B at 31.3, Qwen3.6 27B at 27.1). And the lead is consistent across difficulty levels:
</p>

| CHOCLO — binary semantic equivalence (%) | LatamGPT 70B   | Llama 3.1 70B | Gemma 4 31B | Qwen3.6 27B |
| ------------------------------ | -------------- | ------------- | ----------- | ----------- |
| Overall                        | **39.5** | 37.6          | 31.3        | 27.1        |
| Fácil (easy)                  | **59.8** | 57.1          | 48.4        | 45.1        |
| Intermedia (intermediate)      | **33.9** | 32.3          | 24.3        | 20.7        |
| Difícil (hard)                | **24.9** | 23.6          | 21.3        | 15.6        |

<p style='text-align: justify;'>
This is the result that justifies the effort. Local cultural knowledge does not fall out of scale or recency—Gemma and Qwen are newer and strong, and they still lose here, because the relevant facts about a Chilean dish or a Peruvian tradition were simply never in their training data. You cannot reason your way to knowledge you were never shown. The regional corpus put that knowledge in, and CHOCLO can see it. The honest summary of the two tables together: continued pre-training bought a real, measurable gain on exactly the thing it was built for—and paid for it with a broad regression everywhere else.
</p>

## The uncomfortable comparison

<p style='text-align: justify;'>
There is a harder fact lurking in the same chart. Gemma 4 31B and Qwen3.6 27B are <em>less than half the size</em> of LatamGPT and roughly two years newer, and they dominate the core academic benchmarks—often by twenty points or more. This is the structural problem with adapting a base model: you inherit its ceiling. CPT on a 2024-era Llama cannot out-run pre-training recipes from 2026, no matter how good your regional data is. And the target keeps moving: in 2026 the open-weight frontier ships a major model every few weeks (<a href="https://huggingface.co/blog/daya-shankar/open-source-llms">one tally counted five frontier-class open-weight LLMs in a single month</a>). By the time you finish adapting last year's base, this year's smaller model has lapped you on everything except the local knowledge you added by hand.
</p>

## So what is the real contribution?

<p style='text-align: justify;'>
This is where I want to be a friendly critic, because I genuinely admire the ambition and know some of the people behind it. The results invite a strategic question rather than a verdict: <strong>what is the durable contribution here—the model, or the data?</strong> The weights will be superseded within months; that is just the cadence of the field now. But the regional corpus and a benchmark like CHOCLO are the kind of asset that does <em>not</em> expire. Which leads to three questions worth sitting with:
</p>

<p style='text-align: justify;'>
<strong>Does Latin America need to <em>train</em> a model, or to <em>curate</em> data?</strong> A regional foundation model is a multi-million-dollar bet that frontier labs match in a day of compute. High-quality, openly licensed cultural data is cheaper, durable, and—crucially—usable by <em>any</em> model that matters. <strong>What is the data actually contributing?</strong> CHOCLO already shows it: the local knowledge it encodes is the one place LatamGPT wins. That signal is more valuable as a portable resource than as something frozen into one set of weights. <strong>And is the real leverage in deployment rather than pre-training?</strong> The same curated knowledge can be injected through retrieval-augmented generation, in-context examples, or as a callable "cultural competence" skill for agentic systems—often beating brute-force pre-training on cost and freshness, and improving every model instead of just one.
</p>

<p style='text-align: justify;'>
None of this diminishes what LatamGPT accomplished: a large, multi-country collaboration that assembled permissioned regional data at scale and produced the model that best captures Latin American cultural knowledge today. That is a genuine first, and the data behind it is a real public good. My argument is narrower and, I hope, constructive—that the project's lasting value may live less in the 70B weights than in the corpus and evaluation it created. If models are the engines, data defines the terrain. The most strategically durable thing Latin America can do is make sure its terrain is deeply encoded—so that every model, regional or frontier, this year's or next year's, has no choice but to learn it.
</p>

## A note on the numbers

<p style='text-align: justify;'>
All numbers come from two open evaluation frameworks: the Spanish academic benchmarks were run with EleutherAI's <a href="https://github.com/EleutherAI/lm-evaluation-harness/">lm-evaluation-harness</a>, and CHOCLO's LLM-as-judge scoring with <a href="https://github.com/confident-ai/deepeval">DeepEval</a>. I use MMLU-ProX (es) as the MMLU number because it is fully Spanish. HeadQA is included for completeness, but Gemma 4 31B's score there (≈21, below chance) is almost certainly an evaluation artifact rather than a real capability gap. CHOCLO is scored by one external judge (gpt-5.4-mini) applied identically to all four models, so the four columns are directly comparable.
</p>

## References

- LatamGPT model card: [latam-gpt/Llama-3.1-70B-LatamGPT-SFT-1.0](https://huggingface.co/latam-gpt/Llama-3.1-70B-LatamGPT-SFT-1.0) and the [Latam-GPT organization](https://huggingface.co/latam-gpt) on Hugging Face.
- Omar U. Flórez (2026). [*LatamGPT open-weights release announcement (June 1, 2026)*](https://www.linkedin.com/posts/omar-u-florez_latamgpt-share-7467314002256257025-L45F/).
- Andrés Carvallo (2026). [*Latam-GPT / CHOCLO datasets on Hugging Face*](https://www.linkedin.com/posts/andres-carvallo-b5836a33a_latam-gptchoclo-datasets-at-hugging-face-share-7449581534673633280-elg4/).
- France24 (2026). [*Latam-GPT: a Latin American AI to combat US-centric bias*](https://www.france24.com/en/live-news/20260210-latam-gpt-a-latin-american-ai-to-combat-us-centric-bias).
- AI Business (2026). [*Meet Latam-GPT, the New Open Source AI Model for Latin America*](https://aibusiness.com/generative-ai/the-new-open-source-ai-model-for-latin-america) (source for the ~230 billion words / 300 billion tokens figure, attributed to CENIA).
- Brookings (2026). [*Latam-GPT and the search for AI sovereignty*](https://www.brookings.edu/articles/latam-gpt-and-the-search-for-ai-sovereignty/).
- Rest of World (2025). [*Latin America is building LatamGPT to rival ChatGPT*](https://restofworld.org/2025/chatgpt-latin-america-alternative-latamgpt/).
- Vladimir Araujo, Andrés Villa, Marcelo Mendoza, Marie-Francine Moens, and Alvaro Soto. 2021. [*Augmenting BERT-style Models with Predictive Coding to Improve Discourse-level Representations*](https://aclanthology.org/2021.emnlp-main.240/). EMNLP 2021.
- Vladimir Araujo, Marie-Francine Moens, and Tinne Tuytelaars. 2024. [*Learning to Route for Dynamic Adapter Composition in Continual Learning with Language Models*](https://aclanthology.org/2024.findings-emnlp.38/). Findings of EMNLP 2024.
- EleutherAI. [*lm-evaluation-harness*](https://github.com/EleutherAI/lm-evaluation-harness/) (academic benchmark evaluation).
- Confident AI. [*DeepEval*](https://github.com/confident-ai/deepeval) (LLM-as-judge evaluation, used for CHOCLO).

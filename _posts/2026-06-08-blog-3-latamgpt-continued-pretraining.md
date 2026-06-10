---
title: 'Latin American Knowledge at a Price: Reading LatamGPT Through Its Benchmarks'
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
The numbers show the tax plainly. On general academic benchmarks <em>in Spanish</em>, LatamGPT lands <strong>below its own base model</strong> almost across the board. On MMLU-ProX (es)—the cleanest, fully-Spanish reasoning benchmark in the set—it scores 45.8 against Llama 3.1's 61.5, a drop of nearly sixteen points. Grade-school math (MGSM) falls from 88.8 to 78.8; emotional-reasoning EQ-Bench from 75.4 to 69.0. The only general task where it roughly holds is HeadQA, a medical-exam QA set (51.2 vs. 50.4). In other words, the adaptation meant to make the model <em>more</em> useful in the region left it measurably <em>weaker</em> at general reasoning than the base it was built from. That is a real cost—though, in fairness, topping academic benchmarks was never the goal here; cultural relevance, sovereignty, and access in the region's languages were.
</p>

<p align="center">
    <img src="/images/latamgpt-benchmarks.png" width="760">
	<center>
	<figcaption>On the academic benchmarks (left), LatamGPT (red) trails its own base, Llama 3.1 70B (blue), and the two newer, smaller models lead both—except on HeadQA, where it edges its base (and Gemma's score looks like an evaluation artifact). On the Latin American knowledge benchmarks (right), it beats its base on both CHOCLO and Trueque and tops CHOCLO outright—though Gemma still wins the smaller, human-reviewed Trueque.</figcaption>
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

## Where it pays off: Latin American knowledge

<p style='text-align: justify;'>
Now the other side of the ledger. CHOCLO—<a href="https://www.linkedin.com/posts/te-invitamos-a-nuestro-pr%C3%B3ximo-webinar-donde-ugcPost-7450985197631791105-XLKo/">another CENIA release from the same Latam-GPT effort</a>, openly available <a href="https://huggingface.co/datasets/latam-gpt/CHOCLO">on Hugging Face</a>—is a 104,847-question benchmark of Latin American cultural knowledge (food, flora and fauna, geography, traditions, public figures) spanning the region's countries. I score it with an LLM judge that rules each answer equivalent to the reference answer or not; the number is the share judged equivalent (the judging setup and its caveats are in the note at the end). Here, LatamGPT is the <strong>best model</strong>: 39.5% of its answers are judged equivalent, edging out its base (37.6) and clearly ahead of the newer models (Gemma 4 31B at 31.3, Qwen3.6 27B at 27.1)—and that lead holds at every difficulty level.
</p>

<p style='text-align: justify;'>
This is the result that justifies the effort. Latin American cultural knowledge does not fall out of scale or recency—Gemma and Qwen are newer and strong, and they still lose here, because the relevant facts about a Chilean dish or a Peruvian tradition were simply never in their training data. You cannot reason your way to knowledge you were never shown. The regional corpus put that knowledge in, and CHOCLO can see it.
</p>

<p style='text-align: justify;'>
But it does not win <em>every</em> cultural test. <a href="https://huggingface.co/datasets/latam-gpt/Trueque-Benchmark-beta-0.1">Trueque</a>—a smaller, human-reviewed collaborative Latin American QA benchmark (500 questions in this beta, scored the same way)—tells a more mixed story. LatamGPT again beats its own base (54.2 vs. 51.6) and Qwen (46.0), so the edge the regional corpus gives it over Llama 3.1 holds. But here Gemma 4 31B wins outright, and not by a little: 67.0 to LatamGPT's 54.2. On CHOCLO's long-tail, machine-generated facts the regional data is decisive; on Trueque's broader, human-reviewed questions, a stronger and newer general model pulls ahead despite never being trained on the region.
</p>

<p style='text-align: justify;'>
Both benchmarks side by side, with CHOCLO broken out by difficulty:
</p>

| Binary semantic equivalence (%) | LatamGPT 70B | Llama 3.1 70B | Gemma 4 31B | Qwen3.6 27B |
| --- | --- | --- | --- | --- |
| CHOCLO — Overall (n = 104,847) | **39.5** | 37.6 | 31.3 | 27.1 |
| — Fácil (easy) | **59.8** | 57.1 | 48.4 | 45.1 |
| — Intermedia | **33.9** | 32.3 | 24.3 | 20.7 |
| — Difícil (hard) | **24.9** | 23.6 | 21.3 | 15.6 |
| Trueque — Overall (n = 500, beta) | 54.2 | 51.6 | **67.0** | 46.0 |

<p style='text-align: justify;'>
The honest summary of all of it: continued pre-training bought a consistent edge over the base model on Latin American knowledge—and an outright win on CHOCLO—but not enough to beat a much stronger, newer model on every cultural benchmark, and it paid for that edge with a broad regression on general ability.
</p>

## The uncomfortable comparison

<p style='text-align: justify;'>
There is a harder fact lurking in the same chart. Gemma 4 31B and Qwen3.6 27B are <em>less than half the size</em> of LatamGPT and roughly two years newer, and they dominate the core academic benchmarks—often by twenty points or more. This is the structural problem with adapting a base model: you inherit its ceiling. CPT on a 2024-era Llama cannot out-run pre-training recipes from 2026, no matter how good your regional data is. And the target keeps moving: in 2026 the open-weight frontier ships a major model every few weeks (<a href="https://huggingface.co/blog/daya-shankar/open-source-llms">one tally counted five frontier-class open-weight LLMs in a single month</a>). By the time you finish adapting last year's base, this year's smaller model has lapped you on the academic benchmarks—and, as Trueque shows, can even take some of the cultural ones—leaving the long-tail Latin American knowledge of CHOCLO as its clearest and most distinctive advantage.
</p>

## So what is the real contribution?

<p style='text-align: justify;'>
This is where I want to be a friendly critic, because I genuinely admire the ambition and know some of the people behind it. The results invite a strategic question rather than a verdict: <strong>what is the durable contribution here—the model, or the data?</strong> The weights will be superseded within months; that is just the cadence of the field now. But the regional corpus and a benchmark like CHOCLO are the kind of asset that does <em>not</em> expire. Which leads to three questions worth sitting with:
</p>

<p style='text-align: justify;'>
<strong>Does Latin America need to <em>train</em> a model, or to <em>curate</em> data?</strong> A regional foundation model is a multi-million-dollar bet that frontier labs match in a day of compute. High-quality, openly licensed cultural data is cheaper, durable, and—crucially—usable by <em>any</em> model that matters. <strong>What is the data actually contributing?</strong> CHOCLO already shows it: the Latin American knowledge it encodes is where LatamGPT's edge actually shows. That signal is more valuable as a portable resource than as something frozen into one set of weights. <strong>And is the real leverage in deployment rather than pre-training?</strong> The same curated knowledge can be injected through retrieval-augmented generation, in-context examples, or as a callable "cultural competence" skill for agentic systems—often beating brute-force pre-training on cost and freshness, and improving every model instead of just one.
</p>

<p style='text-align: justify;'>
None of this diminishes what LatamGPT accomplished: a large, multi-country collaboration that assembled permissioned regional data at scale—including Portuguese and Indigenous languages that commercial models largely ignore—built real training capacity in the region, and produced the model that best captures Latin American cultural knowledge today. Much of that never shows up in a benchmark score, and it matters. That is a genuine first—and a version 1.0, with plenty of room to grow—and the data behind it is a real public good. My argument is narrower and, I hope, constructive—that the project's lasting value may live less in the 70B weights than in the corpus and evaluation it created. If models are the engines, data defines the terrain. The most strategically durable thing Latin America can do is make sure its terrain is deeply encoded—so that every model, regional or frontier, this year's or next year's, has no choice but to learn it.
</p>

## A note on the numbers

<p style='text-align: justify;'>
A few caveats. Every benchmark is in Spanish—both the answer-generation and the judging prompts. Academic benchmarks ran on EleutherAI's <a href="https://github.com/EleutherAI/lm-evaluation-harness/">lm-evaluation-harness</a>; CHOCLO and Trueque were scored with <a href="https://github.com/confident-ai/deepeval">DeepEval</a> using one external judge (gpt-5.4-mini) applied identically to every model, so those columns are comparable. CHOCLO's native evaluation is a hybrid (lexical, embedding, LLM-as-judge); I report only the LLM-as-judge part, as a binary equivalent/not call—and since the repos don't fully document their judging, treat the absolute CHOCLO/Trueque scores as internally consistent rather than official. MMLU-ProX (es) is my MMLU number because it's fully Spanish. Gemma 4 31B and Qwen3.6 27B ran with reasoning ("thinking") off. Every result is a single run, except Gemma 4 31B on HeadQA, which I ran three times—≈21 (below chance) each time, almost certainly an evaluation artifact.
</p>

## References

- LatamGPT model card: [latam-gpt/Llama-3.1-70B-LatamGPT-SFT-1.0](https://huggingface.co/latam-gpt/Llama-3.1-70B-LatamGPT-SFT-1.0) and the [Latam-GPT organization](https://huggingface.co/latam-gpt) on Hugging Face.
- CHOCLO dataset: [latam-gpt/CHOCLO](https://huggingface.co/datasets/latam-gpt/CHOCLO).
- Trueque dataset: [latam-gpt/Trueque-Benchmark-beta-0.1](https://huggingface.co/datasets/latam-gpt/Trueque-Benchmark-beta-0.1).
- Omar U. Flórez (2026). [*LatamGPT open-weights release announcement (June 1, 2026)*](https://www.linkedin.com/posts/omar-u-florez_latamgpt-share-7467314002256257025-L45F/).
- CENIA (2026). [*CHOCLO and Trueque benchmarks — release announcement*](https://www.linkedin.com/posts/te-invitamos-a-nuestro-pr%C3%B3ximo-webinar-donde-ugcPost-7450985197631791105-XLKo/) (LinkedIn).
- France24 (2026). [*Latam-GPT: a Latin American AI to combat US-centric bias*](https://www.france24.com/en/live-news/20260210-latam-gpt-a-latin-american-ai-to-combat-us-centric-bias).
- AI Business (2026). [*Meet Latam-GPT, the New Open Source AI Model for Latin America*](https://aibusiness.com/generative-ai/the-new-open-source-ai-model-for-latin-america) (source for the ~230 billion words / 300 billion tokens figure, attributed to CENIA).
- Brookings (2026). [*Latam-GPT and the search for AI sovereignty*](https://www.brookings.edu/articles/latam-gpt-and-the-search-for-ai-sovereignty/).
- Rest of World (2025). [*Latin America is building LatamGPT to rival ChatGPT*](https://restofworld.org/2025/chatgpt-latin-america-alternative-latamgpt/).
- Vladimir Araujo, Andrés Villa, Marcelo Mendoza, Marie-Francine Moens, and Alvaro Soto. 2021. [*Augmenting BERT-style Models with Predictive Coding to Improve Discourse-level Representations*](https://aclanthology.org/2021.emnlp-main.240/). EMNLP 2021.
- Vladimir Araujo, Marie-Francine Moens, and Tinne Tuytelaars. 2024. [*Learning to Route for Dynamic Adapter Composition in Continual Learning with Language Models*](https://aclanthology.org/2024.findings-emnlp.38/). Findings of EMNLP 2024.
- EleutherAI. [*lm-evaluation-harness*](https://github.com/EleutherAI/lm-evaluation-harness/) (academic benchmark evaluation).
- Confident AI. [*DeepEval*](https://github.com/confident-ai/deepeval) (LLM-as-judge evaluation, used for CHOCLO).

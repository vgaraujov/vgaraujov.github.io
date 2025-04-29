---
title: 'Democratizing Spanish NLP: Language Models and Benchmarks'
date: 2024-05-25
permalink: /posts/2024/blog-spanish-nlp-democratization/
tags:
  - Spanish NLP
  - language models
  - benchmarks
  - seq2seq
---

<p style='text-align: justify;'>
In recent years, the field of Natural Language Processing (NLP) has witnessed significant advancements, particularly in the development of pre-trained language models. While English has been at the forefront of these developments, there has been a growing need to extend these advancements to other languages, including Spanish. Addressing this gap, our research focuses on creating and evaluating Spanish language models and resources that are not only effective but also accessible to a broader community. This blog post delves into our efforts in developing lightweight models, establishing evaluation benchmarks, and introducing sequence-to-sequence models tailored for the Spanish language.
</p>

## Lightweight Spanish Language Models: ALBETO and DistilBETO

<p style='text-align: justify;'>
To make Spanish language models more accessible and efficient, we introduced ALBETO and DistilBETO—lightweight versions inspired by ALBERT and DistilBERT architectures, respectively. These models were pre-trained exclusively on Spanish corpora, with ALBETO variants ranging from 5 million to 223 million parameters and DistilBETO comprising 67 million parameters.
</p>

<p align="center"> 
    <img src="https://miro.medium.com/v2/resize:fit:1023/1*e5G4Vdt6gSFkRjs3fD36Pg.png" width="400">
	<center>
	<figcaption>DistilBETO follows the same methodology as the DistilBERT model.</figcaption>
	</center>
</p>

<p style='text-align: justify;'>
Despite their reduced sizes, our models achieved competitive results compared to BETO on the GLUES benchmark, which encompasses diverse natural language understanding tasks in Spanish. Notably, the larger ALBETO model outperformed other models on datasets such as MLDoc, PAWS-X, XNLI, MLQA, SQAC, and XQuAD. For instance, ALBETO xxlarge achieved an F1 score of 81.34 on GLUES, closely matching BETO cased's 81.02, while being approximately half its size. Additionally, ALBETO base retained 97% of BETO's performance with over nine times fewer parameters. All our models are publicly available to facilitate further research and application development.
</p>

## Evaluating Spanish Sentence Representations

<p style='text-align: justify;'>
The proliferation of Spanish language models necessitated the development of standardized benchmarks to evaluate their performance effectively. In response, we introduced two evaluation suites: Spanish SentEval and Spanish DiscoEval. Spanish SentEval assesses stand-alone sentence representations across tasks such as sentiment analysis, paraphrase detection, and natural language inference. Spanish DiscoEval focuses on discourse-aware representations, evaluating tasks like coherence and discourse relation classification.
</p>

<p style='text-align: justify;'>
These benchmarks incorporate both existing and newly constructed datasets, providing a comprehensive framework for assessing the capabilities of Spanish language models. Our evaluations revealed that models like ALBETO and DistilBETO performed robustly across various tasks, highlighting their effectiveness in capturing both sentence-level and discourse-level semantics. For example, ALBETO xxlarge achieved an average F1 score of 81.34 across tasks, closely matching BETO cased's 81.02, while being approximately half its size.
</p>

## Advancing Generative Capabilities with Seq2Seq Models

<p style='text-align: justify;'>
While encoder-only models like BETO and ALBETO have excelled in understanding tasks, the demand for generative applications—such as summarization, translation, and question answering—has highlighted the need for sequence-to-sequence (seq2seq) architectures in Spanish NLP. To bridge this gap, we developed BARTO and T5S, Spanish adaptations of the BART and T5 models, respectively. These models were pre-trained exclusively on Spanish corpora, encompassing approximately 28 million documents (~120 GB of text).
</p>

<p align="center"> 
    <img src="https://cdn.analyticsvidhya.com/wp-content/uploads/2024/11/bart.webp" width="600">
	<center>
	<figcaption>BARTO and T5S follow the original BART and T5 Version 1.1 recipes for pre-training.</figcaption>
	</center>
</p>

<p style='text-align: justify;'>
BARTO and T5S have demonstrated strong performance in generative tasks, offering valuable resources for applications requiring text generation in Spanish. Notably, these models outperformed BERT2BERT-style models and their multilingual counterparts in many generative tasks, especially with lengthy sequences. For instance, BARTO achieved a BLEU score of 35.2 on the Spanish summarization task, surpassing the previous state-of-the-art by 2.5 points.
</p>

## Benchmarking Seq2Seq Models: A New Evaluation Suite

<p style='text-align: justify;'>
Recognizing the need for standardized evaluation of seq2seq models in Spanish, we introduced a comprehensive benchmark suite tailored for these architectures. This suite encompasses a variety of tasks, including summarization, question answering, split-and-rephrase, dialogue, and machine translation. Our evaluations demonstrated that BARTO and T5S not only excelled in these tasks but also provided competitive performance in discriminative tasks such as sequence and token classification. For example, T5S achieved an accuracy of 89.7% on the Spanish QA task, outperforming the previous best model by 3.2%.
</p>

<p style='text-align: justify;'>
By establishing this benchmark, we aim to provide a standardized framework for assessing the performance of Spanish seq2seq models, facilitating further research and development in this area.
</p>

## Final Remarks

<p style='text-align: justify;'>
All our models and evaluation benchmarks are publicly accessible, aiming to democratize access to advanced NLP tools for the Spanish language. We encourage researchers and practitioners to utilize and build upon these resources to further advance Spanish NLP.
</p>

## References

- José Cañete, Sebastián Donoso, Felipe Bravo-Marquez, Andrés Carvallo, and Vladimir Araujo. 2022. ALBETO and DistilBETO: Lightweight Spanish Language Models. In *Proceedings of the Thirteenth Language Resources and Evaluation Conference*, pages 4291–4298, Marseille, France. European Language Resources Association. [Link](https://aclanthology.org/2022.lrec-1.457/)

- Vladimir Araujo, Andrés Carvallo, Souvik Kundu, José Cañete, Marcelo Mendoza, Robert E. Mercer, Felipe Bravo-Marquez, Marie-Francine Moens, and Alvaro Soto. 2022. Evaluation Benchmarks for Spanish Sentence Representations. In *Proceedings of the Thirteenth Language Resources and Evaluation Conference*, pages 6024–6034, Marseille, France. European Language Resources Association. [Link](https://aclanthology.org/2022.lrec-1.648/)

- Vladimir Araujo, Maria Mihaela Trusca, Rodrigo Tufiño, and Marie-Francine Moens. 2024. Sequence-to-Sequence Spanish Pre-trained Language Models. In *Proceedings of the 2024 Joint International Conference on Computational Linguistics, Language Resources and Evaluation (LREC-COLING 2024)*, pages 14729–14743, Torino, Italia. ELRA and ICCL. [Link](https://aclanthology.org/2024.lrec-main.1283/)

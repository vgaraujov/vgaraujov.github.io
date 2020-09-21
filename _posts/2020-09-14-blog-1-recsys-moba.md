---
title: 'Recommendation Systems for Item Recommendation in MOBA Games'
date: 2020-09-21
permalink: /posts/2020/blog-1-recsys-moba/
tags:
  - item recommendation
  - recommendation system
  - deep learning
  - data mining
---

<p style='text-align: justify;'>
The video game industry has adopted recommendation systems to boost users' interest with a focus on game sales. Other exciting applications within video games are those that help the player to make decisions that would maximize their gaming experience. In this blog, I am going to present to you a research focused on the second application that resulted in two papers presented in RecSys.
</p>

## MOBA Games and Recommendation Problem
<p style='text-align: justify;'>
The annual <a href="https://strivesponsorship.com/wp-content/uploads/2020/03/Global-Esports-Market-Report-2020.pdf">report of Newzoo</a> shows that global e-sports revenues and its audience will grow to $1.1 billion and 495
million people in 2020, respectively. MOBA is one of the most significant social gaming genres contributing to
that growth. An example of this phenomenon is the League of Legends World Championship, which was the biggest
tournament of 2019 with more than 105 million hours live on Twitch and YouTube. This type of games can rise up to 64
million active players per month worldwide, with over a billion monthly gaming hours, affirms <a href="https://www.forbes.com/sites/insertcoin/2014/01/27/riots-league-of-legends-reveals-astonishing-27-million-daily-players-67-million-monthly/#1461159b6d39">Forbes</a>.
</p>
<p style='text-align: justify;'>
League of Legends (LoL) has dominated the market since 2012 and is considered one of the most popular electronic games. The game consists of two teams (red and blue) of five players each, that compete to be the first to destroy the enemy base. Each player controls one character (champion) that interacts with the rest through combats, which are carried out in a particular arena.
</p>
<p align="center"> 
    <img src="https://upload.wikimedia.org/wikipedia/commons/d/dc/Map_of_MOBA.svg" width="300">
	<center>
	<figcaption>Typical MOBA map. Circles represent turrets. The green area is the jungle and the yellow area are the lanes. The corner areas are the team's base.</figcaption>
	</center>
</p>
<p style='text-align: justify;'>
The pace of the game is encouraged by an in-game currency reward system, which is used to buy items that increase the statistics and performance of the champion. This is one of the main ways for the players to increase their attack and defense power, thereby increasing their contribution to winning the game.
</p>
<p style='text-align: justify;'>
This game presents at least two recommendation problems: champion <a href="https://dl.acm.org/doi/10.1145/3240323.3240345">(Chen et al., 2018)</a> and item recommendation <a href="https://ieeexplore.ieee.org/document/8395021/">(Looi et al., 2018)</a>, because there are more than 140 characters and around 240 items. Both the choice of champions and the items pose the challenge of the number of possible combinations, which users face making decisions based on experience. In this work, we focused on the latter.
</p>
<p align="center"> 
    <img src="/images/recsys-moba/moba_recsys_problem.png" width="250">
	<center>
	<figcaption>Example of two teams matchup and item recommendations for the Blue team.</figcaption>
	</center>
</p>



## Data Mining for Item Recommendation in MOBA Games

### Framework

In order to tackle this task, we proposed a framework based on classical data mining methodology, including slight changes to use it for the recommendation task. This methodology allows generating recommendations using knowledge learned from the actions carried out by the users in previous matches.

<p align="center"> 
    <img src="/images/recsys-moba/framework.png" width="650">
	<center>
	<figcaption>Example of two teams matchup and item recommendations for the Blue team.</figcaption>
	</center>
</p>

The first step consists on preprocessing the dataset. We used a dataset provided by <a href="https://www.kaggle.com/paololol/league-of-legends-ranked-matches">Kaggle</a>. Because the purpose is a recommendation for a specifc champion, we decided to reorganize the data. The final dataset has transactional structure, where each instance includes the identifers of a selected champion during a match, the items used, and the enemies involved.

The second step is to apply techniques of data mining, both descriptive and predictive. 
On the one hand, we implement a recommender system based on association rules mining. We use Apriori
and Eclat algorithms for this purpose.
On the other hand, for the recommender systems based on a classifier, we use Decision Trees, Logistic Regression, and Artificial Neural Network.

Finally, in order to generate the itemset recommendation for the user, we use a sort function over the output of the recommender systems based on its probabilities.

### Results

As we expected ANN model outperforms the other approaches, achieving F1 of 56%, and MAP of 78%, while the worst was Apriori approach reaching F1 of 48%, and MAP of 58%. Also, ANN obtained the best MRR, which might indicate that this model recommends the first best situational-item.

| Metric | Apriori | Eclat | D Tree | Logit |  ANN |
|--------|---------|-------|:------:|:-----:|:----:|
| F1@1   | 0.18    | 0.18  |  0.21  |  0.27 | **0.32** |
| MAP@1  | 0.43    | 0.44  |  0.52  |  0.67 | **0.77** |
| F1@6   | 0.48    | 0.48  |  0.37  |  0.47 | **0.56** |
| MAP@6  | 0.58    | 0.59  |  0.64  |  0.71 | **0.78** |

### Limitations

This first work proposes a novel use of recommendation systems for the recommendation of elements in MOBA games. However, there are some limitations to address:

* Models do not take advantage of relevant information about the context of the parties. For example, it may be useful to use information from roles or teams.
* In the context of MOBA games, an explanation of why a recommendation is given would be desirable.

If you want to explore the systems, you could find them here: https://github.com/vgaraujov/RecSysLoL

## Interpretable Contextual Team-aware Item Recommendation: Application in Multiplayer Online Battle Arena Games



### Transformer-based Architecture

<p align="center"> 
    <img src="/images/recsys-moba/model_ttir.png" width="750">
	<center>
	<figcaption>Network architecture of TTIR</figcaption>
	</center>
</p>

### Results

| Metric | D Tree | Logit |  ANN | CNN  | TTIR |
|--------|:------:|:-----:|:----:|------|------|
| F1@1   |  0.21  |  0.27 | 0.32 | 0.33 | **0.34** |
| MAP@1  |  0.52  |  0.67 | 0.77 | 0.79 | **0.80** |
| F1@6   |  0.37  |  0.47 | 0.56 | 0.58 | **0.60** |
| MAP@6  |  0.64  |  0.71 | 0.78 | 0.79 | **0.81** |

### Attention as Explanation

<p align="center"> 
    <img src="/images/recsys-moba/attention_matrix.png" width="750">
	<center>
	<figcaption>Visualization of the attention weights for each member of the Blue team on each member of both teams (bottom row).</figcaption>
	</center>
</p>

## Final Remarks

## Citation

If you find this work useful for your research, please consider citing our papers: 
```
@inproceedings{10.1145/3298689.3346986,
	author = {Araujo, Vladimir and Rios, Felipe and Parra, Denis},
	title = {Data Mining for Item Recommendation in MOBA Games},
	year = {2019},
	isbn = {9781450362436},
	publisher = {Association for Computing Machinery},
	address = {New York, NY, USA},
	url = {https://doi.org/10.1145/3298689.3346986},
	doi = {10.1145/3298689.3346986},
	booktitle = {Proceedings of the 13th ACM Conference on Recommender Systems},
	pages = {393–397},
	numpages = {5},
	keywords = {item recommendation, MOBA games, data mining},
	location = {Copenhagen, Denmark},
	series = {RecSys ’19}
}

@inproceedings{10.1145/3383313.3412211,
author = {Villa, Andr\'{e}s and Araujo, Vladimir and Cattan, Francisca and Parra, Denis},
title = {Interpretable Contextual Team-Aware Item Recommendation: Application in Multiplayer Online Battle Arena Games},
year = {2020},
isbn = {9781450375832},
publisher = {Association for Computing Machinery},
address = {New York, NY, USA},
url = {https://doi.org/10.1145/3383313.3412211},
doi = {10.1145/3383313.3412211},
booktitle = {Fourteenth ACM Conference on Recommender Systems},
pages = {503–508},
numpages = {6},
keywords = {Item Recommendation, Deep Learning, MOBA Games},
location = {Virtual Event, Brazil},
series = {RecSys '20}
}
```
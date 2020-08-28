---
title: 'Recommendation Systems for Item Recommendation in MOBA Games'
date: 2020-09-14
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


### Results
<p align="center"> 
<style type="text/css">
.tg  {border-collapse:collapse;border-spacing:0;}
.tg td{border-color:black;border-style:solid;border-width:1px;font-family:Arial, sans-serif;font-size:14px;
  overflow:hidden;padding:10px 5px;word-break:normal;}
.tg th{border-color:black;border-style:solid;border-width:1px;font-family:Arial, sans-serif;font-size:14px;
  font-weight:normal;overflow:hidden;padding:10px 5px;word-break:normal;}
.tg .tg-wp8o{border-color:#000000;text-align:center;vertical-align:top}
.tg .tg-mcqj{border-color:#000000;font-weight:bold;text-align:left;vertical-align:top}
.tg .tg-mqa1{border-color:#000000;font-weight:bold;text-align:center;vertical-align:top}
.tg .tg-73oq{border-color:#000000;text-align:left;vertical-align:top}
.tg .tg-fam6{border-color:#000000;text-align:center;text-decoration:underline;vertical-align:top}
</style>
<table class="tg">
<thead>
  <tr>
    <th class="tg-mcqj">Metric</th>
    <th class="tg-mqa1">Apriori</th>
    <th class="tg-mqa1">Eclat</th>
    <th class="tg-mqa1">D Tree</th>
    <th class="tg-mqa1">Logit</th>
    <th class="tg-mqa1">ANN</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td class="tg-73oq">F1@1</td>
    <td class="tg-wp8o">0.18</td>
    <td class="tg-wp8o">0.18</td>
    <td class="tg-wp8o">0.21</td>
    <td class="tg-wp8o">0.27</td>
    <td class="tg-fam6">0.32</td>
  </tr>
  <tr>
    <td class="tg-73oq">MAP@1</td>
    <td class="tg-wp8o">0.43</td>
    <td class="tg-wp8o">0.44</td>
    <td class="tg-wp8o">0.52</td>
    <td class="tg-wp8o">0.67</td>
    <td class="tg-fam6">0.77</td>
  </tr>
  <tr>
    <td class="tg-73oq">F1@6</td>
    <td class="tg-wp8o">0.48</td>
    <td class="tg-wp8o">0.48</td>
    <td class="tg-wp8o">0.37</td>
    <td class="tg-wp8o">0.47</td>
    <td class="tg-fam6">0.56</td>
  </tr>
  <tr>
    <td class="tg-73oq">MAP@6</td>
    <td class="tg-wp8o">0.58</td>
    <td class="tg-wp8o">0.59</td>
    <td class="tg-wp8o">0.64</td>
    <td class="tg-wp8o">0.71</td>
    <td class="tg-fam6">0.78</td>
  </tr>
</tbody>
</table>
</p>

## Interpretable Contextual Team-aware Item Recommendation: Application in Multiplayer Online Battle Arena Games

### Transformer-based Architecture

<p align="center"> 
    <img src="/images/recsys-moba/model_ttir.png" width="750">
	<center>
	<figcaption>Network architecture of TTIR</figcaption>
	</center>
</p>

### Results
<p style='text-align: justify;'>
<style type="text/css">
.tg  {border-collapse:collapse;border-color:#ccc;border-spacing:0;margin:0px auto;}
.tg td{background-color:#fff;border-color:#ccc;border-style:solid;border-width:1px;color:#333;
  font-family:Arial, sans-serif;font-size:14px;overflow:hidden;padding:10px 5px;word-break:normal;}
.tg th{background-color:#f0f0f0;border-color:#ccc;border-style:solid;border-width:1px;color:#333;
  font-family:Arial, sans-serif;font-size:14px;font-weight:normal;overflow:hidden;padding:10px 5px;word-break:normal;}
.tg .tg-wp8o{border-color:#000000;text-align:center;vertical-align:top}
.tg .tg-mcqj{border-color:#000000;font-weight:bold;text-align:left;vertical-align:top}
.tg .tg-mqa1{border-color:#000000;font-weight:bold;text-align:center;vertical-align:top}
.tg .tg-73oq{border-color:#000000;text-align:left;vertical-align:top}
.tg .tg-12k0{border-color:#000000;text-align:left;text-decoration:underline;vertical-align:top}
</style>
<table class="tg">
<thead>
  <tr>
    <th class="tg-mcqj">Metric</th>
    <th class="tg-mqa1">D Tree</th>
    <th class="tg-mqa1">Logit</th>
    <th class="tg-mqa1">ANN</th>
    <th class="tg-mcqj">CNN</th>
    <th class="tg-mcqj">TTIR</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td class="tg-73oq">F1@1</td>
    <td class="tg-wp8o">0.21</td>
    <td class="tg-wp8o">0.27</td>
    <td class="tg-wp8o">0.32</td>
    <td class="tg-73oq">0.33</td>
    <td class="tg-12k0">0.34</td>
  </tr>
  <tr>
    <td class="tg-73oq">MAP@1</td>
    <td class="tg-wp8o">0.52</td>
    <td class="tg-wp8o">0.67</td>
    <td class="tg-wp8o">0.77</td>
    <td class="tg-73oq">0.79</td>
    <td class="tg-12k0">0.80</td>
  </tr>
  <tr>
    <td class="tg-73oq">F1@6</td>
    <td class="tg-wp8o">0.37</td>
    <td class="tg-wp8o">0.47</td>
    <td class="tg-wp8o">0.56</td>
    <td class="tg-73oq">0.58</td>
    <td class="tg-12k0">0.60</td>
  </tr>
  <tr>
    <td class="tg-73oq">MAP@6</td>
    <td class="tg-wp8o">0.64</td>
    <td class="tg-wp8o">0.71</td>
    <td class="tg-wp8o">0.78</td>
    <td class="tg-73oq">0.79</td>
    <td class="tg-12k0">0.81</td>
  </tr>
</tbody>
</table>
</p>

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
```
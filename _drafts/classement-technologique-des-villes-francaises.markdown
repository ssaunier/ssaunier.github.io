---
layout: post
title: "Classement technologique des villes françaises"
description: "Découvrez quelles sont les agglomérations qui comptent le plus de développeurs sur GitHub"
category: blog
french: true
photo: corsica.jpg
---

Quitter Paris pour de plus vertes contrées, tel est l'ambition qui habite certains d'entre nous. Mais comme tout ingénieur qui se respecte, il faut planifier minitieusement la relocalisation. Combien de développeurs habitent Tours ? Meetup déçoit car [aucun groupe](http://www.meetup.com/find/?allMeetups=true&radius=31&userFreeform=Tours%2C+France&mcId=c1011764&mcName=Tours%2C+FR&sort=default) tech n'est constitué. GitHub quant à lui nous indique que [77 développeurs](https://github.com/search?q=location%3A%22Tours%22&type=Users&ref=searchresults) se sont déclaré, dont 6 avec pour langage principal Ruby.

J'ai voulu établir le classement des villes françaises en fonction du nombre d'utilisateurs GitHub, ce qui m'a attiré beaucoup d'attention sur Twitter :

<blockquote class="twitter-tweet" lang="en"><p>13 villes ont plus d&#39;un développeur pour 1000 habitants (approximation: dev = personne ayant un compte GitHub) <a href="http://t.co/oLK4GDyFca">pic.twitter.com/oLK4GDyFca</a></p>&mdash; Sébastien Saunier (@ssaunier) <a href="https://twitter.com/ssaunier/statuses/464839588633395200">May 9, 2014</a></blockquote>
<script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script>

## Limites

Comme les twittos me l'ont fait remarquer, les résultats obtenus vendredi dernier ont les limites suivantes :

1. Les agglomérations ne sont pas prises en compte (tant sur la population que sur la recherche d'utilisateurs GitHub)
1. Un développeur peut ne pas avoir de compte GitHub
1. Un utilisateur GitHub peut ne pas être un développeur
1. Les informations de *location* sur GitHub peuvent être obsolètes

Pour le point numéro deux, je fais l'hypothèse d'une équi-répartition des développeurs n'ayant pas de compte GitHub sur le territoire. Ainsi, cela changera le nombre absolu, mais pas le ratio developpeur sur nombre d'habitants. Concernant le point numéro trois, je pense qu'on peut appliquer le même argument. Finalement, le point numéro trois ne nous trouble pas trop car on peut supposer que a priori le taux d'information obsolète est équitablement réparti sur toutes les villes.

Le point numéro un nécessite un peu plus de travail.

## Méthodologie

Wikipedia nous donne généreusement la [liste des villes les plus peuplées](http://fr.wikipedia.org/wiki/Liste_des_communes_de_France_les_plus_peupl%C3%A9es). Elle m'a servi à établir le premier classement. À partir de cette liste, on peut explorer chaque ville pour voir à quelle communauté de commune elle est rattachée. Enfin, pour chaque communauté de commune, on récupère la liste exhaustive de toutes les communes la composant.

Ce travail a été réalisé de manière semi-automatique car les pages Wikipedia des communautés de communes ne suivent pas toutes le même patron. Vous pouvez consulter cette [base de données des communautés de communes](https://github.com/ssaunier/github-french-cities/blob/master/data/french_hubs.yml)

Ensuite, pour chaque ville, il faut demander à GitHub le nombre d'utilisateurs déclarés. On obtient [cette liste](https://github.com/ssaunier/github-french-cities/blob/master/data/github_users_per_city.yml) qui nécessite un retraitement manuel. En effet, *Orléans* sort un trop grand nombre de résultats en raison de l'existance de la ville de *New Orleans, Louisiane*.

Il reste maintenant à aggréger les résultats par métropole. Voici le classement.

## Top 20

|   | Agglomération | GitHub | Population | Dev / 1000 habitants |
|--:| ------------- | -----: | ---------: | -------------------: |
| 1 | **Paris** | 7895 | 2249975 | 3.51  |
| 2 | **Nantes** Métropole | 642| 582159| 1.1 |
| 3 | **Grenoble** Alpes Métropole|484|444810|1.09 |
| 4 | **Toulouse** Métropole|774|714332|1.08 |
| 5 | **Montpellier** Agglomération|429|423842|1.01 |
| 6 | **Rennes** Métropole|396|413953|0.96 |
| 7 | Grand **Lyon** |1134|1306972|0.87 |
| 8 | **Bordeaux** Métropole|570|727256|0.78 |
| 9 | **Strasbourg**|307|473187|0.65 |
|10 | Grand **Nancy**|153|256966|0.6 |
|11 | Région de **Compiègne**|43|74000|0.58 |
|12 | Plateau de **Saclay**|70|121276|0.58 |
|13 | **Levallois-Perret**|37|64629|0.57 |
|14 | **Lille** Métropole|630|1112470|0.57 |
|15 | **Brest** métropole océane|114|206893|0.55 |
|16 | **Le Mans** Métropole|95|197953|0.48 |
|17 | **Annecy** |66|140865|0.47 |
|18 | **Pau**-Pyrénées|60|150539|0.4 |
|19 | Métropole **Nice** Côte d'Azur|200|538613|0.37 |
|20 | **Sophia Antipolis**|64|176933|0.36 |

Si votre agglomération n'est pas dans le top 20, consulter [la liste des 180 agglomérations](https://github.com/ssaunier/github-french-cities/blob/master/data/github_users_per_hubs.csv) sur GitHub.

## Discussion

À l'heure où les villes candidatent pour le label [FrenchTech](http://www.lafrenchtech.com), ce classement anticipe sans doute sur l'issue des délibérations. Si vous avez notez une incohérence, n'hésitez pas à ouvrir une *Pull Request* sur [ssaunier/github-french-cities](https://github.com/ssaunier/github-french-cities). Vous pouvez également réexploiter le code / les données, elles sont placées sous license MIT. Merci à tous pour vos retours sur Twitter !

*Photo credit: [Terence S. Jones](https://www.flickr.com/photos/terence_s_jones/8011517843)*


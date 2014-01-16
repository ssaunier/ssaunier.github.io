---
layout: post
title: Réalité financière du Freelance Ruby on Rails
description: Enquête auprès des freelancers francophones sur leur activité
category: blog
french: true
extra_head: "<style type=\"text/css\">\n  .axis path,\n  .axis line {\n    fill: none;\n    stroke: #eee;\n    shape-rendering: crispEdges;\n  }\n\n  .axis text,\n  figcaption {\n    font-family: Helvetica, Arial, sans-serif;\n    font-size: 13px;\n  }\n  #forfait_taux_horaire svg rect {\n    fill: #588ECB;\n    stroke: white;\n  }\n\n  #panel_strucure_juridique svg rect {\n    fill: #314F71;\n    stroke: white;\n  }\n\n  figcaption {\n    font-style: italic;\n  }\n\n  figure {\n    margin-bottom: 2em;\n  }\n\n</style>\n"
published: true
---

J'ai lancé un [appel](https://groups.google.com/forum/#!topic/railsfrance/BPwrappeXlc)
le 14 octobre 2013 dernier pour sonder la réalité financière du [développeur Ruby on Rails](/fr/developpeur-ruby-on-rails-freelance-paris.html)
francophone. Vous avez été nombreux — 37 plus précisément — à y répondre et je vous en remercie. Entrons
sans plus attendre dans le vif du sujet.

## Taux Horaire

L'échantillon sondé a révélé un taux horaire brut **médian**, c'est-à-dire séparant la population
sondée en deux groupes de taille égale, de:

<div style="font-size: 3em; text-align: center; margin-top: 1em; margin-bottom: 1em">
  <strong>75€ HT / heure</strong>
</div>

Il est intéressant de comparer le taux horaire facturé et l'expérience du
développeur freelance. Une hypothèse vient rapidement à l'esprit : le taux horaire devrait
être corrélé à l'expérience. Or en observant le *scatter plot* ci-dessous, on peut voir
qu'il y a deux modèles qui divergent:

1. Les développeurs avec peu d'expérience facturant à un haut niveau
2. Les développeurs de longue date facturant raisonnablement

<figure class="center">
  <div id="houly_rate_by_experience_chart">
  </div>
  <figcaption>Les plus gros points représentent cinq personnes, les plus petits une seule.</figcaption>
</figure>

<del>À quoi correspond ce taux horaire de 75 euros brut ? En utilisant le site
[yourrate](http://yourrate.co), on calcule qu'en prenant **5** semaines de
vacances et en facturant **22** heures par **semaine**, un
développeur ruby peut gagner un salaire **net** de **3000 euros**.</del>
<em>&nbsp;Comme le fait remarquer <a href="http://twitter.com/jblemee">@jblemee</a>,
le calcul est approximatif et trop inspiré du salariat, les charges payées par le freelance
étant moins importantes que 50%. Discussion à suivre dans les commentaires.</em>

## Volume de travail

Cette étude a également pour but de cerner la charge de travail d'un freelance.
As-t-on des *workaholic* ne prenant jamais de vacances, ou bien des développeurs
très affutés facturant à prix d'or leur prestation et ne travaillant que 6 mois par an ?
Regardons les chiffres :

<figure class="center">
  <div id="quality_or_quantity">
  </div>
  <figcaption>Relation entre taux horaire et volume mensuel</figcaption>
</figure>


<figure class="center">
  <div id="work_load">
  </div>
  <figcaption>Congés pris en fonction du nombre d'heures facturées par mois</figcaption>
</figure>

Il semble qu'il y ait deux groupes de freelances, d'un côté ceux qui facturent moins
de 40 heures par mois, et de l'autre ceux qui facturent au moins 100 heures.
On a sans doute une différence entre certains auto-entrepreneurs qui exercent cette
activité à côté d'une activité salariée plus classique. Et de l'autre des freelances
qui ne vivent que de cette activité. Le point en haut à gauche est l'oeuvre d'un petit
malin qui a tout compris : travailler peu mais facturer beaucoup !

Quant aux congés, il semble que les freelances ne sacrifient pas leurs congés au nom
de leur activité, plus d'un quart des sondés indiquant prendre 6 semaines et plus.

## Forfait vs. Taux horaire

Une question qui revient pour chaque nouveau projet qu'un freelance aborde est de
savoir s'il doit facturer au forfait, c'est-à-dire donner une valeur totale au projet
que son client lui paiera, indépendemment du nombre d'heures passées sur le projet,
ou bien s'il doit indiquer un taux horaire et facturer chaque heure passée sur le
projet.

La deuxième solution est moins risquée pour le freelance car il se met à l'abri
des [erreurs d'estimations](http://www.quora.com/Engineering-Management/Why-are-software-development-task-estimations-regularly-off-by-a-factor-of-2-3) récurrentes sur tout projet de développement
qui sont payées cash par le freelance au forfait.

Les sondés ne s'y trompent d'ailleurs pas, voici la part de forfait dans leur activité :

<figure class="center">
  <div id="forfait_taux_horaire">
  </div>
  <figcaption>Les freelances privilégient une facturation au taux horaire</figcaption>
</figure>

## Structure juridique

Pour tout freelance qui démarre, un choix de statut est obligatoire. Et le moins
que l'on puisse dire, c'est qu'on est vite perdu. Entre le statut d'auto-entrepreneur,
celui de gérant d'EURL ou SARL, en entreprise individuelle (ou EIRL) ou bien encore
en SCOP ou en SASU, le choix n'est pas évident. À cela s'ajoutent le choix du
régime fiscal entre l'impôt sur le revenu et l'impôt sur les sociétés, qui démultiplie
les possibilités.

Voici donc les strucures juridiques choisies par le panel :

<figure class="center">
  <div id="panel_strucure_juridique">
  </div>
</figure>

On voit qu'un bon tiers choisissent le statut d'auto-entrepreneur,
ce qui semble logique étant donné la
simplicité du statut (déclaration trimestrielle de chiffre d'affaires, pas
de cotisation sans chiffre). Cependant le plafond de 32600 euros HT peut gêner
certains qui optent donc pour une structure juridique autre.

Autre remarque pertinente de [@gbetous](http://twitter.com/gbetous), la moitié
ont créé une personne morale (EURL, SARL, SAS, SASU, SCOP) alors que l'autre est
en son nom propre (auto-entrepreneur et entreprise individuelle).

Le nouveau statut SASU semble plébiscité par certains freelancers rencontrés à
[@dotRBeu](https://twitter.com/dotrbeu), à suivre
(ping [@Berlimioz](https://twitter.com/Berlimioz) !)

## Expression libre

La dernière question de l'enquête était libre, et portait sur les conseils à donner
aux freelances qui démarrent. Les voici :

> Fréquenter les conférences, être présent sur Twitter et la mailing list, rencontrer les autres freelance, participer aux projets open source. Apprendre à être commercial, se considérer comme son propre produit et se marketer en tant que tel. Connaitre ses forces et faiblesses, quitte à demander aux proches d'être franc à ce sujet.

<p></p>
> De ne pas le faire en France !! :)

<p></p>
> Je trouve pas mal de clients sur Odesk. Facturer a l'heure et s'engager le moins possible sur un ETA. Juste s'engager sur un nombre d'heures de travail/semaine et communiquer beaucoup.

<p></p>
> Ne démarrer qu'avec déjà une mission.

<p></p>
> Il faut toujours prendre en compte le temps passé à l'étude des demandes dans le taux horaire.

<p></p>
> Bouche à oreille : un client satisfait en fait venir d'autres. Savoir rester patient ;)

<p></p>
> Avoir un solide réseau, et pas que virtuel. Sortir, faire des confs, des apéros, des rencontres...

<p></p>
> Do it ! :)

<p></p>
> S'inscrire sur [freelancebooking.pro](http://freelancebooking.pro/fr), venir au meetup ruby, faire des présentations au meetup ruby, NE JAMAIS CÉDER AU COÛT FIXE, faire soi-même son [contrat](https://github.com/tibastral/contrats-francais)

<p></p>
> Savoir qu'on peut mettre 2 à 3 ans à atteindre un rythme de croisière, et prévoir les fonds pour les périodes creuses. Se fixer des limites dès le départ (tarifs, prestations, temps de travail) et s'y tenir, ou ça devient vite invivable. S'accorder le droit à l'erreur.

<p></p>
> Continuer à s'investir dans la communauté et l'open source...

<p></p>
> Espace de co-working : cadre de travail + réseau (la majeure partie de mes clients en sont issus), TDD sur chaque projet dépassant les 30/40h, éduquer le client pour qu'il poste ses issues directement sur Github, un bon suivi des Exceptions pour intervenir avant que le client ne puisse se plaindre

<p></p>
À noter que [@thibaut_barrere](http://twitter.com/thibaut_barrere) coache régulièrement des freelances qui se lancent pour leur donner un coup de pouce, n'hésitez pas à le contacter (mail dans la description de son profil twitter).

## Conclusion

Merci encore à tous ceux qui ont répondu à l'enquête, j'ai bien conscience que
cette étude est loin de respecter les règles statistiques élementaires mais
j'espère qu'elle vous donnera un aperçu de ce qui vous attend si vous vous lancez
en indépendant.

Vous pouvez consulter les données brutes
[ici](https://github.com/ssaunier/ssaunier.github.io/blob/master/data/french_freelance_ruby_on_rails.tsv),
n'hésitez pas à ajouter une analyse en commentaire ou des suggestions sur d'autres
données à croiser. Vous pouvez également consulter le [résumé de Google](https://docs.google.com/forms/d/1Ge7K6DO54Lzf-1wRwu_nJhYwMfpcXJvxaBMDLxCHNi4/viewanalytics) du sondage.

<script src="//cdnjs.cloudflare.com/ajax/libs/d3/3.3.11/d3.min.js">
</script>

<script src="/js/realite_financiere.js">
</script>
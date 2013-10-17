---
layout: post
title: "Réalité financière du Freelance Ruby on Rails"
description: ""
category: blog
extra_head: |
  <style type="text/css">
    .axis path,
    .axis line {
      fill: none;
      stroke: #eee;
      shape-rendering: crispEdges;
    }

    .axis text,
    figcaption {
      font-family: Helvetica, Arial, sans-serif;
      font-size: 13px;
    }
    figcaption {
      font-style: italic;
    }

    figure {
      margin-bottom: 2em;
    }

  </style>
---

J'ai lancé un [appel](https://groups.google.com/forum/#!topic/railsfrance/BPwrappeXlc)
le 14 octobre 2013 dernier pour sonder la réalité financière du développeur Ruby on Rails
francophone. Vous avez été nombreux — 37 plus précisément — à y répondre et je vous en remercie. Entrons
sans plus attendre dans le vif du sujet

## Taux Horaire

L'échantillon sondé a révélé un taux horaire brut **médian**, c'est-à-dire séparant la population
sondée en deux groupes de taille égale, de:

<div class="center" style="font-size: 3em; font-weight: bold; margin: 1em 0">
   75€ brut / heure
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

À quoi correspond ce taux horaire de 75 euros brut ? En utilisant le site
[yourrate](http://yourrate.co), on calcule qu'en prenant **5** semaines de
vacances et en facturant **22** heures par **semaine**, un
développeur ruby peut gagner un salaire **net** de **3000 euros**.

## Volume de travail

Cette étude a également pour but de cerner la charge de travail d'un freelance.
As-t-on des *workaholic* ne prenant jamais de vacances, ou bien des développeurs
très affutés facturant à prix d'or leur prestation et ne travaillant que 6 mois par an ?
Regardons les chiffres :

<figure class="center">
  <div id="work_load">
  </div>
  <figcaption>Congés pris en fonction du nombre d'heures facturées par mois</figcaption>
</figure>

<figure class="center">
  <div id="quality_or_quantity">
  </div>
  <figcaption>Relation entre taux horaire et volume mensuel</figcaption>
</figure>

Il semble qu'il y ait deux groupes de freelances, d'un côté ceux qui facturent moins
de 40 heures par mois, et de l'autre ceux qui facturent au moins 100 heures.
On a sans doute une différence entre certains auto-entrepreneurs qui exercent cette
activité à côté d'une activité salariée plus classique. Et de l'autre des freelances
qui ne vivent que de cette activité. Le point en haut à gauche est l'oeuvre d'un petit
malin qui a tout compris : travailler peu mais facturer beaucoup !

Quant aux congés, il semble que les freelances ne sacrifient pas leurs congés au nom
de leur activité, plus d'un quart des sondés indiquant prendre 6 semaines et plus.

<script src="http://d3js.org/d3.v3.min.js">
</script>

<script>
var margin = {top: 20, right: 20, bottom: 30, left: 40},
    width = 600 - margin.left - margin.right,
    height = 320 - margin.top - margin.bottom;

var color = d3.scale.category10();

function create_svg(selector) {
  return d3.select(selector).append("svg")
      .attr("width", width + margin.left + margin.right)
      .attr("height", height + margin.top + margin.bottom)
    .append("g")
      .attr("transform", "translate(" + margin.left + "," + margin.top + ")");
}

function x_axis(svg, xAxis, height, width) {
  return svg.append("g")
      .attr("class", "x axis")
      .attr("transform", "translate(0," + height + ")")
      .call(xAxis)
    .append("text")
      .attr("class", "label")
      .attr("x", width)
      .attr("y", -6)
      .style("text-anchor", "end");
}

function y_axis(svg, yAxis) {
  return svg.append("g")
      .attr("class", "y axis")
      .call(yAxis)
    .append("text")
      .attr("class", "label")
      .attr("transform", "rotate(-90)")
      .attr("y", 6)
      .attr("dy", ".71em")
      .style("text-anchor", "end")
}

function plot_dots(svg, data, r, cx, cy, fill) {
  return svg.selectAll(".dot")
      .data(data)
    .enter().append("circle")
      .attr("class", "dot")
      .attr("r", r)
      .attr("cx", cx)
      .attr("cy", cy)
      .style("fill", fill);
}

(function() {
  var x = d3.scale.linear()
    .range([0, width]);

  var y = d3.scale.linear()
    .range([height, 0]);

  var xAxis = d3.svg.axis()
      .scale(x.domain([0, 6]))
      .tickValues([0, 1, 2, 3, 4, 5])
      .tickFormat(function (d) { return d; })
      .orient("bottom");

  var yAxis = d3.svg.axis()
      .scale(y)
      .tickValues([0, 25, 50, 75, 100, 125, 150])
      .orient("left");

  var svg = create_svg("#houly_rate_by_experience_chart");

  d3.tsv("/data/french_freelance_ruby_on_rails.tsv", function(error, data) {
    var tuples = {};
    var max_r = 0;
    var tuple_selector = function(d) {
      return d.experience + "-" + d.hourlyRate;
    }
    data.forEach(function(d) {
      d.experience = +d.experience;
      d.hourlyRate = +d.hourlyRate;
      var key = tuple_selector(d);
      if (tuples[key]) {
        tuples[key] += 1;
      } else {
        tuples[key] = 1;
      }
      max_r = Math.max(max_r, tuples[key]);
    });

    var r = d3.scale.linear()
              .domain([1, max_r])
              .range([3, 15]);

    y.domain(d3.extent(data, function(d) { return d.hourlyRate; })).nice();

    x_axis(svg, xAxis, height, width)
      .text("Expérience (années)");

    y_axis(svg, yAxis)
      .text("Taux horaire (€)");

    var vLineY = height * 4.0 / 7 + 3;
    svg.selectAll(".vline").data(d3.range(26)).enter()
      .append("line")
      .attr("y1", vLineY)
      .attr("y2", vLineY)
      .attr("x1", 0)
      .attr("x2", width)
      .style("stroke", "#eee")
      .style("shape-rendering", "crispEdges");

    plot_dots(svg, data,
      function(d) { return r(tuples[tuple_selector(d)]); },
      function(d) { return x(d.experience); },
      function(d) { return y(d.hourlyRate); },
      function(d) { return color(d.hourlyRate >= 75); });
  });
})();

(function() {
  var x = d3.scale.linear()
    .range([0, width]);

  var y = d3.scale.linear()
    .range([height, 0]);

  var xAxis = d3.svg.axis()
      .scale(x.domain([0, 180]))
      .ticks(9)
      .tickFormat(function (d) { return d; })
      .orient("bottom");

  var yAxis = d3.svg.axis()
      .scale(y.domain([0, 6]))
      .tickValues([0, 1, 2, 3, 4, 5, "6+"])
      .tickFormat(function (d) { return d; })
      .orient("left");

  var svg = create_svg("#work_load");

  d3.tsv("/data/french_freelance_ruby_on_rails.tsv", function(error, data) {
    var tuples = {};
    var max_r = 0;
    var tuple_selector = function(d) {
      return d.monthBilledHours + "-" + d.holidayWeeks;
    }

    data.forEach(function(d) {
      d.monthBilledHours = +d.monthBilledHours;
      d.holidayWeeks = +d.holidayWeeks;
      var key = tuple_selector(d);

      if (tuples[key]) {
        tuples[key] += 1;
      } else {
        tuples[key] = 1;
      }
      max_r = Math.max(max_r, tuples[key]);
    });

    var r = d3.scale.linear()
              .domain([1, max_r])
              .range([3, 15]);

    x_axis(svg, xAxis, height, width)
      .text("Volume mensuel facturé (heures)");

    y_axis(svg, yAxis)
      .text("Congés (semaines)")

    plot_dots(svg, data,
      function(d) { return r(tuples[tuple_selector(d)]); },
      function(d) { return x(d.monthBilledHours); },
      function(d) { return y(d.holidayWeeks); },
      "green");
  });
})();

(function() {
  var x = d3.scale.linear()
    .range([0, width]);

  var y = d3.scale.linear()
    .range([height, 0]);

  var xAxis = d3.svg.axis()
      .scale(x.domain([0, 180]))
      .ticks(9)
      .tickFormat(function (d) { return d; })
      .orient("bottom");

  var yAxis = d3.svg.axis()
      .scale(y)
      .tickValues([0, 25, 50, 75, 100, 125, 150])
      .orient("left");

  var svg = create_svg("#quality_or_quantity");

  d3.tsv("/data/french_freelance_ruby_on_rails.tsv", function(error, data) {
    var tuples = {};
    var max_r = 0;
    var tuple_selector = function(d) {
      return d.monthBilledHours + "-" + d.hourlyRate;
    }

    data.forEach(function(d) {
      d.monthBilledHours = +d.monthBilledHours;
      d.hourlyRate = +d.hourlyRate;
      var key = tuple_selector(d);

      if (tuples[key]) {
        tuples[key] += 1;
      } else {
        tuples[key] = 1;
      }
      max_r = Math.max(max_r, tuples[key]);
    });

    var r = d3.scale.linear()
              .domain([1, max_r])
              .range([3, 15]);

    y.domain(d3.extent(data, function(d) { return d.hourlyRate; })).nice();

    x_axis(svg, xAxis, height, width)
      .text("Volume mensuel facturé (heures)");

    y_axis(svg, yAxis)
      .text("Taux horaire (€)")

    plot_dots(svg, data,
      function(d) { return r(tuples[tuple_selector(d)]); },
      function(d) { return x(d.monthBilledHours); },
      function(d) { return y(d.hourlyRate); },
      function(d) { return color(d.hourlyRate >= 75); });
  });
})();

</script>
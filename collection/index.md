---
title: Premier cabinet de curiosités
date: '2015-01-03 00:00:00'
layout: page
draft: false

---
**Statut :** Travaux d'ouverture d'un chantier pour [démarrer des collections Jekyll](/2014/12/31/demarrer-des-collections-dans-jekyll/). L'intention est de lister un premier échantillon de collection de notes de toutes sortes.

**Note technique jekyll** : à l'inverse des posts de blog accessibles sur la page principale, ces notes de collection sont classées par ordre chronologique. 

D'[autres clés de tri](https://github.com/jekyll/jekyll/issues/2515#issuecomment-46107601) seront étudiées selon la nature des notes à classer. (livres, albums, humeurs du jour, etc.) 

<ul>
{% assign sorted_notes = (site.notes | sort: 'date') %}
{% for collection in sorted_notes %}
<li class="h-entry hentry h-as-note"><a class="p-name entry-title e-content entry-content article post-link" href="{{ collection.url }}">{{ collection.title }}</a> - 
<time class="post-date dt-published" datetime="{{collection.date | date_to_xmlschema }}">
{% assign d = collection.date | date: "%-d"  %}{% case d %}{% when '1' %}{{ d }}er{% else %}{{ d }}{% endcase %} {% assign m = collection.date | date: "%-m" %}{% case m %}{% when '1' %}janv.{% when '2' %}févr.{% when '3' %}mars{% when '4' %}avr.{% when '5' %}mai{% when '6' %}juin{% when '7' %}juil.{% when '8' %}août{% when '9' %}sept.{% when '10' %}oct.{% when '11' %}nov.{% when '12' %}déc.{% endcase %} {{ collection.date | date: '%Y' }}
</time>
<p><i>{{ collection.description }}</i></p>
</li>
{% endfor %}
</ul>
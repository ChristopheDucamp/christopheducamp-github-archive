---
layout: page
title: "Aménagement cabinet de curiosités en cours"
---

Premier échantillon de collection dans la navigation primaire.

<ul>
  {% for collection in site.notes %}
     <li><a href="{{ collection.url }}">{{ collection.title }}</a></li>
     <p>{{ collection.short-description }}</p>

  {% endfor %}
</ul>
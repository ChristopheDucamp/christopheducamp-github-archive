---
layout: page
title: À Propos de ce Site, de Jekyll et du Jeu IndieWeb
date: 2015-01-04
---

## Mini-bio : explorations et chaos  

Bonjour, je m'appelle Christophe Ducamp et j'ai 54 ans. Père de famille de 3 enfants, je vis à Paris et mes champs d'intérêts personnels et professionnels évoluent autour de la _communication, la culture libre et l'innovation ouverte_.
 
Entrepreneur, parcours de plus de 20 ans dans la communication, le marketing direct et la banque, de tempérament créatif, curieux et enthousiaste, j'évolue depuis quelques années sur le _conseil en stratégie numérique_.

* À titre bénévole, j'ai initié en mars 2013 l'association [MyData labs](http://mydatalabs.com), une organisation à but non lucratif ayant pour ambition de développer une communauté de recherche et un écosystème entrepreneurial sur le marché des _données personnelles_. Le plan de route 2015 sera écrit le mois prochain.

* Après avoir contribué en 2014 au sein de [l'agence de communication 50a](http://www.50a.fr/equipe.php) sur des chantiers d'_émancipation numérique_, je repars pour 2015 dans la joie du freelance _solo_ avec l'objectif de poursuivre des _missions de design et facilitation_ appuyées sur  quelques expérimentations personnelles et tranversales sur la créativité, les "_[calm tech](http://en.wikipedia.org/wiki/Calm_technology)_" et l'innovation ouverte.

Pour une approche plus linéaire, des références clients, une archive de mon [CV est en ligne et sera bientôt mise à jour](http://christopheducamp.com/curriculumvitae.html). Secteurs de prédilection dans mon radar conversationnel : banque, distribution, presse, habitat sans oublier... les travailleurs indépendants.

## Motivations "IndieWeb"  

Non développeur et passionné par le logiciel, ce site web personnel est en cours d'ensemencement. À considérer simplement comme un "nouveau" petit jardin numérique de *geek libre* pour mon *[selfdogfood](http://indiewebcamp.com/selfdogfood-fr)*. à savoir, apprendre et pratiquer un peu de code au jour le jour, pour contribuer à la promotion du _web indépendant_ et imaginer de nouveaux projets d'entreprises.

Mes motivations de design sont alignées avec les valeurs _indieweb_ : posséder et maîtriser mes contenus et données tout en [restant connecté avec quelques amis et collègues présents sur les médias sociaux](http://indiewebcamp.com/POSSE) tels que twitter et facebook.

### Essais Moteurs 2015

Pour 2015, je poursuivrai divers essais de motorisation logicielle sur un mix composé de :

- [MediaWiki](/w/) : découverte en cours de l'extension Semantic-MediaWiki pour des usages documentaires orientés "data" et de l'accompagement en support de projets d'entreprises.
- [WordPress](/b/2013-05-15/chronoreve-indieweb-je-veux-bosser-dans-scrivener-et-publier-ou-je-veux-sur-le-web/) : solution de blog incontournable pour les travailleurs indépendants et se pliant bien aux exigences indieweb. Usages envisagés : accompagnement et autonomie des auteurs et travailleurs indépendants.
- [Known](http://withknown.com) : [premiers essais en cours](http://xtof.withknown.com) d'une plateforme très prometteuse pour les indépendants et petits groupes. En attente d'une installation locale.

Pour des raisons essentielles d'évolution, de retour à la simplicité et pratiquer quelques blocs de construction du web social libre –et faute de savoir construire ma propre motorisation web–, Jekyll est la solution que j'ai retenu pour évoluer sur l'indieweb en 2015. Pour les curieux, vous trouverez beaucoup plus d'informations sur la personnalisation de votre thème Jekyll, tout comme une documentation basique sur l'usage de Jekyll en visitant [jekyllrb.com](http://jekyllrb.com/).

Pour en savoir plus sur le développement de l'**indiewebcamp**, nous serons ravis de vous recevoir sur [indiewebcamp.com](http://indiewebcamp.com) : rejoignez-nous sur IRC, canal #indiewebcamp sur freenode.

## Moyens de Communication Préférés

Essayant depuis quelques années de [réduire ma dépendance au courrier électronique](http://christopheducamp.com/w/Protocoles_de_communication#Courriel), mon agenda reste ouvert pour un premier contact. N'hésitez pas à me joindre par ordre de préférence  

1. SMS à toute heure au +33 6 32 03 97 96
2. Message sur [Twitter:@xtof_fr](http://twitter.com/xtof_fr)
3. <span class="h-card" rel="me">[LinkedIn](https://www.linkedin.com/in/christopheducamp)</span>
4. irc : xtof sur le canal #indiewebcamp (freenode)

## "Ailleurs" sur les silos sociaux

Le web restant décentralisé, je suis aussi un _métayer_ [ailleurs](/ailleurs/) sur quelques silos et services toujours bien pratiques au quotidien. 

## Notes 

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
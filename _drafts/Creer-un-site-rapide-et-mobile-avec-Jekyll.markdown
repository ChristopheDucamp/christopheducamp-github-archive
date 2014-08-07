---
layout: post
title:  "Créer un Site Rapide et Mobile avec Jekyll"
date:   étude en cours - à venir...

categories: Jekyll
---

Étude en cours. Seul le lien original fait référence.<span class="h-cite"><cite class="pname u-url">[Creating a fast and mobile-friendly website with Jekyll](http://nicolashery.com/fast-mobile-friendly-website-with-jekyll/)</cite> (<span class="h-card p-author">Nicolas Hery</span>, <time class="dt-published">2013-06-21</time>)</span>


*Ceci est un parcours complet expliquant comment j'ai créé un site web statique avec Jekyll, en le maintenant rapide et compatible mobile.*

Jekyll est un générateur de site statique : il transforme les fichiers plein texte (en Markdown pour le contenu, Liquid pour les gabarits) en un site web HTML, CSS et JavaScript prêt pour le déploiement.

Il existe d'autres options pour créer un site web ou un blog, comme le très connu Wordpress, ou les solutions plus récentes Squarespace et Ghost. Celles-ci sont dynamiques, ce qui veut dire que votre contenu est stocké dans une base de données et rempli dans des modèles quand un utilisateur visite le site. Elles ont aussi une section d'administration, qui vous permet de vous connecter et de modifer votre contenu. Ceci fait que le site web est plus facile à mettre à jour, tout spécialement pour les personnes moins douées techniquement.

Mais utiliser un générateur de site statique présente quelques avantages sur ces autres solutions : 

* **Le site web est généralement très rapide** : Parce que ce ne sont que des fichiers statiques, il n'y a pas d'allers-retours vers une database, et les serveurs sont devenus vraiment bons pour servir des fichiers pleins.

* **Il passe aussi très facilement à l'échelle**: Même si vous avez un paquet de trafic, il est plus facile pour un serveur web statique de le gérer, qu'une application PHP WordPress.

* **Il est très facile à déployer**: il y a beaucoup d'options de fichiers de serveur web statiques ici, vous n'avez pas besoin de hooker et configurer une base de données, ou d'installer et maintenir quelque application.

* **Vous possédez vraiment votre contenu** : que des fichiers plein texte, généralement en Markdown, que vous pouvez maintenir en sécurité dans votre dépôt GitHub ou votre DropBox.

* **Il est extensible** : Le fait qu'un générateur statique comme Jekyll ne se concentre que sur la transformation de fichiers plein texte en HTML, et qu'il vous fournisse une interface de plugin, vous pouvez basiquement tout faire : ajouter des librairies tiers comme Disqus ou Google Analytics, créer votre propre logique pour insérer des images à l'intérieur des pages, etc.

Construire un site web en partant de zéro et en utilisant Jekyll peut paraître difficle au premier abord, car il y a beaucoup de morceaux à bouger et des trucs à configurer. Dans ce post, j'aimerais vous conter comment j'y suis parvenu, ce qui je l'espère ceci vous aidera à démarrer plus rapidement et servira de checklist des choses auxquelles vous devrez penser. Garder à l'esprit que ce n'est juste qu'une façon de faire les choses. Vous êtes plus que bienvenus pour adapter ce workflow à vos exigences.

C'est aussi  une lecture plutôt longue, parce que j'ai vraiment voulu y inclure le processus de bout en bout, de l'installation des outils jusqu'au déploiement du site. La table des matières en-dessous vous aidera à aller directement vers les différentes sections.

Et si vous voulez, vous pouvez suivre le parcours avec ce code source.

Table des matières (à installer)

    Installer Jekyll
    Installer Grunt
    Workflow
    CSS
    JavaScript
    Images
    Blocs de Code
    Commentaires
    SEO
    Analytics
    Déploiement
    Conclusion

## Installer Jekyll

Nous utiliserons Jekyll (Ruby) pour générer les fichiers HTML, l'outil de construction [Grunt](http://gruntjs.com/) (Node.js) pour compiler et minimiser les fichiers CSS et JavaScript, et [Pygments](http://pygments.org/) (Python) pour la mise en valeur de la syntaxe dans nos posts.

Ceci veut dire que vous devrez installer : 

* [Ruby](http://www.ruby-lang.org/) avec [RubyGems](http://rubygems.org/)
* [Node.js](http://) with [NPM](https://npmjs.org/)
* [Python](http://www.python.org/) avec [pip](http://www.pip-installer.org/)

Si vous n'avez pas tout cela installé et si vous utilisez un Mac, vous pouvez regarder mon guide de [réglages Mac Dev](https://github.com/nicolashery/mac-dev-setup) pour de l'aide.

Installer Jekyll avec :

    $ gem install jekyll

Jekyll a une [documentation géniale](http://jekyllrb.com/docs/home/) pour vous aider à apprendre comment l'utiliser.

Une structure de répertoire basique ressemble à quelque chose comme ça :

.
├── _includes/
|   ├── footer.html
|   ├── header.html
|   └── posts.html
├── _plugins/
|   ├── asset_url.rb
|   └── image_tag.rb
├── _layouts/
|   ├── default.html
|   ├── page.html
|   └── post.html
├── _posts/
|   ├── 2013-02-11-photos-de-chats.md
|   └── 2013-01-31-hello-world.md
├── _site/
├── _config.yml
└── index.html

Le site web statique est généré dans le répertoire `_site`. Tous les fichiers ou répertoires autres que ceux listés ci-dessus (comme les fichiers CSS et JavaScript) seront copiés à l'intérieur du répertoire _site, sauf si vous les excluez explicitement dans `_config.yml`.

Nous utilisons Git pour le contrôle de version, ainsi ajoutons tout de suite `_site` à notre fichier `.gitignore`.

##Installer Grunt (à étudier plus tard)

Jekyll traite notre layout et les fichiers de contenus texte en HTML. Nous utiliserons [Grunt](http://gruntjs.com/) pour traiter nos fichiers CSS et JavaScript.

Installez l'outil de ligne de commande de Grunt avec :

    $ npm install -g grunt-cli

Créons des répertoires pour nos fichiers source, et ajoutons-les à la liste exclude dans `_config.yml` afin qu'ils ne soient pas copiés sur le site :

...
├── css/
└── js/

Si vous utilisez des pré-processeurs, comme Sass, LESS ou CoffeeScript, vous pouvez ajouter les répertoires correspondants sass/, less/, ou coffee/ .

Nous créons aussi des répertoires pour nos fichiers concaténés (`debug/`) et minifiés (`build/`) qui seront copiés sur le site, et les ajouteront à `.gitignore` pour qu'ils ne soient pas suivis dans le contrôle de version.

...
├── debug/
└── build/

Pour finir, nous créons un répertoire `assets/` pour les plus grands fichiers tels que images, un répertoire que nous excluons du contrôle de version dans `.gitignore` :

...
└── assets/

Un mot d'explication : nous créerons nos feuilles de styles et scripts dans leurs répertoires source, en utilisant autant de fichiers car nous aimons conserver le code maintenable. Nous pourrons alors combiner ces fichiers à l'intérieur d'un fichier CSS `style.css` et d'un fichier JavaScript main.js dans le répertoire `debug/`, qui conservera un nombre de requêtes HTTP minimum pour de meilleures performances. Pour faire ainsi, nous pouvons utiliser les plugins Grunt suivants, selon les outils que vous utilisez (J'aime Compass pour CSS et Browserify pour JavaScript):

    grunt-contrib-concat
    grunt-contrib-sass
    grunt-contrib-compass
    grunt-contrib-less
    grunt-browserify2
    grunt-contrib-coffee

Pour le déploiement, nous minifierons auss les fichiers à l'intérieur de style.min.css et main.min.js dans le répertoire build/, en utilisant :

    grunt-contrib-cssmin
    grunt-contrib-uglify

Toutes ces tâches sont définies dans le fichier `Gruntfile.js` de votre projet. Vous pouvez regarder la documentation de Grunt et mon exemple Gruntfile.js pour un premier démarrage.

## Workflow

J'ai réglé quelques tâches Grunt pour m'aider à automatiser mon workflow. Durant le développement, au moment de modifier mon layout de site, d'écrire du contenu, de modifier les CSS, etc. j'ouvre une première fenêtre de terminal et lance la commande : 

    $ grunt debug

Cette tâche suit les modifications dans les fichiers CSS et JS (et/ou Sass, LESS, CoffeeScript), compile et concatène les fichiers sources dans le répertoire `debug/`. Dans une seconde fenêtre de terminal, je lance :

	$ grunt server

Cette tâche est fondamentalement un alias pour `jekyll serve --watch`, qui fait tourner le serveur de développement sur `http://localhost:4000/ ` et suit aussi les modifications dans le layout Liquid ou les fichiers de contenus Markdown, et reconstruit le site.

Avant le déploiement, je lance :

	$ grunt build

Ceci compile, concatène et minifie les styles et scripts, tout comme cela re-génère la totalité du site Jekyll, et cela utilise la variable d'environnement `JEKYLL_ENV` en la réglant sur "production" pour dire à mes templates Jekyll de pointer les fichiers minifiés dans le répertoire `build/` au lieu de `debug/`.

Nous verrons plus tard, mais pour déployer le site web et publier des modifications, j'utilise la commande : 

	$ jekyll-s3

Les tâches Grun décrites ci-dessus utilisent une combinaison des plugins cités précédemment. Je vous invite à examiner mon fichier  [Gruntfile.js](https://github.com/nicolashery/nicolashery.com/blob/master/Gruntfile.js) et à l'utiliser comme inspiration pour votre propre workflow.

## CSS

Écrire de la CSS plein texte a ses limites, et il existe des pré-processeurs comme Sass (Ruby) et LESS (Node.js) qui vous aide à faire le travail. Il existe aussi des frameworks comme Bootstrap (LESS) et Foundation (Sass) qui vous donnent un bon point de départ avec des styles pour les éléments comme les grilles, boutons, typographie, etc.

Personnellement, j'utilise Sass avec Compass. J'aime aussi Inuit.css comme framework léger et extensible. J'expliquerai comment les régler, mais vous pouvez utiliser n'importe quel outil qui vous plaira.

Pour installer Compass et son plugin framework Inuit :

	$ gem install compass compass-inuit

Je configure ensuite le plugin Grunt grunt-contrib-compass dans mon fichier Gruntfile.js et lui ajoute mon grunt et les tâches  grunt build.

Utiliser les préprocesseurs vous aide à maintenir une CSS modulaire. Si ça vous intéresse, vous pouvez plonger dans le travail réalisé autour des CSS Orientées Objets (OOCSS) ou SMACS. Pour démarrer simplement, je trouve que c'est une bonne pratique de séparer les styles spécifiques à certaines parties de votre site web en différents fichiers, et de préfixer les règles CSS avec ces noms de "modules". Par exemple : 

sass/
├── ui/
|   ├── _footer.scss
|   ├── _header.scss
|   ├── _page.scss
|   └── _scaffolding.scss
├── _config.scss
└── main.scss

// _header.scss

.header--nav {
  @extend .nav;
  @extend .nav--banner;

  margin-bottom: 0;
}

// ...

Désormais il est temps de dire quelques mots sur la construction d'un site web qui soit compatible mobile. C'est devenu une bonne pratique que d'utiliser le [Responsive Web Design](http://en.wikipedia.org/wiki/Responsive_web_design), ce qui veut dire construire un site qui s'adapte avec élégance aux différentes tailles d'écran. Pour faire ainsi, ajoutez ce qui suit dans votre balise HTML <head> HTML :

	<meta name="viewport" content="width=device-width, initial-scale=1.0">

Vous pouvez ensuite utiliser les [media queries](https://developer.mozilla.org/en-US/docs/Web/Guide/CSS/Media_queries) CSS3 pour définir les règles pour certaines tailles d'écran. J'aime l'approche [mobile-first](http://www.html5rocks.com/en/mobile/responsivedesign/), aussi je designe souvent mon site en priorité pour le plus petit écran, puis ajoute des règles pour compléter ou écraser les styles quand l'écran s'agrandit. Par exemple :

body {
  max-width: 700px;
  padding: 12px;
  padding-bottom: 0;
}

@media only screen and (min-width: 481px) {
  body {
    padding-top: 24px;
    padding-right: 24px;
    padding-left: 24px;
  }
}

Pour finir, incluez le lien vers la feuille de style concaténée dans la balise <head> de _layouts/default.html :

{% if site.env == 'production' %}
  <link rel="stylesheet" href="/build/style.min.css">
{% else %}
  <link rel="stylesheet" href="/debug/style.css">
{% endif %}

## JavaScript

J'utilise le gestionnaire de package [Bower](http://bower.io/) pour installer les librairies JS front-end, comme [jQuery](http://jquery.com/). Il a un registre vraiment complet de ces composants front-end. Quelques bibliothèques front-end peuvent aussi être trouvées sur NPM.

Bower installera les bibliothèques dans le répertoire components/. Par exemple : 

$ bower install jquery fastclick

J'utilise ensuite Browserify et le plugin Grunt grunt-browserify2 pour combiner ces différentes bibliothèques en un seul fichier unique JS. Mon fichier JS principal ressemble à quelque chose comme suit : 

// js/main.js

var $ = require('jquery')
  , FastClick = require('fastclick');

// On DOM ready
$(function() {

  // Eliminates the 300ms delay between a physical tap and the firing of a
  // click event on mobile browsers
  new FastClick(document.body);

  // ...

});

Pour pouvoir écrire require('jquery'), vous devez utiliser  browserify-shim dans votre Gruntfile.js et fournir le chemin vers chaque composant.

Dans votre layout default.html, faites un lien vers le fichier JS concaténé juste avant la balise de fermeture </body> (pour une meilleure performance) : 

  <!-- ... -->
  {% if site.env == 'production' %}
    <script src="/build/app.min.js"></script>
  {% else %}
    <script src="/debug/app.js"></script>
  {% endif %}
</body>

## Images

Concernant la performance du site web, les images sont une chose pointue. Nous avons pris le temps de concaténer et de minifier nos fichiers CSS et JS, mais une seule image peut peser plus de kilobites que tous les fichiers combinés.

Comme le montre l'article de Dave Rupert ["Ughck. Images.](http://daverupert.com/2013/06/ughck-images/)", la solution pour des "Images Responsive" n'est pas encore là.

Dans l'intervalle, j'avais besoin de quelque chose de relativement simple qui me permettrait d'inclure des images sur un site web dans un sens où il pouvait se charger rapidement. Je me suis concentré sur deux points : 

* Chargement paresseux des images uniquement une fois que l'utilisateur scrolle vers elle, grâce au plugin jQuery JAIL
* Images responsive qui chargent une version différente selon la taille et la résolution de l'écran, grâce à une version modifiée de la bibliothèque picturefill (petit bricolage pour la rendre compatible avec JAIL)

Après avoir installé et réglé les bibliothèques JavaScript, je crée un plugin Jekyll appelé image_tag.rb, qui me permet d'insérer facilement des images dans mes fichiers Markdown avec une balise personnalisée Liquid : 

{% image mon-image.png "Texte alt image" "légende facultative image" %}

Avec ça en place, les pages du site web devraient être plus rapides car les images ne sont pas chargées jusqu'à ce qu'elles deviennent visible dans le viewport du browser.

I chose this solution because it is simple, and my website doesn't use images much, but there are other options out there. For instance, Paul Stamatiou, who also does photograpy, offers a more sophisticated solution in his article "Developing a responsive, Retina-friendly site".


## Blocs de code

Avec un blog sur la programmation, je vais utiliser beaucoup d'exemples de code. Il est important qu'ils s'affichent élégamment, tant sur les ordinateurs de bureau que les mobiles.

Pour la mise en valeur de la syntaxe, Jekyll dispose d'une belle intégration avec [Pygments](http://nicolashery.com/fast-mobile-friendly-website-with-jekyll/). Pour l'installer, lancez :

	$ pip install pygments

Et ajoutez à votre fichier _config.yml :

pygments: true

J'aime utiliser les blocs de code encadrés, comme on les trouve dans le [Markdown-Enrichi de GitHub](https://help.github.com/articles/github-flavored-markdown), au lieu de la mise en valeur des balises hightlight de Liquid. Pour faire ainsi, j'ai switché mon analyseur Markdown de Jekyll pour [redcarpet](https://github.com/vmg/redcarpet) :

	$ gem install redcarpet

Et ajouté au fichier `_config.yml` :

	markdown: redcarpet

Et désormais vous pouvez inclure les blocs de code avec : 

```javascript
var msg = 'Hello world!';
```

Pour moi, cela présente aussi l'avantage de faire en sorte que les fichiers Markdown de mes posts de blog soient compatibles avec  [Marked](http://markedapp.com/) (voir aussi "Strip YAML Front Matter" dans les préférences de Marked).

Comme je l'ai dit, je trouve important que les blocs de code s'affichent bien tant sur un petit écran mobile que sur un ordinateur de bureau. Pour parvenir à cela, j'utilise d'abord la règle de CSS suivante : 

`pre {
  white-space: pre-wrap;
}
`
Ceci emballera le code quand une ligne est plus longue que la taille de l'écran, au lieu d'afficher une barre de défilement horizontal  (Je trouve affreux le défilement horizontal, mais ce ne pourrait être que moi). Le code emballé n'est pas génial néanmoins, et un peu difficile à lire. Pour limiter cela sur les petis écrans, diminuez la taille de police font-size pour faire qu'autant de code que possible puisse tenir en une ligne, en utilisant media queries : 

`pre, pre > code {
  /* Make more code fit on small screens */
  font-size: 14px;
}
`
`
@media only screen and (min-width: 481px) {
  pre, pre > code {
    /* Bigger font on bigger screens */
    font-size: 16px;
  }
}
`

Pour finir, pour les couleurs et la mise en valeur de la syntaxe, j'aime le thème [Solarized](http://ethanschoonover.com/solarized). J'ai assemblé [deux feuilles de style CSS](https://gist.github.com/nicolashery/5765395) à utiliser avec Pygments et Jekyll, les versions "Light" et "Dark".

Ici j'ai réalisé toute l'importance de tester un site web sur un véritable terminal mobile (versus un simple browser de bureau retaillé). Au lieu de ça, le thème Solarized Dark était élégant sur l'écran lumineux de mon MacBook Air, mais bien trop foncé et difficile à lire sur l'écran de mon iPhone. C'est l'une des raisons pour laquelle j'ai opté pour Solarized Light.

## Commentaires

Beaucoup de blog et de sites web utilisent Disqus pour gérer leurs commentaires. Régler ça avec Jekyll est vraiment facile.

Premièrement, vous devez créer un compte Disqus si vous n'en avez pas déjà un, puis enregsitrer votre site.

Une fois que c'est fait, collez le code universel que Disqus vous donne dans un include Jekyll, _includes/disqus.html, et remplacez shortname par un output Liquid : 

var disqus_shortname = '{{ site.disqus.shortname }}'; 

Puis, dans votre _config.yml, insérez votre pseudo Disqus :

disqus:
    shortname: 'nicolashery'

Désormais, à chaque fois que vous voudrez ajouter des commentaires Disqus (par exemple dans _layouts/post.html), tout ce que vous devrez faire c'est inclure disqus.html avec le marquage Liquid :

{% include disqus.html %}

## SEO

Je serai honnête, je ne connais pas grand chose à propos de l'Optimisation pour les Moteurs de Recherche. Ceci pourrait être un peu naïf, mais pour moi le meilleur moyen de rester en haut de la recherche est d'avoir du bon contenu, d'en parler aux gens, et s'ils apprécient ils produiront des liens vers votre contenu, et ceci ramènera plus de personnes et aidera à votre ranking.

J'ai appris néanmoins, essentiellement grâce à l'article de Segment.io "The Quickest Wins in SEO", qu'il existe quelques étapes basiques que vous devriez envisager.

Premièrement, il est bon d'inclure une rapide description meta dans le marquage <head> de votre page, car Google affichera cela sous le lien de votre site dans les résultas de recherche :

<meta name="description" content="Rapide description de mon site web qui figurera dans les resutlats de recherche.">

Puis, ajoutez un fichier robots.txt dans votre répertoire racine, qui dit aux crawlers (mais ne les force pas) quelles sont les parties du site qu'ils devraient indexer ou non. Ne vous en souciez pas trop, utilisez la forme la plus simple de robots.txt :

User-agent: *
Allow: /

Je ne suis pas certain que ce soit vraiment en rapport avec le SEO, mais c'est aussi une bonne pratique que d'ajouter un fichier  error.html ou 404.html dans le répertoire racine, qui s'affichera par exemple si un utilisateur fait une faute de frappe dans un lien vers votre site. Généralement cette page affiche un message court "Not Found" avec un lien vers d'autres parties du site  (exemple : GitHub 404).

Pour terminer, incluez un Sitemap, qui est un fichier XML qui aide les crawlers à trouver du contenu à indexer sur votre site. Avec Jekyll, tout ce dont vous avez besoin c'est de copier le plugin  sitemap_generator.rb dans votre répertoire _plugins. Re-générez le site, et un fichier sitemap.xml apparaîtra dans le dossier _site. 

## Flux RSS

Un flux RSS est un bon moyen pour les visiteurs de rester informés des nouveaux billets de blog quand ils ajoutent le flux à leurs lecteurs de flux.

Pour générer le fichier XML à votre flux RSS, copiez simplement le   template [feed.xml](https://github.com/snaptortoise/jekyll-rss-feeds/blob/master/feed.xml) vers le dossier racine de votre projet, et ajoutez un lien vers /feed.xml quelque part sur votre site.

## Analytics

Je ne vous expliquerai pas comment paramétrer Google Analytics sur le site, mais vous pouvez adapter ça à d'autres services d'analytics tels que Mixpanel, GoSquared, etc. (pour une liste complète de différents services, regardez la documentation  Segment.io).

Tout d'abord, enregistrez-vous sur Google Analytics avec votre compte Google, si vous n'en avez pas déjà ouvert un.

Puis, ajoutez le fragment de code fourni dans la documentation à l'intérieur d'un include Jekyll, comme _includes/google_analytics.html. Remplacez le code de suivi par un output Liquid : 

_gaq.push(['_setAccount', '{{ site.google_analytics_id }}']);

Vous pourriez placer votre Google Analytics ID dans le _config.yml, mais c'est mieux de ne pas committer ces types de tokens dans le repository Git. Au lieu de ça, je l'ai réglé comme une variable d'environnemnet dans le terminal qui fait tourner le build Jekyll. Afin de faire ça, j'ai créé un plugin simple  environment_variables.rb dans lequel j'ajoute la ligne :

site.config['google_analytics_id'] = ENV['GOOGLE_ANALYTICS_ID']

De cette façon, je peux faire tourner :

$ export GOOGLE_ANALYTICS_ID='UA-XXXXX-Y'
$ grunt build

Qui produira la valeur de variable d'environnement disponible vers le template Liquie à travers site.google_analytics_id.

Comme c'est expliqué dans la documentation de Google, incluez le fragment juste avant la balise de fermeture </head> avec :

  <!-- ... -->
  {% include google_analytics.html %}
</head>

## Déploiement

Le déploiement a quelques pièces différentes que nous aurons besoin de sélectionner : 

    Un hôte pour les fichiers statiques (ex: Amazon S3, GitHub Pages)
    Un registrar pour le nom de domaine (ex : Gandi) 
    Facultatif, un fournisseur séparé de DNS (ex : DNSimple, Amazon Route 53)
    En option, un CDN (ex: CloudFront)

Comme précisé au début de ce post, un avantage d'un site web statique est qu'il peut être déployé sur presque n'importe quel hébergeur qui sait servir des fichiers statiques. Ici j'expliquerai comment le déployer sur Amazon S3, qui a un modèle de tarif basé sur le "paiement-au-trafic" (cela devient vraiment très bon marché si vous avez une tonne de trafic). Si vous n'avez pas beaucoup d'images ou d'autres gros fichiers, GitHub Pages est aussi une belle option.

Je vais aussi expliquer comment héberger votre site au domaine racine "root" (ou "naked" ou "apex") (par ex. "nicolashery.com" au lieu de "www.nicolashery.com"), ce qui est un peu plus délicat. C'est ma préférence, mais si vous aimeriez héberger sur le domaine  "www", vous devriez pouvoir adapter ces instructions vraiment aisément.

Publier un site web Jekyll sur Amazon S3 est vraiment facile, grâce à l'outil jekyll-s3. Créez un compte chez Amazon Web Services (AWS) si vous n'en avez pas, puis installez l'outil avec : 

$ gem install jekyll-s3

Dans la console Amazon S3, créez un bucket (sélectionnez la région   "US Standard") avec un nom qui correspond à votre nom de domaine, (dans mon cas "nicolashery.com").

Note : Parce que j'héberge à la racine du domaine "nicolashery.com", je créerai aussi un second bucket "www.nicolashery.com" que je laisserai vide, et le configurerait pour rediriger toutes les requêtes vers "nicolashery.com", comme c'est expliqué dans la documentation d'AWS.

Puis, créez un fichier _jekyll_s3.yml avec votre identité Amazon déclarée comme variables d'environnement :

	s3_id: <%= ENV['S3_ID'] %>
	s3_secret: <%= ENV['S3_SECRET'] %>
	s3_bucket: nicolashery.com
	gzip: true

Vous obtiendrez vos credentials Amazon à partir de la console AWS  (cliquez sur votre nom tout en haut à droite et "Security Credentials"), puis réglez les variables d'environnement : 

	$ export S3_ID='YOUR_AWS_S3_ACCESS_KEY_ID'
	$ export S3_SECRET='YOUR_AWS_S3_SECRET_ACCESS_KEY'

Une fois que c'est fait, faites tourner l'outil de configuration de bucket fourni avec jekyll-s3, qui le préparera pour héberger un site web statique (à ce stade, dites "no" quand il vous demande si vous voulez configurer le CloudFront CDN) :

configure-s3-website --config-file _jekyll_s3.yml

Pour finir, construisez votre site pour la production et déployez-le vers Amazon S3 avec :

	$ grunt build
	$ jekyll-s3

Vous pouvez déjà visiter le site live avec l'adresse exemple exemple.com.s3-website-us-east-1.amazonaws.com address fournie  (remplacez naturellemnet "exemple.com" avec votre domaine).

Pour pointer notre nom de domaine vers ce bucket Amazon S3, nous devons configurer ses enregistrements DNS. Si nous hébergions sur le domaine "www.exemple.com", tout ce dont nous aurions besoin serait de créer un enregistrement CNAME : 

CNAME www.exemple.com www.exemple.com.s3-website-us-east-1.amazonaws.com

Cependant, nous hébergeons à la racine du domaine "exemple.com", ce qui requiert un enregitrement A qui doit pointer vers une adresse IP. C'est là où ça devient délicat : l'adresse IP de notre Amazon S3 peut changer.

Pour contourner ça, nous devons utiliser un fournisseur de DNS séparé, comme DNSimple ou Amazon Route 53. Tous les deux ont des enregisrements spéciaux ALIAS (DNSimple ALIAS, Route 53 ALIAS) qui vous permettent de pointer un domaine racine ("exemple.com") vers un autre domaine, comme un CNAME.

Dans le cas de DNSimple, nous créons deux enregistrements :

ALIAS example.com example.com.s3-website-us-east-1.amazonaws.com
CNAME www.example.com www.example.com.s3-website-us-east-1.amazonaws.com

Pour utiliser un fournisseur de DNS séparé, vous devez configurer votre registrar (le mien c'est Gandi) pour pointer des propres serveurs DNS.

Je ne plongerai pas maintenant dans la façon de configurer un CDN comme CloudFront. Différents CDN, DNS, combinaisons d'hébergeurs pourraient faire l'objet d'un article complet. Sentez-vous libre de lire la documentation CloudFront, ce devrait être facile à paramétrer en utilisant jekyll-s3 et Route 53.


## Conclusion

Utiliser Jekyll requiert un peu de bricolage et de réglages, mais nous espérons que ce parcours vous aidera à démarrer plus vite.

J'aime le fait que créer "à partir de zéro" vous force à apprendre et comprendre ce qui entre en considération pour produire un site web rapide et moderne. J'aime aussi apprécier la flexibilité que cela vous donne, parce que vous pouvez utiliser des plugins et d'autres outils comme Grunt pour l'adapter à ce qui est spécifique pour votre projet.

Vous avez trouvé cet article utile ? Utilisez-vous Jekyll et avez-vous un flux de production différent que vous aimeriez partager ? Sentez-vous libre d'en parler !
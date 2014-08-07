---
layout: post
title: "Créer un Blog avec Jekyll et Pages GitHub"
description: "Tutoriel pour démarrer facilement sur Jekyll"
category: 
tags: [CMS, Frameworks, workflow, Jekyll]
---
_Statut : Ce qui suit est une traduction en cours d’étude d’un article de <span class="h-card microcard"><img alt="barry clark" src="https://avatars3.githubusercontent.com/u/692762" />[Barry Clark](http://www.barryclark.co/)</span> publié <time datetime="2014-08-01">cette semaine</time> pour Smashing Magazine. Un article vraiment remarquable pour passer les premiers obstacles de mise en route sur Jekyll. Pas de ligne de commande à cette heure ! Une traduction à suivre pour étude et raffinage qui pourra se prêter à un premier défi personnel, ouvert en collaboration à tous les membres indieweb francophones : tenter de produire un tutoriel acceptable de [niveau 2 sur l’échelle indiemark](http://indiewebcamp.com/IndieMark#Level_2) ! 

À cette heure et comme à l’habitude, [seul le lien original](http://www.smashingmagazine.com/2014/08/01/build-blog-jekyll-github-pages/) fait référence ! _  

*J’ai récemment migré mon blog de WordPress vers Jekyll, un générateur fantastique de site web conçu pour construire des blogs minimalistes et statiques à héberger sur les Pages GitHub. La simplicité de la couche de thème Jekyll et le flux de production d’écriture sont merveilleux ; néanmoins, l’installation de mon site m’a pris plus de temps que tout ce à quoi je pouvais m’attendre.*

Dans cet article, nous avancerons pas à pas sur ce qui suit : 

  * le moyen de plus rapide d’installer un blog motorisé par Jekyll ; 
  * comment éviter les problèmes communs en utilisant Jekyll ;
  * comment importer votre contenu provenant de WordPress, utiliser votre nom de domaine et bloguer dans votre éditeur préféré ;
  * comment construire des thèmes dans Jekyll, avec des exemples de gabarits Liquid ;
  * quelques nouvelles fonctionnalités de Jekyll 2.0, comprenant le support de Sass et CoffeeScript ainsi que les collections.

## La Promesse Simple de Jekyll

Tom Preston-Werner a créé Jekyll pour permettre aux personnes de bloguer en utilisant un simple site web statique HTML, avec tout le contenu hébergé doté du système de contrôle de version GitHub. 
Le but était d’éliminer la complexité des autres plateformes de blog en créant un flux de production qui vous permette de [bloguer comme un hacker](http://tom.preston-werner.com/2008/11/17/blogging-like-a-hacker.html).

![La mascotte Octocat de Jekyll](http://www.smashingmagazine.com/wp-content/uploads/2014/07/octojekyll-opt.jpg)La mascotte Octocat de Jekyll. (Crédit image: [GitHub](http://jekyllrb.com/))

Jekyll prend votre contenu écrit en Markdown, le passe à travers vos gabarits et recrache tout sous forme d’un site web statique complet, prêt à être servi. GitHub Pages sert directement le site web à partir de votre dépôt GitHub pour que vous n’ayez pas à vous soucier de quelque hébergement.

Voici quelques sites web qui ont été créés avec Jekyll : 

  * [Le blog de Barry Clark](http://www.barryclark.co/?utm_source=smashing&utm_medium=post&utm_campaign=jekyll) (NDT : l’auteur original de l’article)
  * [Le blog de Zach Holman](http://zachholman.com/)
  * [CSS Wizardry](http://csswizardry.com/about/#colophon)
  * [Jekyll](http://jekyllrb.com/)
  * [HealthCare.gov](https://www.healthcare.gov/) Page d’accueil et sous-pages de contenus
  * [Development Seed](http://developmentseed.org/blog/2013/10/24/its-called-jekyll/)


## Les Avantages d’une Migration Vers le Statique

- **Simplicité**   
Jekyll dépouille tout au strict minimum, éliminant beaucoup de complexité : 

    * **Pas de base de données**  
À la différence de WordPress et d’autres systèmes de gestion de contenus (CMS), Jekyll n’a pas de database. Tous les billets de blog et pages sont convertis en HTML statique avant publication. C’est vraiment génial pour la vitesse de chargement parce qu’aucun appel de database n’est réalisé quand une page est chargée.

    * **Pas de CMS**  
Écrivez simplement tous vos contenus en Markdown, et Jekyll les transformera à travers des gabarits pour générer votre site web statique. GitHub peut servir de CMS au besoin parce que vous pouvez éditer votre contenu dessus.

    * **Rapide**  
Jekyll est rapide parce qu’étant totalement dépouillé et sans database, vous ne faites que servir des pages statiques. Mon thème de base [Jekyll Now](http://github.com/barryclark/jekyll-now) ne produit que trois requêtes HTTP -- y compris l’image de profil et les icônes sociales ! 

    * **Minimal**  
Votre site web Jekyll n’inclura aucune fonctionnalité que vous n’utiliserez pas.

- **Contrôle du Design**  
Passez moins de temps à lutter avec des templates complexes écrits par d’autres personnes, et plus de temps à créer votre propre thème ou à personnaliser un thème de base non compliqué.

 - **Securité**  
La plupart des vulnérabilités qui touchent les plates-formes comme WordPress n’existent pas parce que Jekyll n’a pas de <abbr title="Content Management System">CMS</abbr>, de database ou de PHP. Par conséquent, vous n’aurez plus autant de temps à installer des mises à jour de sécurité.

- **Hébergement Pratique**  
Pratique si vous utilisez déjà GitHub, ce qui est le cas ? GitHub Pages construira et hébergera gratuitement votre site web, tout en gérant simultanément le contrôle de version.

#### Essayons  

Il existe plusieurs façons de démarrer avec Jekyll, chacune avec ses propres variantes. Voici quelques options : 

  * Installer Jekyll localement via la ligne de commande, créer un nouveau site web passe-partout en utilisant `jekyll new`, le construire localement avec `jekyll build`, puis le servir. (Le [site web Jekyll](http://jekyllrb.com/) montre ce workflow.)
  * Cloner un point de départ sur votre machine locale, installer Jekyll localement via la ligne de commande, produire les mises à jour sur votre site web, le construire localement et le servir.
  * Forker un point de départ, produire des modifications, et puis le servir.

Nous commencerons par l’option la plus rapide et la plus facile : forker un point de départ. Ceci nous permettra de démarrer en quelques minutes, et nous n’aurons pas à installer les dépendances. Voici ce que nous allons faire pour aller directement sur GitHub.com dans le navigateur :

  1. Créer notre site web motorisé par Jekyll.
  2. L’héberger gratuitement sur GitHub Pages.
  3. Le personnaliser pour y inclure notre nom, notre avatar et les liens sociaux.
  4. Publier notre premier post de blog ! 

### 1. Forkez un Point de Départ

Nous commencerons par forker un dépôt qui a suivi les bonnes pratiques et les workflows que nous couvrirons dans cet article. Ceci nous mettra sur la bonne piste pour gagner du temps.

J’ai préparé un dépôt. Rendez-vous sur [Jekyll Now](https://github.com/ChristopheDucamp/jekyll-now), et pressez le bouton “Fork” dans le coin tout en haut et à droite du dépôt pour forker une copie du thème de blog vers votre compte GitHub.

*(image gif animé à vérifier et localiser)*
<figure>![étape 1](assets/images/step1.gif)<figcaption>Parcours des étapes 1 et 2.</figcaption></figure>

Démarrer par un fork est génial parce que cela vous donne une première impression de ce à quoi peut ressembler Jekyll. Et ce, avant de devoir l’installer pour un environnement de développement local, installer et configurer les dépendances et imaginer le processus de construction de Jekyll.

**Problème connu #1** : Créer un site web Jekyll à la ligne de commande peut être frustrant et chronophage parce que vous devez **installer et configurer des dépendances** comme Ruby et RubyGems. Laissons le soin à GitHub Pages de construire le site pour vous, à moins que vous n’ayez un vrai besoin de le construire localement.

### 2. Hébergez sur votre Compte GitHub

En tant qu’utilisateur GitHub, vous avez droit à un site web "utilisateur" gratuit (à l’opposé d’un site web "projet"), qui vivra sur `http://votrenomutilisateur.github.io`. Cet espace est idéal pour héberger un blog Jekyll ! 

Le truc le plus savoureux est que vous pouvez simplement placer votre blog Jekyll non construit sur la branche `master` de votre repository utilisateur, et GitHub Pages vous construira et servira le site web statique. Vous n’aurez même pas à vous soucier du processus de construction — tout est pris en charge.

Cliquez sur le bouton “Settings” dans votre nouveau repository forké (dans le menu sur la droite), et changez le nom du repository en `votrenomutilisateur.github.io`, en remplaçant `votrenomutilisateur` par votre nom d’utilisateur GitHub.

Votre site sera probablement déjà en vie. Vous pouvez vérifier en allant sur `http://votrenomutilisateur.github.io`. Pas de panique s’il n’est pas déjà live : l’étape 3 le forcera à se construire.

![Le thème de base de votre blog ressemblera à ça après avoir été forké.](assets/images/jekyll-now-by-Barry-Clark.png)] Le thème de base de votre blog ressemblera à ça après avoir été forké. (Image : [Jekyll Now](http://christopheducamp.github.io/jekyll-now/Hello-World/)) 

Wôw ! Nous avançons vite ici. Nous disposons déjà d’un site web Jekyll en marche et fonctionnel ! Revenons en arrière une seconde et regardons les principaux problèmes à prendre en compte au moment d’héberger un site web Jekyll sur GitHub Pages.

**Problème connu #2** : Soyez conscients de la différence entre les [sites web d’utilisateurs et les sites web de projets](https://help.github.com/articles/user-organization-and-project-pages) sur GitHub. Avec un site web d’utilisateur (ce que nous allons installer), vous n’aurez pas besoin de faire quelque branchement. La branche `master` est déjà configurée pour faire tourner tout ce qui est dessus en passant par Jekyll. Vous n’aurez pas à paramétrer une branche `gh-pages`.

**Problème commun #3** : Utiliser un site web de projet pour votre site web Jekyll [présente quelque complexité](http://jekyllrb.com/docs/github-pages/#project_page_url_structure) parce que votre site web sera publié sur un sous-répertoire. L’URL ressemblera à quelque chose comme `http://votrenom.github.io/nom-repository`, ce qui provoquera des problèmes dans les templates Jekyll, comme casser les références vers les images et le fait de ne pas pouvoir prévisualiser le site web localement.

**Problème commun #4** : Jekyll a [beaucoup de plug-ins](http://jekyllrb.com/docs/plugins/), mais GitHub Pages n’en supporte que [quelques-uns](https://help.github.com/articles/using-jekyll-plugins-with-github-pages). Si vous essayez d’inclure un plug-in non supporté, alors Jekyll échouera durant la construction de votre site web. Aussi, tenez-vous à la liste des plug-ins supportés. Heureusement, mon plug-in favori est sur la liste : [Jemoji](https://github.com/jekyll/jemoji) qui vous laisse inclure des emoji dans les posts de blog, tout comme ce que vous faites sur GitHub et Basecamp.

### 3. Personnalisez votre Site Web

Vous pouvez maintenant modifier le nom de votre site web, sa description, l'avatar et d'autres options en éditant le fichier  `_config.yml`. Ces variables personnalisées ont été réglées par commodité et sont extraites à l’intérieur de votre thème quand votre site se construit.

Produire une modification sur `_config.yml` (ou tout fichier présent dans le repository) forcera GitHub Pages à reconstruire votre site web avec Jekyll. Le site web reconstruit sera visible quelques secondes plus tard sur `http://votrenomutilisateur.github.io`. Si votre site web n’était pas live immédiatement après l’étape 2, il le sera après cette étape.

Lancez-vous et personnalisez votre site web en mettant à jour les variables de votre fichier `_config.yml`, et engagez les modifications.

Vous pouvez changer vos fichiers de blog à partir d’ici de ces trois manières. Choisissez celle qui vous conviendra le mieux : 

  * Éditez les fichiers dans votre nouveau repository `nomutilisateur.github.io` directement dans le navigateur sur GitHub.com (comme affiché ci-dessous).
  * Utilisez un éditeur de contenu GitHub tiers, comme [Prose](http://prose.io) de Development Seed. Il est optimisé pour Jekyll, ce qui facilite l’édition en Markdown, l’écriture de brouillons et le téléversement d’images.
  * Clonez votre repo et produisez vos mises à jour en local, puis poussez-les vers votre repository GitHub ([Atlassian a un guide](https://www.atlassian.com/git/tutorial/git-basics) là-dessus).

[![Éditez le _config.yml de votre site web GitHub.com](http://www.smashingmagazine.com/wp-content/uploads/2014/07/config-500-opt.png)](http://www.smashingmagazine.com/wp-content/uploads/2014/07/config-large-opt.png)24Éditez le _config.yml de votre site web GitHub.com. (Image credit: [Jekyll Now](http://github.com/barryclark/jekyll-now)5125159) ([View large version](http://www.smashingmagazine.com/wp-content/uploads/2014/07/config-large-opt.png)26)

**Problème commun #5** : Ne supposez pas que vous devez construire localement le site web avec `jekyll build` afin de personnaliser et construire le thème d’un site Jekyll. GitHub Pages fait ça pour vous. Vous devez simplement placer les fichiers que vous voulez construire dans la branche master de votre repo utilisateur ou dans la branche `gh-pages` de tout autre repo, ensuite GitHub Pages traitera tout cela avec Jekyll.

### 4. Publiez votre Premier Billet de Blog

Votre site web est désormais personnalisé, en live et fait bonne figure. Vous n’avez plus qu’à publier votre premier post :

  1. Éditez `/_posts/2014-3-3-Hello-World.md`, en effaçant le contenu placeholder et en entrant le votre. Si vous avez besoin d’une aide rapide sur l’écriture en Markdown, regardez l’[anti-sèche d’Adam Pritchard](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet).
  2. Modifiez le nom de fichier pour y inclure la date du jour et le titre de votre post. Jekyll requiert que les post soient nommés sous la forme `année-mois-jour-titre.md`.
  3. Mettez à jour le titre en haut du fichier Markdown. Ces variables en haut du billet de blog sont appelées front matter, nous y reviendrons plus à fond plus tard. Dans ce cas, elles spécifient quel layout utiliser et le titre du billet de blog. Des [variables supplémentaires de front-matter](http://jekyllrb.com/docs/frontmatter/) existent, comme les `permalink`, `tags` et `category`.

Si vous souhaitez créer de nouveaux billets de blog dans le navigateur sur GitHub.com, pressez simplement l’icône “+” dans `/_posts/`. Souvenez-vous simplement de mettre en forme le nom de fichier correctement et d’y inclure le bloc front-matter de manière à ce que le fichier soit traité par Jekyll.

[![Créer un nouveau post sur GitHub.com](http://www.smashingmagazine.com/wp-content/uploads/2014/07/newpost-500-opt.png)](http://www.smashingmagazine.com/wp-content/uploads/2014/07/newpost-large-opt.png)29Creating a new post on GitHub.com. ([View large version](http://www.smashingmagazine.com/wp-content/uploads/2014/07/newpost-large-opt.png)30)

**Problème commun #6**: Un problème que j’ai rencontré en utilisant Jekyll pour mon blog était qu’il n’y avait pas de CMS, aussi je ne pouvais pas facilement produire des modifications rapides en me connectant à une interface de CMS quand j’étais loin de mon bureau. Il apparaît que votre blog Jekyll aura un CMS si vous utilisez GitHub Pages, parce que GitHub lui-même fait office de CMS. Vous pouvez éditer les posts dans le navigateur, et même sur votre téléphone. Ce pourrait ne pas être aussi pratique que d’autres CMS, mais vous ne serez pas coincés quand vous serez loin de votre ordinateur pour faire une modification.

### Étapes Facultatives
#### Utilisez Votre Propre Domaine

Configurer votre nom de domaine pour le pointer vers GitHub Pages est un processus simple en deux-étapes :

  1. Allez à la racine du repository de votre blog, et éditez le fichier CNAME pour y placer votre nom de domaine (par exemple, `www.votredomaine.com`).
  2. Allez chez votre registrar de nom de domaine, et ajoutez un enregistrement CNAME DNS pointant votre domaine vers les Pages GiHub : 
    * type: `CNAME`
    * host: `www.votrenomdedomaine.com`
    * answer: `votrenomutilisateur.github.io`
    * TTL: `300`

Puis, rafraîchissez [What’s My DNS](https://www.whatsmydns.net/) comme un fou jusqu’à la propagation. Si vous rencontrez quelques proplèmes, référez vous à la page “[Personnaliser un Nom de Domaine avec les Pages GitHub](https://help.github.com/articles/setting-up-a-custom-domain-with-pages).”

#### Importer Vos Posts de Blog à partir de WordPress

Avant d’importer, vous devrez exporter vos données à partir de WordPress, en tripotant un peu les data (par exemple, en mettant à jour les références d’image) et puis en l’important à l’intérieur de votre nouveau site web Jekyll. Heureusement, quelques outils géniaux peuvent vous aider.

Pour **exporter à partir de WordPress**, je recommande chaudement le plugin [Exportateur en un clic WordPress vers Jekyll](https://github.com/benbalter/wordpress-to-jekyll-exporter) de Ben Balter. Il exporte tout votre contenu WordPress sous forme de fichier ZIP, y compris les posts, images et métadonnées, pour le convertir dans le format Jekyll. Merci Ben.

L’autre option est d’exporter tout le contenu dans le menu "Outils" du tableau de bord WordPress, puis d’importer le tout avec l’[importateur de Jekyll](http://import.jekyllrb.com/docs/wordpress/).

Puis, nous avons besoin de **mettre à jour nos références d’images**. Le plugin de Ben Balter exportera toutes vos images à l’intérieur d’un répertoire. Puis, vous devrez les copiez là où vous allez héberger vos images sur votre blog Jekyll. Ceci pourrait se faire dans un répertoire `/images` ou sur un réseau de distribution de contenus.

Puis, vous aurez la tâche amusante de mettre à jour tous les liens vers ces images dans votre contenu WordPress. Parce que je n’ai mis à jour que cinq ou six posts, un rapide trouver-remplacer a bien fonctionné, mais si vous avez beaucoup de contenus, ça peut valoir la peine d’écrire un programme ou de regarder les scripts que d’autres ont écrits, tels que celui de [Paul Stamatiou](https://gist.github.com/stammy/790971).

Pour finir, nous devons **importer les commentaires**. Jekyll étant une plateforme de sites web statiques ne supporte pas les commentaires. Néanmoins, une solution hébergée comme Disqus fonctionne vraiment bien ! Je recommanderais d’[importer vos commentaires WordPress vers Disqus](http://help.disqus.com/customer/portal/articles/466255-importing-comments-from-wordpress). Ensuite, si vous utilisez Jekyll Now, vous pouvez entrer votre nom d'utilisateur Disqus dans  `_config.yml` et vous êtes prêt.

#### Bloguez Localement dans votre Éditeur Favori

Si vous préférez écrire dans Sublime, Vim, Atom ou tout autre éditeur, tout ce que vous devrez faire c’est cloner votre repository, créer de nouveaux posts Markdown dans le répertoire `_posts`, puis pousser les modifications vers GitHub. Les Pages GitHub reconstruiront automatiquement votre site web dès que vos fichiers Markdown touche le repository, et votre nouveau billet de blog sera live dès que le build sera achevé.

  1. Premièrement, `git clone git@github.com:votrenomutilisateur/votrenomutilisateur.github.io.git`, ou clonez votre repository en utilisant [GitHub Mac](https://mac.github.com/).
  2. Créez un nouveau post dans le répertoire `_posts`. Souvenez-vous de le nommer dans le format `année-mois-jour-titre.md`, et placez le front matter tout en haut du post.
  3. Committez le fichier du post Markdown, et poussez le vers votre repository sur GitHub. ([Le guide Atlassian des fondamentaux Git](https://www.atlassian.com/git/tutorial/git-basics) pourrait s’avérer pratique.)
  4. C’est tout ! Attendez que Pages GitHub reconstruise votre site. Ceci dure généralement 10 secondes, en supposant que vous n’ayez pas une quantité énorme de contenu.

**Problème commun #7** : De nouveau, vous n’avez pas besoin de construire localement votre site web Jekyll afin d’écrire un post de blog localement et de le publier sur votre site web. Vous pouvez écrire directement en local le post Markdown et le pousser avec n’importe quelles images que vous avez utilisées, puis Pages GitHub reconstruira le site directement sur le serveur.

### Produire un Thème Jekyll

Vous voulez personnaliser votre thème ? Voici quelques trucs à connaître.

#### Construire Votre Site Web Localement

Si votre développement de thème est plus substantiel, alors construire et tester votre site web Jekyll localement peut s’avérer pratique. Cette étape n’est pas requise pour le développement de thème. Vous pourriez juste pousser les modifications du thème vers votre repository, et GitHub Pages fera le build pour vous. Néanmoins, disposer d’un aperçu et d’une prévisualisation locale du site sera utile si vous créez un thème personnalisé.

Tout d’abord, vous devrez instaler Jekyll et ses dépendances. Faites tourner la ligne de commande `gem install jekyll`, puis `gem install jemoji jekyll-sitemap`. (Vous aurez besoin d’installer [Ruby](http://www.ruby-lang.org/), [RubyGems](http://rubygems.org/) et [Kramdown](https://github.com/gettalong/kramdown). Jekyll a une [liste complète des dépendances](https://github.com/jekyll/jekyll)42.)

Voici le workflow pour construire et prévisualiser localement votre site web : 

  1. Premièrement, `cd` à l’intérieur du répertoire où votre site web Jekyll est stocké.
  2. Puis, `jekyll serve --watch`. (Jekyll explique [plus d’options build](http://jekyllrb.com/docs/usage/).)
  3. Regardez votre site web sur l’url : `http://0.0.0.0:4000`.
  4. Une fois terminé, committez les modifications et poussez le tout vers la branche master de votre site web utilisateur GitHub. Pages GitHub reconstruira et servira alors le site web.

**Problème Commun #8** : Ayez à l’esprit que `jekyll build` effacera tout dans `/_sites/`. La première étape de `jekyll build` est d’effacer tout dans `/_sites/` et de tout reconstruire à partir de zéro. Aussi, ne stockez pas de fichiers là. Tout ce qui va dans `/_sites/` devrait être généré par Jekyll.

#### Structure de Répertoire

Voici un aperçu de la structure du répertoire de mon site web Jekyll : 
    
    /Users/barryclark/Code/jekyll-now
    ├─ CNAME # Contient votre nom de domaine personnalisé (facultatif)
    ├─ _config.yml # les réglages de configuration de Jekyll
    ├─ _includes # Fragments de code qui peuvent s'utiliser dans vos templates
    │  ├─ analytics.html
    │  └─ disqus.html
    ├─ _layouts 
    │  ├─ default.html # Le template principal. Comprend <head>, <navigation>, <footer>, etc
    │  ├─ page.html # Layout de page statique
    │  └─ post.html # Layout de billet de blog
    ├─ _posts # Tous les posts vont dans ce répertoire !
    │  └─ 2014-3-3-Hello-World.md
    ├─ _site # Après que Jekyll ait construit le site web, il place ici la production des fichiers HTML statiques. C’est ce qui est servi !
    │  ├─ CNAME
    │  ├─ LICENSE
    │  ├─ about.html
    │  ├─ feed.xml
    │  ├─ index.html
    │  ├─ sitemap.xml
    │  └─ style.css
    ├─ about.md # Une page "About" statique que j’ai créée.
    ├─ feed.xml # Motorise le fil RSS
    ├─ images # Toutes mes images sont stockées là.
    │  ├── premier-post.jpg
    ├─ index.html # Layout Page d’accueil
    ├─ scss # Les feuilles de style SAAS pour mon site web
    │  ├─ _highlights.scss
    │  ├─ _reset.scss
    │  ├─ _variables.scss
    │  └─ style.scss
    └── sitemap.xml # la carte du site pour le site web
    

#### Thème Liquid

Jekyll utilise le langage de thèmeLiquid pour traiter les gabarits. Il y a deux choses importantes à savoir concernant l’usage de Liquid.

Tout d’abord, un bloc **YAML front-matter** se trouve au début de chaque fichier de contenu. Il spécifie le layout de la page et d’autres variables, comme `title`, `date` et `tags`. Il peut inclure aussi des variables de pages que vous avez créées.

Les **Liquid template tags** sont utilisés pour exécuter des boucles et déclarations conditionnelles et pour produire le contenu.

Par exemple, chaque post de blog utilise le layout suivant tiré de `/_layouts/post.html`:
    
    ---
    layout: default
    ---
    
    <article class="post">
    
      <h1>{{ page.title }}</h1>
    
      <div class="entry">
        {{ content }}
      </div>
    
      <div class="date">
		Écrit le {{ page.date | date: "%B %e, %Y" }}
      </div>
    
      <div class="comments">
        {% include disqus.html disqus_identifier=page.disqus_identifier %}
      </div>
    </article>
    

En haut du fichier, le front matter YAML est entouré de trois tirets. Ici, nous spécifions que ce fichier devrait être traité comme le contenu pour le layout `default.html`, qui contient le header et le footer du site web.

Le marquage Liquid utilise les doubles accolades pour produire le contenu. Les premières balises de template Liquid qui font ça dans l’exemple du dessus sont `{{ page.title }}` et `{{ content }}`, qui produisent le titre et le contenu du post de blog. Un certain nombre de [variables Jekyll](http://jekyllrb.com/docs/variables/) sont disponibles pour l’outpout dans les gabarits.

Les accolades simples et modules sont utilisés pour les conditionnelles et les boucles et l’affichage des inclusions. Ici, j’inclus un système de commentaires partiel en bas du billet de blog, qui affichera le marquage extrait de `includes/disqus.html`.

#### _config.yml

Ceci est votre fichier de configuration Jekyll, contenant tous les [réglages pour votre site web Jekyll](http://jekyllrb.com/docs/configuration/). Le truc génial concernant `_config.yml` est que vous pouvez aussi spécifier là toutes vos propres variables à extraire via les fichiers de gabarit sur l'ensemble du site web.

Par exemple, j'utilise les variables personnalisées dans le `_config.yml` de Jekyll Now pour permettre aux icônes SVG d'être facilement incluses dans le footer : 

Voici `_config.yml`:
    
    # Includes an icon in the footer for each user name you enter
    footer-links:
      github: barryclark
      twitter: baznyc
    

Et voici `/_layouts/default.html`:
    
    <footer class="footer">
      {% if site.footer-links.github %}<a href="http://github.com/{{ site.footer-links.github }}">{% include svg-icons/github.html %}</a>{% endif %}
      {% if site.footer-links.twitter %}<a href="http://twitter.com/{{ site.footer-links.twitter }}">{% include svg-icons/twitter.html %}</a>{% endif %}
    </footer>
    

![Exemple d’icônes de footer en SVG](http://www.smashingmagazine.com/wp-content/uploads/2014/07/footer-icons-opt.png)Exemple d’icônes de footer en SVG

La variable est produite vers l’URL de twitter comme ceci `http://twitter.com/{{ site.footer-links.twitter }}`, de manière à ce que l’icône dans le footer fasse un lien vers votre profil Twitter. Une autre chose que j’aime vraiment à propos des variables est que vous pouvez les utiliser pour y ajouter facultativement des morceaux d’UI. Ainsi, une icône de pied de page ne s’affichera pas si vous n’entrez rien dans la variable.

**Problème Commun #9** : Remarquez que les modifications de `_config.yml` sont chargées au moment de la compilation, pas en runtime. Ceci signifie que si vous faites tourner localement `jekyll serve` et que vous éditez `_config.yml`, alors les modifications ne seront pas détectées. Vous devrez killer et refaire tourner `jekyll serve`.

#### Layouts et Pages Statiques

La plupart des sites web de type blog n’ont besoin que de deux fichiers de layout : l’un pour les posts de blog (`post.html`) et un pour les pages statiques (`page.html`). La seule véritable différence entre ces layouts dans Jekyll Now est que `post.html` inclut le bloc de commentaires Disqus et une date date, et `page.html` n’en a pas.

Si vous créez un fichier avec une extension `.html` ou `.md` à la racine de votre site web Jekyll, il sera traité comme une page statique. Par exemple, `about.md` serait produit sous `www.monsite.com/about`. Élégant et facile !

#### Images

Je stocke mes images dans un répertoire `/images/` dans le et n’ai pas encore connu quelque problème de performance. Si votre site web est hébergé sur GitHub Pages, alors les images seront servies par le réseau de contenu super rapide de GitHub. Je n’ai pas trouvé le besoin de les stocker ailleurs à cette heure, mais si je voulais migrer mes images vers un endroit comme CloudFront, alors le fait de pointer simplement tous mes liens images vers un répertoire sur ce serveur serait suffisamment aisé.

J’aime la simplicité de sauvegarder une image vers le dossier `/images/` et puis de faire un lien vers elle dans le contenu. Le Markdown pour inclure une image dans le contenu est simple :
    
    ![Description Image](/images/my-image.jpg)
    

#### Support Préprocesseur

Jekyll supporte désormais Sass et CoffeeScript sans quelque besoin de plug-ins ou de fichiers Grunt. Vous pouvez inclure vos fichiers `.sass`, `.scss` et `.coffee` n’importe où dans votre répertoire de site web, et Jekyll les traitera, en produisant un fichier `.css` dans ce même répertoire.

![It's Sass time!](http://www.smashingmagazine.com/wp-content/uploads/2014/07/sass.svg)It’s Sass time ! (Crédit Image : [Sass](http://sass-lang.com/))

Pour vous assurer que vos fichiers `.sass`, `.scss` et `.coffee` soient traités, démarrez le fichier avec deux lignes de trois tirets, comme ceci :

    ---
    ---
    $color-coffee: #644C37;
    

Si vous utilisez `@imports` pour diviser votre Sass en partiels, alors vous devrez faire savoir cela à Jekyll en ajoutant le marquage qui suit dans votre fichier `_config.yml` file.
    
    sass:
      sass_dir: _scss
    

Vous pouvez aussi spécifier ici le style produit ici :
    
    sass:
      sass_dir: _scss
      style: :compressed
    

### Fonctionnalités Avancées Jekyll

Jekyll a quelques fonctionnalités puissantes, plus avancées qui pourraient vous aider si vous souhaitez faire évoluer votre site web à partir d’un simple blog.

#### Fichiers Data

Il y a deux façons d’intégrer des données externes avec un site web Jekyll. La première se fait en utilisant des services tiers et des APIs. Par exemple, Disqus vous permet d’embarquer du contenu dynamique sur votre site web statique via son service externe.

Le second moyen se fait en utilisant des fichiers de data. Jekyll peut lire les [fichiers de data](http://jekyllrb.com/docs/datafiles/) YAML et JSON à partir du répertoire `/_data/`, vous permettant de les utiliser dans vos gabarits tout comme avec d’autres variables. Ceci est vraiment utile pour stocker des data répétitives ou spécifiques au site web que vous ne voudriez pas bulking up `_config.yml`.

Les fichiers Data rendent aussi possible l’inclusion de gros ensembles de data dans votre site web Jekyll. Vous pourriez écrire un script pour extraire un jeu de données à l’intérieur de fichiers JSON et les placer dans votre répertoire de site web Jekyll `/_data/` folder. Un exemple créatif pour ça est de [servir les data de Google Analytics vers Jekyll](http://developmentseed.org/blog/google-analytics-jekyll-plugin/) afin de classer les posts de blog par ordre de popularité.

#### Collections

Les deux types de documents standards dans Jekyll sont les posts (pour le contenu de blog) et les pages (pour les pages statiques). Le lancement de Jekyll 2.0 a amené les [collections](http://jekyllrb.com/docs/collections/), qui vous permettent de définir vos propres types de documents. Par exemple, vous pourriez utiliser une collection pour définir un album photos, un livre ou un portfolio.

### Conclusion

Jekyll n’est pas conçu pour tous les projets. Le plus gros inconvénient d’un générateur de site web statique est que l'incorporation de fonctionnalités dynamiques côté serveur devient difficile. Le nombre de services à intégrer, comme Disqus, va croissant, mais tous n’offrent pas tant de flexibilité et de contrôle. Jekyll n’est pas conçu pour construire des sites web orientés-utilisateur parce qu’il n’y a pas de base de données ou de logique côté serveur pour gérer l’enregistrement utilisateur ou la connexion.

La force de Jekyll, c’est sa simplicité et son minimalisme, vous donnant juste ce dont vous avez besoin pour créer un site web concentré sur le contenu n’ayant pas besoin d’interaction-utilisateur dynamique. Il est donc parfait pour votre blog et votre portfolio et vaut aussi la peine d’être considéré pour un site web simple de client.

Ne soyez pas dissuadés par la réputation de Jekyll comme une plateforme de blogging pour les hackers. Construire un site web minimal, magnifique et rapide dans Jekyll ne requiert pas de talent de hacker d’élite ou de maîtrise de la ligne de commande. Comme nous l’avons vu dans la progression, vous pouvez le paramétrer en quelques minutes, vous laissant ainsi plus de temps sur votre contenu et votre design.

#### Ressources pour Aller Plus Loin 

  * [Jekyll](http://jekyllrb.com)  
Le site officiel Jekyll est la meilleure ressource pour personnaliser Jekyll et créer un site web. 
  * [Jekyll Now](http://github.com/christopheducamp/jekyll-now)  
Ce point de départ facilite la création d’un blog Jekyll en éliminant beaucoup de réglages up-front.
  * [Le code source Jekyll](http://github.com/jekyll/jekyll) 
Le repository Jekyll partage le code source et les discussions concernant Jekyll.
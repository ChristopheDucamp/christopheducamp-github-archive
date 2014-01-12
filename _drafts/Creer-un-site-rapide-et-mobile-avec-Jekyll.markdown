---
layout: post
title:  "Créer un Site Rapide et Mobile avec Jekyll"
date:   à venir...

categories: Jekyll
---

Étude en cours. Seul le lien original fait référence.<span class="h-cite"><cite class="pname u-url">[Creating a fast and mobile-friendly website with Jekyll](http://nicolashery.com/fast-mobile-friendly-website-with-jekyll/)</cite> (<span class="h-card p-author">Nicolas Hery</span>, <time class="dt-published">2013-06-21</time>)</span>


*Ceci est un parcours complet expliquant comment j'ai créé un site web statique avec Jekyll, en le maintenant rapide et compatible mobile.*

Jekyll is a static site generator: it transforms plain text files (in Markdown for content, Liquid for templates) into an HTML, CSS, and JavaScript website ready for deployment.

Il existe d'autres options pour créer un site web ou un blog, comme le très connu Wordpress, ou les solutions plus récentes Squarespace et Ghost. Celles-ci sont dynamiques, ce qui veut dire que votre contenu est stocké dans une base de données et rempli dans des modèles quand un utilisateur visite le site. Elles ont aussi une section d'administration, qui vous permet de vous connecter et de modifer votre contenu. Ceci fait que le site web est plus facile à mettre à jour, tout spécialement pour les personnes moins douées techniquement.

Mais utiliser un générateur de site statique présente quelques avantages sur ces autres solutions : 

* **Le site web est généralement très rapide** : Parce que ce ne sont que des fichiers statiques, il n'y a pas d'allers-retours vers une database, et les serveurs sont devenus vraiment bons pour servir des fichiers pleins.

* **Il passe aussi très facilement à l'échelle **: Même si vous avez un paquet de trafic, il est plus facile pour un serveur web statique de le gérer,  qu'une application PHP WordPress.

* **Il est très facile à déployer**: il y a beaucoup d'options de fichiers de serveur web statiques ici, vous n'avez pas besoin de hooker et configurer une base de données, ou d'installer et maintenir quelque application *(There are many static file web server options out there, you don't need to hook up and configure a database, or install and maintain any application.)*

* **Vous possédez vraiment votre contenu** : que des fichiers plein texte, généralement en Markdown, que vous pouvez maintenir en sécurité dans votre dépôt GitHub ou votre DropBox.

* **Il est extensible** : Le fait qu'un générateur statique comme Jekyll ne se concentre que sur la transformation de fichiers plein texte en HTML, et qu'il vous fournisse une interface de plugin, vous pouvez basiquement tout faire : ajouter des librairies tiers comme Disqus ou Google Analytics, créer votre propre logique pour insérer des images à l'intérieur des pages, etc.

Construire un site web en partant de zéro et en utilisant Jekyll peut paraître difficle au premier abord, car il y a beaucoup de parties à bouger et des trucs à configurer. Dans ce post, j'aimerais vous conter comment j'en j'y suis parvenu, ce qui je l'espère ceci vous aidera à démarrer plus rapidement et servira de checklist des choses auxquelles vous devrez penser. Garder à l'esprit que ce n'est juste qu'une façon de faire les choses. Vous êtes plus que bienvenus pour adapter ce workflow à vos exigences.

C'est aussi  une lecture plutôt longue, parce que j'ai vraiment voulu inclure le processus de bout en bout, de l'installation des outils jusqu'au déploiement du site. La table des matières en-dessous vous aidera à aller directement vers les différentes sections.

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

Ceci veut dire que vous devrez installer 

* [Ruby](http://www.ruby-lang.org/) avec [RubyGems](http://rubygems.org/)
* [Node.js](http://) with [NPM](https://npmjs.org/)
* [Python](http://www.python.org/) with [pip](http://www.pip-installer.org/)

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

Le site web statique est généré dans le répertoire `_site`. Tous les autres fichiers ou répertoires que ceux listés ci-dessus (comme les fichiers CSS et JavaScript) seront copiés à l'intérieur du répertoire _site, sauf si vous les excluez explicitement dans `_config.yml`.

Nous utilisons Git pour le contrôle de version, ainsi ajoutons tout de suite `_site` à notre fichier `.gitignore`.

##Installer Grunt (à étudier plus tard)

Jekyll traite notre layout et les fichiers de contenus texte en HTML. Nous utiliserons [Grunt](http://gruntjs.com/) pour traiter nos fichiers CSS et JavaScript.

Installez l'outil de ligne de commande de Grunt avec :

    $ npm install -g grunt-cli

Créons de répertoires pour nos fichiers source, et ajoutons-les à la liste exclude dans `_config.yml` afin qu'ils ne soient pas copiés sur le site :

...
├── css/
└── js/

Si vous utilisez des pré-processeurs, comme Sass, LESS ou CoffeeScript, vous pouvez ajouter les répertoires correspondants sass/, less/, ou coffee/ .

Nous créons aussi des répertoires pour nos fichiers concaténés (`debug/`) et minifiés (`build/`) qui seront copiés sur le site, et les ajouteront à `.gitignore` pour qu'ils ne soient pas suivis dans le contrôle de version.

...
├── debug/
└── build/

Pour finir, nous créons un répertoire `assets/` pour les plus grands fichiers tels que images, aussi exclu du contrôle de version dans `.gitignore` :

...
└── assets/

Un mot d'explication : nous créerons nos feuilles de styles et scripts dans leurs répertoires source, en utilisant autant de fichiers car nous aimons conserver le code maintenable. Nous pourrons alors combiner ces fichiers à l'intérieur d'un fichier CSS `style.css` et d'un fichier JavaScript main.js dans le répertoire `debug/`, qui conservera le nombre de requêtes HTTP au minimum pour de meilleures performances. Pour faire ainsi, nous pouvons utiliser les plugines Grunt suivants, selon les outils que vous utilisez (J'aime Compass pour CSS et Browserify pour JavaScript):

    grunt-contrib-concat
    grunt-contrib-sass
    grunt-contrib-compass
    grunt-contrib-less
    grunt-browserify2
    grunt-contrib-coffee

Pour le déploiement, nous minifierons auss les fichiers à l'intérieur de style.min.css et main.min.js dans le répertoire build/, en utilisant :

    grunt-contrib-cssmin
    grunt-contrib-uglify

Toutes ces têches sont définiez dans le fichier `Gruntfile.js` de votre projet. Vous pouvez regarder la documentation de Grun et mon exemple Gruntfile.js pour un premier démarrage.

## Workflow

J'ai réglé quelques tâches Grun pour m'aider à automatiser mon workflow. Durant le développement, au moment de modifier mon layout de site, d'écrire du contenu, de modifier les CSS, etc. j'ouvre une première fenêtre de terminal et lance la commande : 

    $ grunt debug

Cette tâche suit les modifications dans les fichiers CSS et JS (et/ou Sass, LESS, CoffeeScript), compile et concatène les fichiers sources dans le répertoire `debug/`. Dans une seconde fenêtre de terminal, je lance :

	$ grunt server

Cette tâche est fondamentalement un alias pour `jekyll serve --watch`, qui fait tourner le serveur de développement sur `http://localhost:4000/ ` et suit aussi les modifications dans le layout Liquid ou les fichiers de contenus Markdown, et reconstruit le site.

Avant le déploiement, je lance :

	$ grunt build

Ceci compile, concatène et minifie les styles et scripts, tout comme cela re-génère la totalité du site Jekyll, et cela utilise la variable d'environnement `JEKYLL_ENV` en la réglant sur "production" pour dire à mes templates Jekyll de pointer les fichiers minifiés dans le répertoire `build/` au lieu de `debug/`.

Nous verrons plus tard, mais pour déployer le site web et publier des modifications, j'utilise la commande : 

	$ jekyll-s3

The Grunt tasks described above use a combination of the plugins mentioned earlier. I invite you to check out my [Gruntfile.js](https://github.com/nicolashery/nicolashery.com/blob/master/Gruntfile.js) and use it as inspiration for your own workflow.
CSS

Writing plain CSS has its limits, and there are preprocessors like Sass (Ruby) and LESS (Node.js) that help you overcome them. There are also frameworks like Bootstrap (LESS) and Foundation (Sass) that give you a head start with styles for elements like grids, buttons, typography, etc.

Personally, I use Sass with Compass. I also like Inuit.css as a lightweight, extensible framework. I will explain how to set those up, but you can use whichever tool you prefer.

To install Compass and its Inuit framework plugin:

	$ gem install compass compass-inuit

I then configure the Grunt plugin grunt-contrib-compass in my Gruntfile.js and add it to my grunt debug and grunt build tasks.

Using preprocessors helps you keep your CSS maintainable and modular. If interested, you can dive into the work around Object-Oriented CSS (OOCSS) or SMACS. To start simple, I find it a good practice to separate styles specific to certain parts your website in different files, and prefix the CSS rules with those "module" names. For example:

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

Now is a good time to say a few words on building a mobile-friendly website. It has a become best practice to use [Responsive Web Design](http://en.wikipedia.org/wiki/Responsive_web_design), i.e. building a site that adapts nicely to different device screen sizes. To do so, add the following in your <head> HTML tag:

	<meta name="viewport" content="width=device-width, initial-scale=1.0">

You can then use CSS3 [media queries](https://developer.mozilla.org/en-US/docs/Web/Guide/CSS/Media_queries) to define rules for certain screen sizes. I like the [mobile-first](http://www.html5rocks.com/en/mobile/responsivedesign/) approach, so I often design my site for the smallest screen size first, then add rules to complement or override the styles as the screen gets bigger. For example:

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

Finally, include the link to the concatenated stylesheet in the <head> tag of _layouts/default.html:

{% if site.env == 'production' %}
  <link rel="stylesheet" href="/build/style.min.css">
{% else %}
  <link rel="stylesheet" href="/debug/style.css">
{% endif %}

## JavaScript

I use the [Bower](http://bower.io/) package manager to install front-end JS libraries, like [jQuery](http://jquery.com/). It has a pretty extensive registry of these front-end components. Some front-end libraries can also be found on NPM.

Bower will install libraries in the components/ directory. For example:

$ bower install jquery fastclick

I then use Browserify and the Grunt plugin grunt-browserify2 to combine these different libraries into one single JS file. My main JS file will look something like:

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

To be able to write require('jquery'), you need to use browserify-shim in your Gruntfile.js and provide the path to each component.

In your default.html layout, link to the concatenated JS file just before the closing </body> tag (for best performance):

  <!-- ... -->
  {% if site.env == 'production' %}
    <script src="/build/app.min.js"></script>
  {% else %}
    <script src="/debug/app.js"></script>
  {% endif %}
</body>

## Images

Regarding website performance, images are a tricky thing. We took the time to concatenate and minify our CSS and JS files, but a single image can weight more kilobytes than both files combined.

As Dave Rupert's article ["Ughck. Images.](http://daverupert.com/2013/06/ughck-images/)" shows, the solution to "Responsive Images" isn't quite here yet.

In the meantime, I needed something relatively simple that would allow me to include images on a website in a way that keeps it loading fast. I focused on two things:

* Lazy load images only once the user scrolls down to them, thanks to the JAIL jQuery plugin
* Responsive images that load a different version depending on the screen size and resolution, thanks to a modified version of the picturefill library (small tweak to make it compatible with JAIL)

After installing and setting up the JavaScript libraries, I create a Jekyll plugin called image_tag.rb, that allows me to easily insert images in my Markdown files with a custom Liquid tag:

{% image my-image.png "Image alt text" "Optional image caption" %}

With this in place, pages of the website should feel faster as images aren't loaded until they become visible in the browser viewport.

I chose this solution because it is simple, and my website doesn't use images much, but there are other options out there. For instance, Paul Stamatiou, who also does photograpy, offers a more sophisticated solution in his article "Developing a responsive, Retina-friendly site".


## Blocs de code

With a blog on programming, I'm going to be using a lot of code examples. It's important that they look good, both on desktop and mobile.

For syntax highlighting, Jekyll has a nice integration with [Pygments](http://nicolashery.com/fast-mobile-friendly-website-with-jekyll/). To install, run:

	$ pip install pygments

And add to your _config.yml file:

pygments: true

I like using fenced code blocks, as found in [GitHub-Flavored Markdown](https://help.github.com/articles/github-flavored-markdown), instead of Liquid highlight tags. To do so, I switched my Jekyll Markdown parser to [redcarpet](https://github.com/vmg/redcarpet):

	$ gem install redcarpet

And add to the `_config.yml` file:

	markdown: redcarpet

And now you can include code blocks with:

```javascript
var msg = 'Hello world!';
```

For me, this also has the benefit of making the Markdown files of my blog posts compatible with [Marked](http://markedapp.com/) (also check "Strip YAML Front Matter" in Marked's preferences).

As I said, I find it important that the code blocks look good on a small mobile screen as well as the desktop. To achieve this, I first use the following CSS rule:

`pre {
  white-space: pre-wrap;
}
`
This will wrap the code when a line is longer than the screen size, instead of displaying a horizontal scroll bar (I find horizontal scrolling awkward, but that might be just me). Wrapped code isn't great though, and a little difficult to read. To limit this on small screens, diminish the font-size to make as much code as possible fit on one line, using media queries:

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

Finally, for the colors of the syntax highlighting, I like the [Solarized](http://ethanschoonover.com/solarized) theme. I put together [two CSS stylesheets](https://gist.github.com/nicolashery/5765395) to use with Pygments and Jekyll, the "Light" and "Dark" versions.

Here I realized the importance of testing a website on an actual mobile device (versus just in a resized desktop browser). Indeed, the Solarized Dark theme was fine on the bright screen of my MacBook Air, but too dark and diffictult to read on my iPhone screen. That's one of the reasons I opted for Solarized Light.

## Comments

Many blogs and websites use Disqus to manage their comments. Setting it up with Jekyll is very easy.

First you need to create a Disqus account if you don't have one already, then register your site.

Once that is done, paste the universal code Disqus gives you into a Jekll include, _includes/disqus.html, and replace the shortname with a Liquid output:

var disqus_shortname = '{{ site.disqus.shortname }}'; 

Then, in your _config.yml, insert your Disqus shortname:

disqus:
    shortname: 'nicolashery'

Now, anywhere on your site you want to add Disqus comments (for instance, in _layouts/post.html), all you have to do is include disqus.html with the Liquid tag:

{% include disqus.html %}

## SEO

I'll be honest, I don't know much about Search Engine Optimization. This might be a bit naive, but to me the best way to be on top of search results is to have great content, tell people about it, and if they like it they will link to your content, and that will bring more people and help your ranking.

I did learn however, mostly thanks to Segment.io's article "The Quickest Wins in SEO", that there are a couple basic steps you should take.

First, it is good to include a short meta description in the <head> tag of your page, as Google will display that under the link to your site in the search results:

<meta name="description" content="Short description of my website that will appear in search results.">

Next, add a robots.txt file in your root directory, which tells crawlers (but doesn't force them to) which parts of the site they should index or not. Don't worry about it too much, just use the simplest form of robots.txt:

User-agent: *
Allow: /

I'm not sure if this is really SEO-related, but it's also good practice to include an error.html or 404.html file in the root directory, that will be displayed for example if a user mistypes a link to your site. Usually that page displays a short "Not Found" message with a link to other parts of the site (example: GitHub 404).

Finally, include a Sitemap, which is an XML file that helps crawlers find content to index on your site. With Jekyll, all you need to do is copy the sitemap_generator.rb plugin to your _plugins folder. Re-generate the site, and a sitemap.xml file will appear in the _site folder.

## RSS feed

An RSS feed is a good way for visitors to keep updated on new blog posts when they add it to their feed reader.

To generate the XML file for your RSS feed, just copy the [feed.xml](https://github.com/snaptortoise/jekyll-rss-feeds/blob/master/feed.xml) template to your project root directory, and add a link to /feed.xml somewhere on your site.
Analytics

I'm going to explain how to set up Google Analytics on the site, but you can adapt this to other analytics services like Mixpanel, GoSquared, etc. (for a nice list of different services out there, see the Segment.io documentation).

First, register for Google Analytics with your Google account, if you haven't done so already.

Then, add the code snippet provided in the documentation into a Jekyll include, like _includes/google_analytics.html. Replace the tracking code with a Liquid output:

_gaq.push(['_setAccount', '{{ site.google_analytics_id }}']);

You could put your Google Analytics ID in the _config.yml, but it's best not to commit these kind of tokens inside the Git repository. Instead, I set it as an environment variable in the terminal that's running the Jekyll build. In order to do this, I created a simple environment_variables.rb plugin in which I add the line:

site.config['google_analytics_id'] = ENV['GOOGLE_ANALYTICS_ID']

That way I can run:

$ export GOOGLE_ANALYTICS_ID='UA-XXXXX-Y'
$ grunt build

Which will make the environment variable's value available to the Liquid template through site.google_analytics_id.

As explained in Google's documentation, include the snippet just before the closing </head> tag with:

  <!-- ... -->
  {% include google_analytics.html %}
</head>

## Deploiement

Deploying has a few different pieces that we'll need to select:

    A host for the static files (ex: Amazon S3, GitHub Pages)
    A registrar for the domain name (ex: Gandi)
    Optionally, a separate DNS provider (ex: DNSimple, Amazon Route 53)
    Optionally, a CDN (ex: CloudFront)

As mentioned at the beginning of this post, one advantage of a static website is that it can be deployed to just about any host that can serve static files. Here I'll explain how to deploy to Amazon S3, which has a "pay-according-to-traffic" pricing model (it ends up being pretty cheap, unless you really have a ton of traffic). If you don't have many images or other big files, GitHub Pages is also a nice option.

I'm also going to explain how to host your site at the "root" (or "naked" or "apex") domain (i.e. "nicolashery.com" instead of "www.nicolashery.com"), which is a little more tricky. This is my preference, but if you'd like to host at the "www" domain, you should be able to adapt these instructions fairly easily.

Pushing a Jekyll website to Amazon S3 is really easy, thanks to the jekyll-s3 tool. Create an Amazon Web Services (AWS) account if you don't have one already, then install the tool with:

$ gem install jekyll-s3

In the Amazon S3 console, create a bucket (select the "US Standard" region) with a name that matches your domain name, (in my case "nicolashery.com").

Note: Since I'm hosting at the root domain "nicolashery.com", I will also create a second bucket "www.nicolashery.com" that I will leave empty, and configure it to redirect all requests to "nicolashery.com", as explained in the AWS documentation.

Next, create a _jekyll_s3.yml file with your Amazon credentials declared as environment variables:

	s3_id: <%= ENV['S3_ID'] %>
	s3_secret: <%= ENV['S3_SECRET'] %>
	s3_bucket: nicolashery.com
	gzip: true

Obtain your Amazon credentials from the AWS console (click on your name in the top right and "Security Credentials"), and set the environment variables:

	$ export S3_ID='YOUR_AWS_S3_ACCESS_KEY_ID'
	$ export S3_SECRET='YOUR_AWS_S3_SECRET_ACCESS_KEY'

Once that is done, run the S3 bucket configuration tool provided with jekyll-s3, which will prepare it to host a static website (for now, say "no" when it asks you if you want to configure it for the CloudFront CDN):

configure-s3-website --config-file _jekyll_s3.yml

Finally, build your site for production and deploy to Amazon S3 with:

	$ grunt build
	$ jekyll-s3

You can already visit the live site with the example.com.s3-website-us-east-1.amazonaws.com address provided (replace "example.com" with your domain of course).

To point our domain name to this Amazon S3 bucket, we need to configure its DNS records. If we were hosting at the "www.example.com" domain, all we would need to do is create a CNAME record:

CNAME www.example.com www.example.com.s3-website-us-east-1.amazonaws.com

However, we are hosting at the root domain "example.com", which requires an A record that has to point to an IP address. This is where it gets tricky: the IP address of our Amazon S3 can change.

To work around that, we need to use a separate DNS provider, DNSimple or Amazon Route 53. They both have special ALIAS records (DNSimple ALIAS, Route 53 ALIAS) that allow you to point a root domain ("example.com") to another domain, much like a CNAME.

In the case of DNSimple, we create two records:

ALIAS example.com example.com.s3-website-us-east-1.amazonaws.com
CNAME www.example.com www.example.com.s3-website-us-east-1.amazonaws.com

To use a separate DNS provider, you need to configure your registrar (mine is Gandi) to point to its DNS servers.

I won't dive right now into how to configure a CDN like CloudFront. Different CDN, DNS, host combinations could be a whole other article. Feel free to read through the CloudFront documentation, it should be pretty straighforward to set up using jekyll-s3 and Route 53.


## Conclusion

Using Jekyll does require a bit of tinkering and setting up, but hopefully this walkthrough will help you get started faster.

I like the fact that creating "from scratch" forces you to learn and understand what goes into making a fast and modern website. I also appreaciate the flexibility it gives you, since you can use plugins and other tools like Grunt to adapt it to what's specific about your project.

Did you find this article helpful? Do you use Jekyll and have a different workflow you'd like to share? Feel free to reach out!
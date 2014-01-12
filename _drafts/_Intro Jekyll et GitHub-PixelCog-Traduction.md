Jekyll À Partir de Zéro - Comment démarrer
[Lien de référence](http://pixelcog.com/blog/2013/jekyll-from-scratch-introduction/)

date • temps de lecture


Jekyll + Pages GitHub sera définitivement la plate-forme qui fera tourner mon nouveau blog indieweb 2014. Face aux difficultés rencontrer pour démarrer, j'ai décidé de documenter cette première expérience au fil de l'eau, pour tous les amateurs débutants et non programmeurs qui seraient tentés de suivre.

## Table des Matières :

    Le Blogging pour Hackers
    Intro Jekyll et Pages GitHub
        Trouver la Bonne Information
    Démarrage 
        Écrire votre Premier Billet de Blog 
        Basique sur le Moteur de Rendu
        Moteur de Template Liquid
    Publier Votre Site avec GitHub Pages
        Permettre un Nom de Domaine Personnalisé
        Autres Options de Déploiement
    Migrer un Blog Existant
    Ressources Utiles
    Conclusion Finale

## Un Blog pour les Hackers

Bien que ce ne soit pas ma première expérience de plate-forme de blog, cela faisait un bail que je rêvais d'une solution plus simple et plus indépendante, me permettant d'écrire en dehors de la fenêtre d'un navigateur web. 

N'étant pas programmeur, il me fallait aussi une solution bidouillable simplement sous le capot pour poursuivre l'étude du HTML et CSS. 

Au fur et à mesure de recherches et recommandations, les principales raisons retenues et qui m'amènent à *switcher* : 

* **Automatisme** — Pas envie de construire tout à la main. J'avais besoin d'une plate-forme qui me fournissent les fonctionnalités basiques d'un blog moderne pouvant évoluer sur l'indiemark (flux, pagination, mots-clés, gestion de contenus)

* **Contrôle des Données /Portabilité** — Avoir le contrôle complet des contenus source. Je ne veux pas publier pour exister dans un silo où vous enterrez vos contenus sans pouvoir facilement les exporter. Venant de la culture wiki, les systèmes de contrôles de versions sont inéluctablement un plus.
* **Apprendre et garder le pouvoir** — Je ne veux pas construire un site web sur une toute nouvelle plate-forme n'ayant pas encore de communauté solidement ancrée
* **Mise en Forme des Textes** — Après avoir beaucoup travaillé en syntaxe mediawiki, je découvre et utilise Markdown depuis quelques semaines, et c'est indéniablement le langage d'écriture le meilleur pour écrire rapidement, naturellement et qui semble rester inter-opérable. Assez d'être contraint à jongler entre les éditeurs WYSIWYG très imparfaits pour produire un HTML conforme. Et la production de HTML à la main dans BBEdit me fatigue.
* **Personnalisation** — Je veux pouvoir contrôler chaque morceau de HTML, ... et disposer d'un système de gabarits simple et robustes avec des variables faciles à gérer.
* **Simplicité / Passage à l'échelle** — Ayant auto-hébergé des bases de données MySQL sans rien y comprendre, I don’t want unneccesary database overhead or legacy one-size-fits-all code impeding my ability to maintain a simple, scalable hosting solution.
* **Sécurité** — N'étant pas expert en auto-hébergement, je ne souhaite pas me battre pour héberger un micro-site personnel. Je pourrais y parvenir en me battant mais ce n'est pas mon job à plein temps et j'ai suffisamment de trucs plus intéressants à étudier.

Compte tenu de ces critères, j'en suis venu à trouver quelques solutions.

La première que j'ai trouvée était Scriptogr.am. At first glance it seemed like it would fit quite well. It takes Markdown files hosted in a Dropbox folder, compiles them using a simple customized template, and hosts them for you for free. My main reservations were the absense of a monitization strategy (there’s no such thing as a free lunch), a few unfavorable anecdotes from switchers, and an apparant lack of active development (as of this writing, the last development blog update was 10 months ago in Sept 2012). Scriptogr.am has some promise, but I’d rather give it a few years to incubate and see where it ends up.

J'ai étudié aussi un projet open source appelé [Octopress](http://octopress.org/) from the folks on the [/dev/hell podcast](http://devhell.info/) and it appeared to represent nearly everything I was looking for; Markdown support, incredibly easy self-hosting, and extreme flexibility while still providing a solid framework with which to build my website. Perfect!

Deciding to give it a try I dug deeper and learned that Octopress is actually a bootstrapped layer on top of another project called Jekyll. Having heard this I decided to give vanilla Jekyll a try before dealing with any (potentially unnecessary) layer of abstraction provided by Octopress.

## Intro à Jekyll et Pages GitHub

Jekyll is a powerful static website generator. It takes raw markup files and templates and compiles them into a complete, static html website. Since it requires no server-side scripting components it can be hosted on bare bones file servers like [Amazon S3](http://aws.amazon.com/s3/) rather than elaborate server stacks with a scripting language like PHP tied to a SQL database where you need to worry about things like security and scalability.

Jekyll happens to be the engine behind ”GitHub Pages” which is easily Jekyll’s most attractive hosting solution. With it you can simply upload your Jekyll blog to a GitHub repository and have it automatically compiled and deployed through a post-receive hook each time you commit. Through this setup, publishing your blog to GitHub becomes as simple as typing git push! The service is free, has unlimited-bandwidth (AFAIK), and allows custom domains, and since I already use git for all of my professional projects, this workflow felt right at home.

The one catch to using GitHub Pages is its lack of plugin support. For obvious reasons, GitHub doesn’t want people executing arbitrary ruby code on their servers, so Jekyll is run in safe mode. This can be [worked around](http://edhedges.com/blog/2012/07/30/jekyll-with-plugins-hosted-on-github-pages/) by adding an extra build-step prior to publishing or by automating deployment with rake files, commit hooks, or shell scripts, but I have chosen to forgo plugins and go the simple route for a few reasons:

1. Using a vanilla Jekyll deployment is more portable, simple, and elegant. I’m not a fan of unnecessary complexity.
2. GitHub Pages default deployment solution requires no additional setup. I could go to any computer, clone my repo, make some changes, and push without mucking around with commit hooks, installing dependencies, and so forth.
3. Most anything you’d want to do with your blog can be done without plugins.

I have decided to keep my **blog completely plugin free** and GitHub Pages compatible, utilizing only Liquid, Markdown, and core Jekyll functions. If you’d like to work with plugins I won’t be covering any of that here, but there are [many](http://octopress.org/docs/plugins/) [other](http://www.jekyll-plugins.com/) [resources](http://jekyllrb.com/docs/deployment-methods/) out there to help you get past those logistical hurdles.


###Finding Good Information

Jekyll may be rising in popularity, but its community is still quite small, and Google is not yet good at curating its more helpful online resources. Jekyll’s own official website and docs rank below other search results on several Google queries. The project site could certainly use some good link juice from blog posts like this and others.

The project recently reached maturity with its 1.0.0 release (May 2013), bringing with it support for drafts, excerpts, and many other enhancements. Many Jekyll guides and resources, inlcluding popular child projects Octopress and Jekyll-Bootstrap, still disseminate obsolete solutions aimed at patching-in some of these enhancements through plugins. This blog post is largely a reaction to the lack of post-1.0, non-plugin-dependent resources out there for new Jekyll users.

I suspect many of the remaining plugin-based workarounds will also be made obsolete as popular features continue to get rolled into core. Tag and category pagination, more robust templating features, and Octopress-like publishing workflow automation aren’t far down the pipeline.

## Démarrage

Getting your feet wet with Jekyll is very simple. I find it best to force installation of the same versions of Jekyll and Liquid currently used on GitHub Pages to ensure feature parity (1.0.3 and 2.5.0 respectively as of July 2013). Assuming you already have ruby installed, following Jekyll’s Quick-Start Guide is as simple as this:

	$ gem update --system
	$ gem install liquid -v 2.5.0
	$ gem install jekyll -v 1.0.3
	$ jekyll new blog
	$ cd blog
	$ jekyll serve -w

Now simply go to localhost:4000 and marvel at your creation…
(crtl+c when you’re done)

    Update — Nov 22nd, 2013: GitHub Pages now has its own gemfile to install the currently used versions of Jekyll and its dependencies. You can now run gem install github-pages in place of the above install commands. More information here.

### Écrire Votre Premier Post de Blog

Looking at the shell directory structure that we just created you’ll notice a _posts subdirectory. This is where your published blog posts will live. These must all follow the naming convention YYYY-MM-DD-name-of-post.ext. The example post uses the extension *.markdown but I much prefer the sussinct *.md extension; it makes no difference either way. Jekyll will infer the date and permalink slug of your post from this name unless overridden in the YAML front-matter (the content between the lines with three dashes at the start of the file):

---
layout: post
title:  "Welcome to Jekyll!"
date:   2013-05-24 18:34:38
categories: jekyll update
---

You'll find this post [...]

Beginning with the recent release of Jekyll 1.0, we can now create and preview blog post drafts without mixing them in with published posts and conforming to this rigid file naming convention. Simply create a _drafts folder in the root directory of your blog and add a new file to it. Let’s call it hello-world.md:

---
	layout: post
---

	`# Hello World`
	This is my first _ever_ Jekyll post!

Save the file and run Jekyll with the --drafts flag to preview your work.

	$ jekyll serve -w --drafts

Go back to localhost:4000 and you should see your new post listed under today’s date. Jekyll infers the post title from the filename if none is provided, and uses the file’s last-modified date as the publication date.

Now when you’ve finished a draft and decide to publish it, the process is pretty straight forward. Move the file from _drafts to _posts, add the year formatted as YYYY-MM-DD- to the start of the filename. You may then wish to configure the front-matter to add meta-data like tags, categories, permalink, et cetera.

> Pro Tip — I like to store my blog post drafts where I can view and edit them on the go. To do this, I made my _drafts folder a symbolic link to a folder in my Dropbox. Using this method I can access my drafts from Byword on my iOS devices and update them from anywhere.

### Fondamentaux sur le Moteur de Restitution

Files in _posts are rendered according to the default permalink rule (/:categories/:year/:month/:day/:title.html) which can be overridden in _config.yml or the post’s own front-matter (like permalink: /foo/bar/).

Page templates are assigned with the layout front-matter variable and can be found in the _layouts directory. Jekyll provides two by default, default.html and post.html. Template include files are found in _includes and can be embedded with {% include file.ext %}.

All other files follow these four rules:

	1. Files and directories which begin with . or _ are ignored (unless overridden in `_config.yml`)

	2. Files without YAML front-matter are passed through when rendered.(/css/style.css or /foo.html with no front-matter are copied verbatim)
	3. Files with YAML front-matter are processed with Liquid and (if appropriate) rendered in Markdown with an appropriate file extension change. (/file.md with front-matter becomes /file.html)
	4. Files with YAML front-matter which specify a permalink are preprocessed and rendered at the given url. If the permalink is a directory, index.html is used.
		(permalink: /fancy/link/ yields /fancy/link/index.html)

Once you understand these simple rules, you’ll have a pretty firm grasp of Jekyll’s rendering engine.

### Moteur de Template Liquid Template

Going over the intricacies of the Liquid templating language is beyond the scope of this tutorial, but there are some good references [here](http://jekyllrb.com/docs/templates/) and [here](http://wiki.shopify.com/Liquid). Anyone who has used a basic templating language before will find all the usual trappings — for loops, if/else logic, variable assignment, etc. I’d encourage you to play around with it and explore the [official Jekyll documentation](https://help.github.com/articles/using-jekyll-with-pages) before proceeding with the more advanced stuff in my next posts.

## Publier Votre Site Avec GitHub Pages

Once you’ve edited your first post and played around with the templates, you’ll want to publish your work. Provided you already have git installed, this is a pretty simple process.

Following the instructions on help.github.com, you’ll need to create an empty repository named {user or org name}.github.io and then perform the following actions:

	$ git init
	$ git add -A && git commit -m "initial commit"
	$ git remote add origin git@github.com:username/username.github.io.git
	$ git push -u origin master

> remember to replace username with your user or org name

Within 10 minutes (but usualy much quicker) your Jekyll blog will be rendered and online!

### Régler un Nom de Domaine Personnalisé

If you want to enable a custom domain name on GitHub Pages, you can perform the following steps (also from help.github.com):

Go to wherever you’re domain’s DNS zone is hosted and edit your A record to point to 204.232.175.78.

Add a CNAME file to the github repo to tell it to use our domain:

$ echo "mydomain.com" >> CNAME
$ git add -A && git commit -m "enable custom domain"
$ git push

* replace mydomain.com with your domain name

Wait for the DNS zone changes to propogate (up to 48 hours, but usually much sooner), and enjoy!

### Autres Options de Déploiement

GitHub Pages may be my deployment option of choice, but it certainly isn’t the only option. As mentioned before, Amazon S3 is another good, low-cost solution. This and many other options and deployment automation techniques can be found on Jekyll’s official website.

## Migrer un Blog Existant

Jekyll has built support for importing blog data from a large number of blogging platforms including Tumblr, WordPress, Blogger, and many others. Visit Jekyll’s migration page for more info.

In my next post, I’ll cover ways to maintain urls you were using from your old blog so you don’t break all of your inbound links.
## Helpful Resources

    Jekyll Official Documentation
    Jekyll Changelog
    Jekyll Template Reference
    Liquid Engine Reference
    Liquid Filters Reference
    Markdown Reference
    GitHub Pages Help Section
    Development Seed’s Jekyll Blog — really helped me understand the basics of Jekyll’s rendering engine when I started out, and provides some really clever examples of what you can do with Jekyll.
    Sebastian Teumert’s Jekyll Template Toolkit — an indispensable plugin-free Jekyll resource.
    Tobias Sjösten’s Blog — has some nice tips and tricks for Jekyll, also plugin-free.
    StackOverflow’s Jekyll Tag — the place to go to ask questions if you get hung up.

## Conclusion 

One of the biggest boons for budding young web developers in the last few decades has been the ability to hit “view source” to instantly see what’s behind the curtain of their favorite website. For a kid who begged his parents for triangle shaped screwdrivers so that he could dissect his McDonalds toys and put them back together, I found this feature invaluable when I first cut my teeth on HTML in my early teens. It’s always easier to learn by observing real-life examples.

This is one reason why Jekyll’s pairing with GitHub Pages is so awesome. Many of the Jekyll websites out there are hosted on public GitHub repositories, free to anyone who wants to dissect them and see what makes them tick. You can view the source material for this post, and even this entire website on GitHub. One of the best ways I’ve found to learn it is to find a Jekyll blog you like and take a look at how they do things.

Go forth and start blogging like a hacker!

In [my next post](http://pixelcog.com/blog/2013/jekyll-from-scratch-core-architecture/) I’ll be covering some more advanced website structural configuration with Jekyll.
## Migrating an Existing Blog

Jekyll has built support for importing blog data from a large number of blogging platforms including Tumblr, WordPress, Blogger, and many others. Visit Jekyll’s migration page for more info.

In my next post, I’ll cover ways to maintain urls you were using from your old blog so you don’t break all of your inbound links.
Helpful Resources

    Jekyll Official Documentation
    Jekyll Changelog
    Jekyll Template Reference
    Liquid Engine Reference
    Liquid Filters Reference
    Markdown Reference
    GitHub Pages Help Section
    Development Seed’s Jekyll Blog — really helped me understand the basics of Jekyll’s rendering engine when I started out, and provides some really clever examples of what you can do with Jekyll.
    Sebastian Teumert’s Jekyll Template Toolkit — an indispensable plugin-free Jekyll resource.
    Tobias Sjösten’s Blog — has some nice tips and tricks for Jekyll, also plugin-free.
    StackOverflow’s Jekyll Tag — the place to go to ask questions if you get hung up.

Final Thoughts

One of the biggest boons for budding young web developers in the last few decades has been the ability to hit “view source” to instantly see what’s behind the curtain of their favorite website. For a kid who begged his parents for triangle shaped screwdrivers so that he could dissect his McDonalds toys and put them back together, I found this feature invaluable when I first cut my teeth on HTML in my early teens. It’s always easier to learn by observing real-life examples.

This is one reason why Jekyll’s pairing with GitHub Pages is so awesome. Many of the Jekyll websites out there are hosted on public GitHub repositories, free to anyone who wants to dissect them and see what makes them tick. You can view the source material for this post, and even this entire website on GitHub. One of the best ways I’ve found to learn it is to find a Jekyll blog you like and take a look at how they do things.

Go forth and start blogging like a hacker!

In my next post I’ll be covering some more advanced website structural configuration with Jekyll.
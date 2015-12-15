---
layout: note
title:  "Mise à jour Jekyll 3"
date: "2015-12-13"
description: "Mise à jour Jekyll 2 vers 3"
categories: jekyll, collection
tags: jekyll, collection
---
Mise à jour à partir d'une ancienne version de Jekyll ? Quelques petites choses ont changé dans la version 3.0.

Avant de nous lancer, ouvrez votre fenêtre de terminal et récupérez la dernière version de Jekyll :

{% highlight bash %}
$ gem update jekyll
{% endhighlight %}

<div class="note feature">
  <h5 markdown="1">Plongeon</h5>
  <p markdown="1">Vous voulez un nouveau site Jekyll et démarrer rapidement ? Lancez simplement 
 <code>jekyll new NOMSITE</code> pour créer un nouveau répertoire avec un site Jekyll pré-installé.</p>
</div>

### site.collections a été modifié 

Dans la version 2.x, vos itérations sur `site.collections` yielded an array with the collection
label and the collection object as the first and second items, respectively. In 3.x,
this complication has been removed and iterations now yield simply the collection object.
A simple conversion must be made in your templates:

- `collection[0]` becomes `collection.label`
- `collection[1]` becomes `collection`

When iterating over `site.collections`, ensure the above conversions are made.

### Dépendances abandonnées

We dropped a number of dependencies the Core Team felt were optional. As such, in 3.0, they must be explicitly installed and included if you use any of the features. They are:

- jekyll-paginate – Jekyll's pagination solution from days past
- jekyll-coffeescript – processing of CoffeeScript
- jekyll-gist – the `gist` Liquid tag
- pygments.rb – the Pygments highlighter
- redcarpet – the Markdown processor
- toml – an alternative to YAML for configuration files
- classifier-reborn – for `site.related_posts`

### Future posts

A seeming feature regression in 2.x, the `--future` flag was automatically _enabled_.
The future flag allows post authors to give the post a date in the future and to have
it excluded from the build until the system time is equal or after the post time.
In Jekyll 3, this has been corrected. **Now, `--future` is disabled by default.**
This means you will need to include `--future` if you want your future-dated posts to
generate when running `jekyll build` or `jekyll serve`.

### Layout metadata

Introducing: `layout`. In Jekyll 2 and below, any metadata in the layout was munged onto
the `page` variable in Liquid. This caused a lot of confusion in the way the data was
merged and some unexpected behaviour. In Jekyll 3, all layout data is accessible via `layout`
in Liquid. For example, if your layout has `class: my-layout` in its YAML front matter,
then the layout can access that via `{% raw %}{{ layout.class }}{% endraw %}`.

### Syntax highlighter a été modifié
For the first time, the default syntax highlighter has changed for the
`highlight` tag and for backtick code blocks. Instead of [Pygments.rb](https://github.com/tmm1/pygments.rb),
it's now [Rouge](http://rouge.jneen.net/). If you were using the `highlight` tag with certain
options, such as `hl_lines`, they may not be available when using Rouge. To
go back to using Pygments, set `highlighter: pygments` in your
`_config.yml` file and run `gem install pygments.rb` or add
`gem 'pygments.rb'` to your project's `Gemfile`.

_Did we miss something? Please click "Improve this page" above and add a section. Thanks!_
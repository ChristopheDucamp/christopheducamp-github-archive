---
layout: default
title:  "Les Collections !"
date:   2014-07-31
categories: jekyll collections
---



# Les collections dans Jekyll 2.0

Tout dans votre site Jekyll n'est pas un post ou une page. Vous voudrez peut-être documenter différentes méthodes dans votre projet OpenSource, l'annuaire des membres d'une équipe ou une liste d'albums de musique. Les collections vous permettent de définir un nouveau type de document qui se comporte comme le font normalement les Pages ou les Posts, mais disposent aussi de leurs propres propriétés uniques et d'un espace-nom.

## Utiliser les Collections 

### Étape 1 : Dire à Jekyll de lire dans votre collection

Ajoutez ce qui suit au fichier `_config.yml` de votre site, en remplaçant `ma_collection` avec le nom de votre collection :

{% highlight yaml %}
collections:
- ma_collection
{% endhighlight %}

Vous pouvez facultativement spécifier les métadonnées de votre collection dans votre configuration : 

{% highlight yaml %}
collections:
  ma_collection:
    foo: bar
{% endhighlight %}

### Étape 2 : Ajouter votre Contenu 

Créez un dossier correspondant (par ex. `<source>/_ma_collection`) et ajoutez-y des documents.
Le front-matter YAML est lu comme de la data s'il existe, si non, alors tout est simplement placé dans l'attribut `content` du Document.

Note : le répertoire doit être nommé de la même manière que la colleciton que vous avez définie dans votre fichier config.yml, en le faisant précéder du caractère`_`.

### Étape 3 : En option, restituez vos documents de votre collection en fichiers indépendants

If you'd like Jekyll to create a public-facing, rendered version of each document in your collection, set the `output` key to `true` in your collection metadata in your `_config.yml`:

{% highlight yaml %}
collections:
  my_collection:
    output: true
{% endhighlight %}

This will produce a file for each document in the collection.
For example, if you have `_my_collection/some_subdir/some_doc.md`,
it will be rendered using Liquid and the Markdown converter of your
choice and written out to `<dest>/my_collection/some_subdir/some_doc.html`.

As for posts with [Permalinks](../Permalinks/), document URL can be customized by setting a `permalink` metadata to the collection:

{% highlight yaml %}
collections:
  my_collection:
    output: true
    permalink: /awesome/:path/
{% endhighlight %}

For example, if you have `_my_collection/some_subdir/some_doc.md`, it will be written out to `<dest>/awesome/some_subdir/some_doc/index.html`.

<div class="mobile-side-scroller">
<table>
  <thead>
    <tr>
      <th>Variable</th>
      <th>Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>
        <p><code>collection</code></p>
      </td>
      <td>
        <p>Label of the containing collection</p>
      </td>
    </tr>
    <tr>
      <td>
        <p><code>path</code></p>
      </td>
      <td>
        <p>Path to the document relative to the collection's directory</p>
      </td>
    </tr>
    <tr>
      <td>
        <p><code>output_ext</code></p>
      </td>
      <td>
        <p>Extension of the output file</p>
      </td>
    </tr>
  </tbody>
</table>
</div>

## Liquid Attributes

### Collections

Chaque collection est accesible via la variable Liquid `site`. Par exemple, si vous voulez accéder à la collection `albums` trouvée dans `_albums`, vous utiliseriez `site.albums`. Chaque collection est en elle-même une série de documents (par ex. `site.albums` est une série de documents, tout comme `site.pages` et `site.posts`). Voir ci-dessous pour savoir comment accéder aux attributs de ces documents.

Les collections sont aussi disponibles sous `site.collections`, avec la métadonnée que vous avez spécifiée dans votre `_config.yml` (si présent) et l'information qui suit : 

<div class="mobile-side-scroller">
<table>
  <thead>
    <tr>
      <th>Variable</th>
      <th>Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>
        <p><code>label</code></p>
      </td>
      <td>
        <p>
          Le nom de votre collection, par ex. <code>ma_collection</code>.
        </p>
      </td>
    </tr>
    <tr>
      <td>
        <p><code>docs</code></p>
      </td>
      <td>
        <p>
          Une série de <a href="#documents">documents</a>.
        </p>
      </td>
    </tr>
    <tr>
      <td>
        <p><code>relative_directory</code></p>
      </td>
      <td>
        <p>
          The path to the collections's source directory, relative to the site source.
        </p>
      </td>
    </tr>
    <tr>
      <td>
        <p><code>directory</code></p>
      </td>
      <td>
        <p>
          The full path to the collections's source directory.
        </p>
      </td>
    </tr>
    <tr>
      <td>
        <p><code>output</code></p>
      </td>
      <td>
        <p>
          Whether the collection's documents will be output as individual files.
        </p>
      </td>
    </tr>
  </tbody>
</table>
</div>


### Documents

In addition to any YAML front-matter provided in the document's corresponding file, each document has the following attributes:

<div class="mobile-side-scroller">
<table>
  <thead>
    <tr>
      <th>Variable</th>
      <th>Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>
        <p><code>content</code></p>
      </td>
      <td>
        <p>
          The (unrendered) content of the document. If no YAML front-matter is provided,
          this is the entirety of the file contents. If YAML front-matter
          is used, then this is all the contents of the file after the terminating
          `---` of the front-matter.
        </p>
      </td>
    </tr>
    <tr>
      <td>
        <p><code>output</code></p>
      </td>
      <td>
        <p>
          The rendered output of the document, based on the <code>content</code>.
        </p>
      </td>
    </tr>
    <tr>
      <td>
        <p><code>path</code></p>
      </td>
      <td>
        <p>
          The full path to the document's source file.
        </p>
      </td>
    </tr>
    <tr>
      <td>
        <p><code>relative_path</code></p>
      </td>
      <td>
        <p>
          The path to the document's source file relative to the site source.
        </p>
      </td>
    </tr>
    <tr>
      <td>
        <p><code>url</code></p>
      </td>
      <td>
        <p>
          The URL of the rendered collection. The file is only written to the
          destination when the name of the collection to which it belongs is
          included in the <code>render</code> key in the site's configuration file.
        </p>
      </td>
    </tr>
  </tbody>
</table>
</div>




---
layout: docs
title: Collections
prev_section: variables
next_section: datafiles
permalink: /docs/collections/
---

<div class="note warning">
  <h5>Collections support is unstable and may change</h5>
  <p>
    This is an experimental feature and that the API may likely change until the feature stabilizes.
  </p>
</div>

Not everything is a post or a page. Maybe you want to document the various methods in your open source project, members of a team, or talks at a conference. Collections allow you to define a new type of document that behave like Pages or Posts do normally, but also have their own unique properties and namespace.



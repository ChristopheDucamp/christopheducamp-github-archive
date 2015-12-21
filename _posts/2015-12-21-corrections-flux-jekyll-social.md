---
layout: post
title: Merci @eduardoboucas<br>Just tried Jekyll-Social with tags and it rocks !   
description: "currently searching how to optimize the twitter feed"
date: 2015-12-22 00:11
tags: [jekyllsocial twitter feed optimization]
share: twitter --twitter-hashtags
---

Bonjour [Eduard](https://twitter.com/eduardoboucas) et merci.

Just forked and tried [jekyll social](https://github.com/eduardoboucas/jekyll-social) in order to localize it in French. Love the idea, bravo.

Newbie on Jekyll and Liquid syntax, I've tweaked a bit your suggested [social-feeds](https://github.com/ChristopheDucamp/christopheducamp.github.io/tree/master/social-feeds) for validation here.

### First example on twitter with hashtags

This "original post" ( [http://christopheducamp.com/2015/12/21/bernard-werber-sintresse-au-sommeil](http://christopheducamp.com/2015/12/21/bernard-werber-sintresse-au-sommeil)) posted
with `--twitter-hashtags` added in `share` property is rendering like that on the twitter copy  :

<blockquote class="twitter-tweet" lang="fr"><p lang="fr" dir="ltr">Bernard Werber s&#39;intéresse au sommeil: /2015/12/21/bernard-werber-sintresse-au-sommeil <a href="https://twitter.com/hashtag/sommeil?src=hash">#sommeil</a> <a href="https://twitter.com/hashtag/sleeptech?src=hash">#sleeptech</a> <a href="https://t.co/dikFmkxasE">https://t.co/dikFmkxasE</a></p>&mdash; Christophe Ducamp (@xtof_fr) <a href="https://twitter.com/xtof_fr/status/679047082972479489">21 Décembre 2015</a></blockquote>
<script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script>

### Optimization to be done : remove suffix url   

If you have any idea on how to remove the "suffix-url" in `twitter.xml` ? eg in the example above, the following string :

   `: /2015/12/21/bernard-werber-sintresse-au-sommeil`

(The [twitter.xml][1] is on Github)

Merci encore et bonne nuit.

## En français

Ne maîtrisant pas la syntaxe Liquid, je serais preneur de toute aide via "PR" ou autre... pour ôter ce morceau d'URL inutile et peu élégant :  
`: /2015/12/21/bernard-werber-sintresse-au-sommeil`

## Annexes :
- [repo github](https://github.com/ChristopheDucamp/christopheducamp.github.io)
- le fichier twitter.xml / source [twitter.xml sur Github](https://github.com/ChristopheDucamp/christopheducamp.github.io/blob/master/social-feeds/twitter.xml)

{% highlight html %}
---
layout: null
---
<?xml version="1.0" encoding="UTF-8"?>
<rss version="2.0" xmlns:atom="http://www.w3.org/2005/Atom">
  <channel>
    <title>{{ site.blog_title | xml_escape }}</title>
    <description>{{ site.blog_description | xml_escape }}</description>
    <link>{{ site.site_url }}{{ site.blog_url }}</link>
    <atom:link href="{{ site.url }}/feed.xml" rel="self" type="application/rss+xml" />
    <pubDate>{{ site.time | date_to_rfc822 }}</pubDate>
    <lastBuildDate>{{ site.time | date_to_rfc822 }}</lastBuildDate>
    <generator>Jekyll v{{ jekyll.version }}</generator>
    {% assign platform = 'twitter' %}

    {% for post in site.posts limit:10 %}
        {% assign share_on = post.share | downcase %}

        {% if share_on contains '--twitter-hashtags' %}
            {% assign format_parsed = site.twitter_format | replace: '@title', post.title | replace: '@url', '' | replace: '@tags', '' | size %}
            {% assign post_tags = '' %}
            {% assign space_left = site.twitter_max_length | minus: format_parsed.size %}

            {% if site.twitter_format contains '@url' %}
                {% assign space_left = space_left | minus: site.twitter_url_length %}
            {% endif %}

            {% for tag in post.tags %}
                {% capture new_tag %} #{{ tag }}{% endcapture %}
                {% assign new_space_left = space_left | minus: new_tag.size %}

                {% if new_space_left > 0 %}
                    {% assign post_tags = post_tags | append: new_tag %}
                    {% assign space_left = space_left | minus: new_tag.size %}
                {% endif %}
            {% endfor %}

            {% assign post_title = site.twitter_format | replace: '@title', post.title | replace: '@tags', post_tags | replace: '@url', post.url %}
        {% else %}
            {% assign post_title = post.title %}
        {% endif %}

        {% if share_on contains platform %}
        <item>
            <title>{{ post_title | xml_escape }}</title>
            <description>{{ post.content | xml_escape }}</description>
            <pubDate>{{ post.date | date_to_rfc822 }}</pubDate>
            <link>{{ site.url }}{{ post.url }}</link>
            <guid isPermaLink="true">{{ site.url }}{{ post.url }}</guid>
            {% for tag in post.tags %}
            <category>{{ tag | xml_escape }}</category>
            {% endfor %}
            {% for cat in post.categories %}
            <category>{{ cat | xml_escape }}</category>
            {% endfor %}
        </item>
        {% endif %}
    {% endfor %}
  </channel>
</rss>
{% endhighlight %}



[1]:	https://github.com/ChristopheDucamp/christopheducamp.github.io/blob/master/social-feeds/twitter.xml

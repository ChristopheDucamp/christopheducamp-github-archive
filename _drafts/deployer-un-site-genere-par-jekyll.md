## Déployer un Site Généré par Jekyll

[lien de référence](http://web.archive.org/web/20091223025644/http://www.taknado.com/en/2009/03/26/deploying-a-jekyll-generated-site/)

Enfin, j'ai finalement réussi à migrer ce blog de mephisto vers [jekyll](http://web.archive.org/web/20091223025644/http://github.com/mojombo/jekyll) et voulait simplement partager quelques gotchas.

La migration en elle-même s'est bien passée. J'ai utilisé des scripts postés ici pour vidanger les posts (même si j'ai remarqué par la suite qu'il existait un convertisseur mephisto intégré dans jekyll et qui n'est pas mentionné dans le README) et migrer mes commentaires vers disqus.

||Il y avait une chose néanmoins avec les URLs. J'utilise un style de permalien "pretty" qui génère des répertoires pour chaque post et y place un index.html avec le post lui-même   (comme /2009/3/26/deploying-a-jekyll-generated-site/index.html) ainsi vous pourriez avoir des URLs sans “.html” à la fin (/2009/3/26/deploying-a-jekyll-generated-site/), parce que j'avais ce type d'URL dans mephisto. Maintenant Apache vous redirige vers “/url/” si vous tapez “/url” dans ce cas. Et pour disqus “/url” et “/url/” sont deux URLs différentes et si vous aviez purgé les commentaires avec des URLs sans le slash suivant, elles ne s'afficheront pas. ||Ce problème des belles URLs est désormais réglé.

Puis une autre chose était la façon de déployer le site. J'héberge ça sur un compte partage chez dreamhost et par conséquent il serait difficile de générer le site sur le serveur. Il n'a pas pygments (qui est utilisé pour la coloration syntaxique) et j'utilisais la version edget de jekyll.

Par conséquent, j'ai ajouté le site généré (dir “_site”) au repo git, créé un clone du repo sur dreamhost, pointé le domaine vers le dir “_site” et ajouté un hook post-update vers le repo distant (hébergé sur le même compte) qui fait vraiment un “git pull” sur dreamhost après chaque “git push” de mon côté :

$ cat ~/git/blog.git/hooks/post-update 
#!/bin/sh
unset GIT_DIR && cd /home/eugenebolshakov/blog/ && git pull

Le truc important ici est “unset GIT_DIR”. post-update qui est appelé par receive-pack qui règle  GIT_DIR to ‘.’ et peut porter à confusion le “git pull”. J'ai trouvé le truc ici.

Maintenant je peux simplement ajouter un post, générer le site (||j'ai un script raccourci pour ça dans le repo avec les params que j'utilise|| **_config.yml fonctionne merveilleursement pour ça**), je fait un git push et tout est réglé. Joli !

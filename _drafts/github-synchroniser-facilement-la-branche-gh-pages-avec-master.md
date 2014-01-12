# GitHub : Synchroniser Facilement la Branche `gh-pages` avec la branche `master`


[Posté le 13 octobre 2011 par Lea Verou](http://lea.verou.me/2011/10/easily-keep-gh-pages-in-sync-with-master/)

J'ai toujours adoré la capacité de GitHub de pouvoir publier des pages pour un projet et de ne plus me soucier du serveur. Néanmoins, à chaque essai, je me battais pour maintenir à jour la branche gh-pages. Jusqu'à ce que je découvre la merveilleuse commande git `rebase`.

Généralement mon workflow github ressemble à quelque chose comme ça : 

git add .
git status // pour voir quelles sont les modfications à committer
git commit -m 'Un message descriptif de commit'
git push origin master

Désormais, quand j'utilise `gh-pages`, il n'y a plus que quelques commandes en plus que je dois utiliser après ce qui a été fait ci-dessus : 

git checkout gh-pages // aller sur la branche gh-pages
git rebase master // synchronise gh-pages avec master
git push origin gh-pages // committe les modifications
git checkout master // retourne à la branche master

Je sais que pour bon nombre d'entre vous, cela peut vous paraître désuet  (je suis une n00b github me battant avec les trucs basiques, aussi mon conseil s'adresse probablement aux autres n00bs), mais si j'avais su ça il y a quelque mois, cela m'aurait épargné des prises de tête, aussi je l'écris pour les autres qui se retrouvent comme moi il y a quelques mois.

Maintenant, si seulement je trouve un moyen facile d'automatiser ça … :)

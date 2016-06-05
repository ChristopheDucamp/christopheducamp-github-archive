---
title: Forker un Repo sur GitHub
date: '2013-12-16 14:36:00'
categories:
- github
- git
- repo
- fork
- howto
- tutoriel
share: twitter --twitter-hashtags
layout: post
slug: forker-un-repo-github
tags: []
draft: false

---
Lien de référence : <span class="h-cite"><cite class="p-name u-url">[Fork A Repo](https://help.github.com/articles/fork-a-repo)</cite></span>

Si vous vous trouvez sur cette page, je peux supposer que vous [débutez sur Git et GitHub](/2013/12/15/Github-pour-nuls-partie-1/). Ce petit guide vous fera réviser quelques fondamentaux et vous aidera à faire vos premiers pas sur GitHub.

## Contribuer sur un projet

À un certain stade, vous vous retrouvez en situation de vouloir contribuer sur le projet de quelqu'un d'autre, ou vous aimeriez utiliser le projet de quelqu'un comme point de départ pour  votre propre projet. Ceci est connu comme le "fork" ou bifurcation. Pour ce tutoriel, nous utiliserons le projet [Spoon-Knife](https://github.com/octocat/Spoon-Knife), hébergé sur GitHub.com.

### Étape 1 : Forker le dépôt "Spoon-Knife" 

Pour forker ce projet, cliquez sur le bouton "Fork" dans le dépôt GitHub.com.

![Image](https://github-images.s3.amazonaws.com/help/Bootcamp-Fork.png) 

### Étape 2: Cloner votre fork

Vous avez forké avec succès le dépôt Spoon-Knife, mais à ce stade, il n'existe que sur GitHub. 
Pour pouvoir travailler sur le projet, vous devrez le *cloner*, à savoir rapatrier une copie sur votre machine locale.

Faites tourner le code qui suit :

{% highlight bash %}
git clone https://github.com/username/Spoon-Knife.git
# Clone votre fork du dépôt à l'intérieur du répertoire en cours dans le terminal
{% endhighlight %}  
  
  
  
### Étape 3 : Configurer les remotes

Quand un dépôt est cloné, il a un *remote* par défaut appelé `origin` qui pointe vers votre fork sur GitHub, et non pas le dépôt original à partir duquel il a été forké. Pour garder une trace du dépôt original, vous devez ajouter un autre remote appelé `upstream` :

#### Plus sur les remotes

Un *remote* est un dépôt stocké sur un autre ordinateur, dans ce cas sur le serveur de GitHub. C'est une pratique standard (et aussi le défaut quand vous clonez un dépôt) de donner le nom `origin` au remote qui pointe vers votre dépôt principal hors du site (par exemple, votre dépôt GitHub).

Git supporte plusieurs remotes. Ceci s'utilise communément au moment de forker un dépôt.

{% highlight bash %}
cd Spoon-Knife
# Change le répertoire actif dans le terminal vers le nouveau répertoire fraîchement cloné "Spoon-Knife"
git remote add upstream https://github.com/octocat/Spoon-Knife.git
# Assigne le dépôt original à un remote nommé "upstream"
git fetch upstream
# Rapatrie les modifications non présentes dans votre dépôt local, sans modifier vos fichiers
{% endhighlight %}  
  


## Plein de Trucs Restent À Faire !

Vous avez forké avec succès un dépôt, mais vous avez maintenant plein d'autres choses cools à faire : 

### Pousser des commits

Une fois que vous avez produit quelques commits vers un dépôt forké et que vous voulez les pousser vers votre dépôt forké, vous faites ça de la même façon que vous le feriez avec un dépôt normal : 

{% highlight bash %}
git push origin master
# Pousse les commits vers votre dépôt remote (distant) stocké sur GitHub
{% endhighlight %}  
  
  
#### Plus sur les commits

Pensez à un *commit* comme un instantané de votre projet - code, fichiers, tout - à un instant t. Après votre premier commit, git ne sauvegardera que les fichiers qui ont été modifiés, d'où le gain d'espace.

**Attention :** git fera de son mieux pour compresser vos fichiers, mais les gros fichiers et les binaires peuvent amener un dépôt à devenir obèse et peu maniable. Essayez d'éviter de committer des choses comme des ficihers compressés (zips, rars, jars), du code compilé (fichiers object, bibliothèques, exécutables), sauvegardes de database et fichiers média (flv, psd, musique, films)


### Rentrer des modifications upstream

Si le dépôt original que vous avez forké pour votre projet est mis à jour, vous pouvez ajouter ces mises à jour à votre fork en faisant tourner la commande suivante : 

{% highlight bash %}
git fetch upstream
# Rapatrie toutes les nouvelles modifications provenant du dépôt original
git merge upstream/master
# Fusionne toutes les modifications rapatriées à l'intérieur de vos fichiers de travail
{% endhighlight %}

#### Quelle est la différence entre fetch et  pull ?

Il y a deux moyens de recevoir des commits provenant d'un dépôt distant ou d'une branche : `git fetch` et `git pull`. Bien que ces deux commandes puissent sembler similaires, il existe des différences que vous devriez comprendre :

##### Pull

{% highlight bash %}
git pull upstream master
# Tire les commits provenant de 'upstream' et les stocke dans le dépôt local
{% endhighlight %}

Quand vous utilisez `git pull`, git essaye automatiquement de faire le travail à votre place. C'est sensible au contexte, ainsi git fusionnera tous les les commits tirés (pull) à l'intérieur de la branche dans laquelle vous êtes en train de travailler. Une chose à garder à l'esprit, est que `git pull` fusionne automatiquement les commits sans vous laisser d'abord les réviser. Si vous ne gérez pas vos branches de très près, vous pourriez rencontrer des conflits fréquents.

##### Fetch & Merge

{% highlight bash %}
git fetch upstream
# Rapatrie tous les nouveaux commits provenant du dépôt original
git merge upstream/master
# Fusionne tous les commits rapatriés à l'intérieur de vos fichiers de travail
{% endhighlight %}

Quand vous lancez `git fetch`, git retrouve tous les nouveaux commits provenant du `remote` cible que vous n'avez pas et les stocke dans votre dépôt local. Néanmoins, il ne les fusionne pas dans votre branche actuelle. Ceci est tout particulièrement utile si vous avez besoin de conserver votre dépôt à jour mais travaillez sur quelque chose qui pourrait se casser si vous mettiez vos fichiers à jour. Pour intégrer les commits à l'intérieur de votre branche locale, vous utilisez `git merge`. Ceci combine les branches spécifiées et vous alerte en cas de conflits.

### Créer des branches

Brancher vous permet de construire de nouvelles fonctionnalités ou de tester de nouvelles idées, sans mettre votre projet principal en péril. Dans git, `branch`er est une sorte de bookmark qui référence le dernier commit produit dans la branche. Ceci produit des branches très petites et faciles à travailler.

##### Comment j'utilise les branches ?

Les branches sont vraiment faciles à travailler et vous épargneront beaucoup de maux de têtes, tout spécialement quand vous travaillez avec beaucoup de gens. Pour créer une branche et commencer à travailler dedans, lancez ces commandes : 

{% highlight bash %}
git branch *mabranche*
# Crée une nouvelle branche nommée "mabranche"
git checkout *mabranche*
# Transforme "mabranche" en branche active
{% endhighlight %}

Alternativement, vous pouvez utiliser le raccourci :

{% highlight bash %}
git checkout -b *mabranche*
# Crée une nouvelle branche nommée "mabranche" et la transforme en granche active
{% endhighlight %}

Pour basculer entre les branches, utilisez `git checkout`.

{% highlight bash %}
git checkout master
# Transforme "master" en branche active
git checkout *mabranche*
# Transforme "mabranche" en branche active
{% endhighlight %}

Une fois que vous avez fini de travailler sur votre branche et quand vous êtes prêt à la combiner en retour à l'intérieur de la branche `master`, utilisez `merge`.

{% highlight bash %}
git checkout master
# Fait de "master" la branche active
git merge *mabranche*
# Fusionne les commits provenant de "mabranche" à l'intérieur de "master"
git branch -d *mabranche*
# Efface la branche "mabranche"
{% endhighlight %}

**Truc** : Quand vous switchez entre les branches, les fichiers sur lesquels vous travaillez (la "copie de travail") sont mis à jour pour renvoyer les modifications dans la nouvelles branche. Si vous avez des modifications que vous n'avez pas committées, git s'assurera que vous ne les perdez pas. Git fait aussi très attention durant les fusions et les pulls pour vous assurer que vous ne perdez pas de modifications. **En cas de doute, committez tôt et committez souvent**.

### Demandes de Pull

Si vous espérez contribuez en retour sur le fork original, vous pouvez envoyer à l'auteur original une [demande de pull](https://help.github.com/articles/using-pull-requests).

### Ne plus suivre le dépôt principal

Quand vous forkez un dépôt populaire, vous pouvez vous retrouver avec beaucoup de mises à jour non voulues. Pour vous désabonner des mises à jour sur le dépôt original, cliquez sur le bouton "Unwatch" sur le **dépôt principal** et sélectionnez "Not Watching".

![image](https://github-images.s3.amazonaws.com/help/Bootcamp-Not-Watching.png "Cliquez sur Unwatch")

### Effacer votre fork

À un certain stade, vous pouvez décider de vouloir effacer votre fork. Pour effacer un fork, suivez simplement les mêmes étapes comme vous [effaceriez un dépôt normal](https://help.github.com/articles/deleting-a-repository).

### Bravo 

Vous avez désormais forké un dépôt. Que voulez-vous faire ensuite ?

* [Installer Git](/2013/12/10/installer-git/)
* [Créer un Repo](/2013/12/16/creer_un_repo_GitHub/)
* **Forker un Repo**
* [Etre Social](/2013/12/16/github-etre-social/)





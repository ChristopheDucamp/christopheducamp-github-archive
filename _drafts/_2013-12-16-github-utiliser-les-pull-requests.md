---
layout: post
title:  "Github Utiliser les Demandes de Requêtes "
date:   2013-12-16 20:00
categories: github git tutoriel howto pull pull request collaboration
tags: hack github howto versioncontrol  pull pullrequest
---
[Lien de référence](https://help.github.com/articles/using-pull-requests)

# Utiliser les Demandes de Requêtes (Pull Request)

Les demandes de requêtes vous permettent de prévenir d'autres à propos de modifications que vous avez poussées vers un dépôt GitHub. Une fois qu'une demande de pull est envoyée, les parties intéressées peuvent réviser l'ensemble des modifications, discuter de modifications potentielles, et même pousser des commits de relance si nécessaire.

Ce guide vous promènera sur le processus d'envoi d'une demande de requête hypothétique et des outils de gestion pour prendre la modification jusqu'à son aboutissement.

## Une Note Rapide sur les Modèles de Développement Collaboratif

Il existe deux modèles connus de développement collaboratif sur GitHub :
### Fork & Pull

Le modèle *Fork & Pull* permet à quiconque de forker un dépôt existant et de pousser les modifications vers son fork personnel sans obliger à ce que l'accès soit accordé au dépôt de la source. Les modifications doivent être ensuite être rapatriées dans le dépôt source par le mainteneur du projet. Ce modèle réduit la quantité de friction pour les nouveaux contributeurs et elle est populaire dans les projets open source parce qu'elle permet aux personnes de travailler indépendamment sans coordination pyramidale.

### Modèle de Dépôt Partagé

Le *Modèle de Dépôt Partagé* est plus prévalent dans les petites équipes et organisations collaborant sur des projets privés. Tout le monde a l'autorisation d'accédér en push à un dépôt unique partagé et les branches thématiques sont utilisées pour isoler les modifications.

Les requêtes Pull sont particulièremnet utilise dans le Modèle *Fork & Pull* parce qu'elles fournissent un moyen de notifier les mainteneurs de projets des modifications dans votre fork. Néanmoins, elles sont aussi utiles dans le *Modèle de Dépôt Partagé* où elles sont utilisées pour initier la révision de code et la discussion générale concernant un ensemble de modifications avant de les fusionner dans la branche principale.

## Avant de Commencer

Ce guide suppose que [vous disposez d'un compte GitHub](https://github.com/signup), que vous avez forké un dépôt existant et poussé vos modifications. Pour de l'aide sur le fork et les push de modifications, regardez l'article [Forker un Repo](http://cyberhippie.fr/news/2013/12/16/forker-un-repo-github/).

## Initier la Demande de Pull 

Dans l'exemple qui suit, **chatcodeur** a fini son travail sur un fork du dépôt Spoon-Knife d'Octocat, poussé un commit vers une branche thématique dans son fork, et aimerait que quelqu'un puisse réviser et fusionner.

Naviguez jusqu'à votre repo avec les modifications que vous voulez que quelqu'un d'autre puisse rapatrier et pressez le bouton *Pull Request*.

Switchez sur votre branche :

![image](https://github-images.s3.amazonaws.com/help/pick-your-branch.png "menu déroulant de sélection vers votre branche")

Cliquez le bouton **Compare & review**

![image](https://github-images.s3.amazonaws.com/help/pull-request-start-review-button.png "Bouton à cliquer de Demande de Pull le bouton Compare & review")

Les demandes de Publl peuvent être envoyées à partir de n'importe quelle branche ou commit mais il est recommandé qu'une branche thématique soit utilisée afin que les commits de relance puissent être poussés pour mettre à jour la demande de pull si nécessaire.

## Réviser la Demande de Pull

Après avoir démarré la révision, vous vous voyez présenté une page de révision où vous pouvez recevoir un aperçu de haut niveau de ce qui a été modifié exactement entre votre branche et la branche `master` du dépôt. Vous pouvez réviser tous les commentaires produits sur les commits, identifier quels sont les fichiers qui ont été modifiés, et recevoir une liste des contributeurs sur votre branche.

![image](https://github-images.s3.amazonaws.com/help/pull-request-review-page.png "page de révision de Demande de Pull")


## Modifier le Niveau de la Branche et le Dépôt de Destination

Par défaut, les demandes de pull sont supposées être basées sur la [branche par défaut](https://help.github.com/articles/setting-the-default-branch) du dépôt parent. Dans ce cas, le dépôt `hubot/Spoon-Knife` a été forké à parit de `octocat/Spoon-Knife` ainsi la demande de pull est supposée être basée sur la branche `master` du dépôt `octocat/Spoon-Knife`.

Dans une grande majorité des cas, les valeurs par défaut seront bonnes ; néansmoins , si quelque partie de cette information est incorrecte, vous pouvez modifier le dépôt parent et brancher à partir des listes déroulantes. Cliquez sur le bouton **Edit** tout en haut vous permet de basculer entre votre head et la base, tout en définissant les diffs entre différents points de référence. Les références peuvent comprendre : 

1. versions taguées
2. Commit SHAs
3. Noms de branches
4. Marqueurs d'historique Git (comme HEAD^1)
5. Références de temps valides (comme master@{1day})

![image](https://github-images.s3.amazonaws.com/help/pull-request-review-edit-branch.png "Pull Request editing branches")

Le moyen le plus facile de penser au niveau de la branche est ceci : la branche de base est là où vous pensez que les modifications devraient s'appliquer, la branche head est ce que vous aimeriez appliquer.

Modifier le dépôt de base modifie qui est notifié de la demande de pull. Tous ceux qui peuvent pusher vers le dépôt base recevront une notification par e-mail et verront la nouvelle demande de pull dans leurs tableaux de bord la prochaine fois qu'ils se connecteront.

Quand vous modifiez n'importe quelle partie de l'info dans le niveau branche, les aires de prévisualisation des commit et fichiers modifiés se mettront à jour pour afficher votre nouveau niveau.

## Envoyer la Demande de Pull

Quand vous êtes prêts pour proposer votre demande de pull, cliquez sur l'en-tête pour démarrer une discussion :

![image](https://github-images.s3.amazonaws.com/help/pull-request-review-create.png "Demande de Pull d'édition de branches")

Vous serez conduit vers une page de discussion où vous pouvez entrez un titre et une description optionnelle. Vous pourrez voir exactement quels commits seront inclus quand la demande de pull sera envoyée.

Une fois que vous avez entré le titre et la description, produit toutes les personnalisations nécessaires pour le niveau de commit, et révisé les commits et modifications de fichiers à envoyer, presser le bouton *Send pull request*.

![image](https://github-images.s3.amazonaws.com/help/send-pull-request.png "Pull Request send button")

La demande de pull est envoyée immédiatement. Vous êtes emmené vers la discussion principale de pull request et la page de révision.

Après l'envoi de votre demande de pull, tous les pushes produits sur votre branche seront automatiquement mis à jour avec ces commits. Ceci est utilie si vous devez faire plus de modifications.

## Gérer les Demandes de Pull

Toutes les demandes de pull envoyées ou reçues par vous sont navigables dans le tableau de bord de pull request. Les demandes de pull pour un dépôt spécifique sont aussi navigables par quiconque ayant l'accès en visitant la page *Pull Requests*.

![image](https://github-images.s3.amazonaws.com/help/repo-pull-requests.png "Open Pull Request listing")

Le tableau de bord de demande de pull et la liste du dépôt de demande de pull supportent une large gamme de contrôles de filtrages et de tris. Utilisez-les pour diminuer la liste aux requêtes pull qui vous intéressent. 

## Réviser les Modifications Proposées

Quand vous recevez une demande de pull, la premère chose à faire est de réviser l'ensemble des modifications proposées. Les demandes de pull sont étroitement intégrées avec le dépôt git sous-jacent, aussi vous pouvez voir exactement quels commits seraient fusionnés si la requête venait à être acceptée : 

![image](https://github-images.s3.amazonaws.com/help/review-commits.png "Onglet Commit review")

Vous pouvez aussi réviser les diffs cumulées de toutes les modifications de fichiers sur l'ensemble de tous les commmits.

![image](https://github-images.s3.amazonaws.com/help/review-changes.png "Onblet révision des diff")

## Discussion Pull Request

Après avoir révisé la description basique, les commits, et diffs cumulées, la personne en charge d'appliquer les modifications peut avoir des questions ou commentaires. Peut-être que le style du code ne correspond pas au guide de style du projet, ou que la modification manque de tests d'unités, ou peut-être que tout semble génial et quelque propositions sont en ordre. La vue discussion est conçue pour encourager et saisir ce type de discussion.

![image](https://github-images.s3.amazonaws.com/help/conversation.png "Conversation Pull Request")

La vue discussion démarre par le titre de la demande originale et la description, puis saisit l'activité supplémentaire pour s'afficher en ordre chronologique à partir de là. Toutes les activités suivantes sont saisies au fur et à mesure qu'elles arrivent : 

- Commentaires laissés sur la demande de requête elle-même.
- Commits supplémentaires poussés vers la branche de demande de pull.
- Fichier et notes laissés sur n'importe lesquels de commits inclus dans le niveau de la demande de pull.

Les commentaires de demandes de Pull sont compatibles Markdown, ainsi vous pouvez y embarquer des images, utiliser des blocs de texte pré-formatés, et tous les autres formats supportés par Markdown.

## Visualiser les Demandes de Pull à l'affiche depuis longtemps

Quand une fonctionnalité ou une réparation-de-bug se poursuit depuis des mois mais n'aboutit jamais ni ne meurt, vous voudrez le plus souvent jeter un oeil plus attentif pour voir ce qui empêche la demande de Pull d'être résolue. Faire qu'il soit possible de trouver des vielles demandes de Pull mais toujours actives facilite ceci.

Vous pouvez visualiser les Demands de Pull et trier celles qui datent à partir de votre page dépôt de Pull Request.

![image](https://github-images.s3.amazonaws.com/help/pull-request-longest-running.png "tri des demandes de Pull")

Une Requête Pull qui dure est celle qui existe depuis plus d'un mois et qui a eu de l'activité durant le dernier mois. Ce filtre affiche ces vieilles Demandes de Pull actives triées par durée de vie : la quantité de temps depuis la création jusqu'à l'activité la plus récente.

## En rapport

Merging a Pull Request
Closing a Pull Request
Tidying up Pull Requests

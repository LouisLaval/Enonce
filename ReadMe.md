# Scenario de kata Git

## Modèle Gitflow

<p align="center"><img src="/images/0.png"></p>

On s'appuyera sur ce schéma Gitflow pour la suite de l'exercice.  
Pour plus de détails sur le modèle Gitflow : [cliquez ici](https://datasift.github.io/gitflow/IntroducingGitFlow.html).

## Configuration
Avant toute chose il est nécessaire d'adopter la configuration de git présente sur le dépôt github suivant : [Configurations.git](https://github.com/WhyThat/Configurations.git).

Tous les scripts suivants seront à interpréter avec Git Bash.

## Initialiser les dépôts
Le but ici est de créer une situation où votre dépôt distant (habituellement github ou VSTS) est hébergé directement sur votre poste dans un dossier séparé de votre dossier de travail.

Le dossier dans lequel vous travaillerez (qui s'apparente à votre poste local) s'appellera Local, le dossier qui hébergera le dépôt distant s'appellera Remote.

### Configuration indispensable (à faire la première fois, après la configuration reste)
Cette commande permet, lors d'un pull sur l'origin, de ne pas créer de nouvelles branches dans le cas où d'autres personnes ont apporté des modifications sur le dépôt Remote.
```
git config --global pull.rebase preserve
```

Cette commande permet lors d'un merge de ne pas appliquer le fastforward (si on applique le fast-forward, revoir l'historique est compliqué), pour plus d'infos : [cliquez ici](http://tech.m6web.fr/tentative-d-explication-des-fast-forward-sous-git).
```
git config --global merge.ff false
```

Pour bien comprendre les tenants et aboutissants des commandes rebase, pull et merge, [cliquez ici](https://delicious-insights.com/fr/articles/bien-utiliser-git-merge-et-rebase/#dans-quels-cas-utiliser-merge-).

### Créer le dépôt distant Remote
```
git init --bare Remote
```

### Créer le dépôt Local à partir du dépôt Remote
```
git clone Remote Local
cd Local
echo 'Votre texte' >> ReadMe.md
git add .
git commit -m 'init'
```

![git lg](/images/1.PNG)

### Créer une branche develop (voir modèle Gitflow) et se placer dessus
```
git branch develop
git checkout develop
```
###### Note : ces 2 commandes peuvent être raccourcies en :
```
git checkout -b develop
```

* À ce stade nous venons d'initialiser les dépôts Git Local et Remote et fait le premier commit sur le dépôt local. Ce commit est nécessaire pour que le le gestionnaire du dépôt Git puisse pointer sur une branche.

## Manipulations avant de faire un commit
Créer un fichier test.txt  
Écrire dans le fichier : "fichier test"

### Quelques commandes utiles avant de faire le commit
Pour voir les différences apportées depuis le dernier commit
```
git diff
```
Utiliser pour faire des ajouts de parties isolées dans le stage (utiliser s pour diviser le code en plus petites parties encore)
```
git add -p NOM_FICHIER
```

![git add -p ReadMe.md](/images/4.PNG)

Utiliser pour voir les fichiers présents dans le stage
```
git status
```
Utiliser pour voir les parties de code modifiées présentes dans le stage
```
git diff --staged
```
Utiliser pour vider le stage
```
git reset
```

![git reset](/images/3.PNG)

Utiliser pour commiter avec le message
```
git commit -m 'message'
```
Utiliser cet alias pour voir les branches et les commits sur le projet
```
git lg
```

* Attention à ne pas oublier de faire un *git add* avant de faire un *git commit*.

## Manipulations sur une branche de feature

Les branches de feature permettent à des développeurs de travailler sur des fonctionnalités connexes à la branche de développement principal. Ces branches sont très pratiques quand plusieurs développeurs travaillent sur un même projet sur des fonctionnalités différentes.

### Créer une branche de feature et se placer dessus
```
git checkout develop
git checkout -b feature
```

### Modifier un commit précédent (exemple changer l'auteur d'un commit)
```
git rebase -i HEAD~5
```
Placer un edit à la place de pick sur le commit en question

![git rebase -i HEAD~5](/images/5.PNG)

```
git commit --amend --author="auteur <adresseMail>" -m 'message'
git rebase --continue
```

### Fusionner plusieurs commits entre eux
```
git rebase -i HEAD~3
```
Placer un squash à la place de pick sur tous les commits que vous voulez fusionner avec le commit le plus ancien

![git rebase -i HEAD~3](/images/6.PNG)

```
git rebase --continue
```

### Modifier le contenu d'un commit
```
git rebase -i HEAD~2
```
Placer un edit à la place de pick sur tous les commits dont on veut changer le contenu
```
git reset HEAD^
git add -p .
git commit -a --amend --no-edit
git rebase --continue
```

### Clore une branche de feature
```
git checkout develop
git merge --no-ff feature
```
Gérer les conflits s'il y en a
```
git branch -d feature
```

![git lg](/images/7.PNG)

* Ici, nous avons pu travailler sur une branche de feature et faire travailler sur l'historique des commits avec le rebase interactive *rebase -i*.
###### Note : il est important d'avoir un historique propre avant de le pousser sur le dépôt distant

## Manipulations en cas d'erreur de nommage de branche

### Changer le nom d'une branche
```
git branch -m develop developt
```

### Changer la branche trackée par la branche locale sur le dépôt distant
```
git branch -u origin/develop developt
git branch -u client/develop developt
```

![git branch ...](/images/9.PNG)

* En cas d'erreur, pas de panique : nous venons de voir que Git permettait de re-diriger les pointeurs sur les dépôts distants

## Manipulations sur une branche de release

Les branches de release permettent les tests des versions des applis. Après que les tests sont valides, on peut fusionner avec la branche de master et celle de velop.

### Créer une branche de release
```
git checkout develop
git checkout -b release
```

### Clore une branche de release
```
git checkout master
git merge --squash --ff release
```
Gérer les conflits s'il y en a
```
git commit -m 'message'
git tag -a v1.0 -m 'message'
git checkout develop
git merge --no-ff release
```
Gérer les conflits s'il y en a
```
git branch -d release
```

![git lg](/images/10.PNG)

### Pousser maintenant les branches develop et master sur le dépôt distant
```
git pull origin develop
```
Gérer les conflits s'il y en a
```
git push origin develop
```

![git push origin develop](/images/8.PNG)

```
git pull origin master
```
Gérer les conflits s'il y en a
```
git push origin master --follow-tags
```

* Nous avons donc appris à créer une branche de release puis la clore en la fusionnant sur les branches master et develop.

## Manipulations sur une branche de hotfix

Une branche de hotfix est créée lorsque des bugs majeurs sont à corriger sur la version de l'application présente sur la branche master.

### Créer une branche de hotfix
```
git checkout master
git checkout -b hotfix
```

### Clore une branche de hotfix
```
git checkout master
git merge --squash --ff hotfix
```
Gérer les conflits s'il y en a
```
git commit -m 'message'
git tag -a v1.0.1 -m 'message'
git checkout develop
git merge --no-ff hotfix
```
Gérer les conflits s'il y en a
```
git branch -d hotfix
```

![git lg](/images/11.PNG)

* Nous avons appris à manipuler une branche de hotfix et à la clore.

## Effacer l'avant dernier commit en date puis le récupérer ensuite
```
git rebase -i HEAD~2
```
Supprimer le commit en mettant drop devant
```
git lg
git reflog
```
Copier la référence du commit perdu
```
git cherry-pick COMMIT_REFERENCE
git lg
```

## Livrer une version d'une application chez un client (d'Econocom) en tenant compte des modifications apportées par le client sur sa propre version
Cette étape est cruciale car si le client a modifié sa version de l'application depuis la dernière livraison, il est impératif de prendre les modifications qu'il a fait.

Pour cette fin de l'exercice nous aurons besoin de simuler un dépôt appartenant à un client de l'entreprise sur lequel apparaîtront seulement les versions abouties (production) des logiciels. Ce dossier s'appellera donc Client.

### Créer le dépoôt Client
```
cd ..
git init --bare Client
```

### Ajouter le dépôt Client à votre liste de dépôts distants
```
cd Local
git remote add client ../Client
```

![git remote -v](/images/2.PNG)

### Livrer une première version de l'application
```
git push client master --follow-tags
```

Faîtes des modifications sur la version côté Local et Client

### Livrer la version en tenant compte des modifications faîtes par Client
```
git checkout master
git checkout -b livraison
git fetch client
git pull client/master
```
Gérer les conflits s'il y en a
```
git checkout master
git merge --squash --ff livraison
```
Gérer les conflits s'il y en a
```
git commit -m 'message'
git tag v2.0
git branch -D livraison
git push -f client master --follow-tags
git push origin develop
git push origin master --follow-tags
```

![git lg](/images/12.PNG)

* Bravo, vous êtes désormais un maître en Git.

## Documentation utile
[Git Protips](http://tdd.github.io/devoxx-git-protips/#/)  
[Git Protips 2](http://drive.delicious-insights.com/legacy-files/talks/blend2014-git-protips/#/)  
[Delicious Insights](https://delicious-insights.com)

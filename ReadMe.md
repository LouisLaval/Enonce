# Scenario de kata Git

## Initialiser les dépôts

### Créer un dépôt distant Remote à partir du dépôt github

![git lg](/images/1.PNG)

### Config indispensable
git config --global pull.rebase preserve  
git config --gloabl merge.ff false  
git config --global merge.commit false  

### Créer les dépôts Local et Client

### Aller sur le dépôt local
* À partir de maintenant on ne bouge plus du dépôt local  

### Ajouter les dépôts Remote et Client à votre liste de dépôts distants

![git remote -v](/images/2.PNG)

## Manipulations pour les commits

* Faire quelques changements dans votre dépôt local  

### Quelques commandes utiles avant de faire le commit
git diff  
git add -p fichier (utiliser s pour sélectionner chaque modification indépendamment)  

![git add -p ReadMe.md](/images/4.PNG)

git status  
git diff --staged  
git reset  

![git reset](/images/3.PNG)

git commit -m 'message'  
git lg  

## Manipulations sur une branche de feature

### Créer une branche de feature

### Modifier un commit précédent (exemple changer l'auteur d'un commit)

![git rebase -i HEAD~5](/images/5.PNG)

### Fusionner plusieurs commits entre eux

![git rebase -i HEAD~3](/images/6.PNG)

### Modifier le contenu d'un commit

### Clore une branche de feature

![git lg](/images/7.PNG)

### Pousser une branche sur le dépôt distant (exemple ici avec develop)

![git push origin develop](/images/8.PNG)

## Manipulations en cas d'erreur de nommage de branche

### Changer le nom d'une branche

### Changer la branche trackée par la branche locale sur le dépôt distant

![git branch ...](/images/9.PNG)

## Manipulations sur une branche de release

### Créer une branche de release

### Clore une branche de release

![git lg](/images/10.PNG)

### Pousser maintenant les branches develop et master sur le dépôt distant

### Pousser maintenant la branche master sur le dépôt client

## Manipulations sur une branche de hotfix

### Créer une branche de hotfix

### Clore une branche de hotfix

![git lg](/images/11.PNG)

## Pousser une version chez le client en tenant compte de ses modifs

![git lg](/images/12.PNG)

## Récupérer un commit effacé

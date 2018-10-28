# Commencer un projet rails dans les meilleures conditions
C'est jamais facile de créer un dossier Rails en mode Heroku compatible qui marche bien OKLM, voici donc une petite marche à suivre pour mettre tout ceci sur Heroku OKLM.

## 1. Lancement
Commencer par créer une nouvelle appli rails : 
```shell
$ rails new nom_de_mon_app
```
Puis se mettre dedans (sinon dur de travailler dessus) :
```shell
$ cd nom_de_mon_app
```

## 2. First commit
En théorie, installer une app rails initialize un git, donc pas besoin de `git init`. On va donc faire le premier commit. Si l'on veut lier son application Rails à un remote, c'est possible avec `git remote add origin blabla_nom_origine`.

```shell
git add .
git commit -m "premier commit"
```

## 3.1 Gemfile, bundle
Il faut maintenant remplacer le Gemfile de base par un Gemfile compatible Heroku, sinon le push ne passera pas. Voici mon super [Gemfile](https://github.com/felhix/cheat_sheets/blob/master/Ruby/Gemfile.rb) que j'utilise. Une fois le Gemfile changé, on peut faire le `bundle install`. Deux solutions :

### a. PostGreSQL n'est pas installé sur mon ordinateur
Pour éviter qu'il t'envoie chier avec les gems de production, il faudra lui dire de ne pas tenir compte de la prod :
```shell
$ bundle install --without production
```

### b. PostGreSQL est installé sur mon ordinateur
```shell
$ bundle install
```

## Git push heroku master
Maintenant il ne reste plus qu'à commiter, et pousser ça chez heroku.
```shell
$ git add .
$ git commit -m "Gemfile OK"
$ heroku create
$ git push heroku master
```

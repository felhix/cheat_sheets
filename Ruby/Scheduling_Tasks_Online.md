# Des programmes qui tournent en ligne
Dans ce fichier nous allons voir comment lancer des programmes en serveur via Ruby et Rails.

## Preps
Pour que cela marche, il faut :

- Avoir Rails d'installé, un compte Heroku branché. Tout est marqué [ici](http://installfest.railsbridge.org/installfest/)
- S'y connaître un petit peu en Ruby

## Installations
Nous allons voir comment brancher un job via Rails et Heroku. Tout d'abord il faut créer une app rails

```shell
$ rails new jobs-test
```

Ensuite remplacer le Gemfile par un [Gemfile qui marche](https://github.com/felhix/cheat_sheets/blob/master/Ruby/Gemfile.rb). Puis faire ses premiers push : 

```shell
$ bundle install
$ git add .
$ git commit -m "first commit"
$ heroku create nom_du_mon_app_trop_cool
$ git push heroku master
```

Voilà, nous avons une version en ligne de notre application vide. Maintenant, nous allons dire à Heroku que nous voulons utiliser la fonctionnalité du _Scheduler_, qui permet de programmer des "tasks" à intervalles régulières :

```shell
$ heroku addons:create scheduler:standard
```

## Créer la "task"
Maintenant que nous avons branché Heroku pour qu'il soit possible de créer des tâche. Puis nous verrons comment dire à Heroku d'exécuter ces tâches. Les tâches sont dans le dossier `lib/tasks`. La façon la plus simple de générer des tasks est de rentrer la ligne de commande suivante :

```shell
$ rails generate task nom_de_ma_task
```

Ainsi, un fichier `lib/tasks/nom_de_ma_task.rake` va être créé. Dans ce fichier, remplacer le contenu par :

```ruby
desc "Petite description de ma task !"
task nom_de_ma_task: :environment do
	# ici mettre le code que l'on veut run
end
```

En exécutant `$ rake --tasks`, on pourra voir dans la liste que ma task en mentionnée, et que l'on peut l'exécuter en faisant `$ rake nom_de_ma_task`.



## Articles à ce sujet

- La [doc de Heroku](https://devcenter.heroku.com/articles/scheduler) explique très bien comment faire des jobs
- Les Ruby Snack correspondants montrent comment brancher ses rake : [Ruby Snack #63](https://www.youtube.com/watch?v=u8le514tAJM), [Ruby Snack #68](https://www.youtube.com/watch?v=S5hsL7t70ls), 
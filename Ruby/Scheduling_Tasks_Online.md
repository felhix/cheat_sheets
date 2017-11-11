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

## Scheduler ma task
Une fois l'app, sur Heroku, on peut exécuter la task en faisant :
```shell
$ heroku run rake nom_de_ma_task
```

Plutôt cool, mais ce n'est pas exactement ce que l'on voulait. Il faut aller dans [My Apps](https://dashboard.heroku.com/apps), sélectionner l'app qui nous intéresse, puis dans l'onglet _Overview_, dans la partie _Installed add-ons_, cliquer sur _Heroku Scheduler_.

Puis l'interface est assez explicite pour ajouter un nouveau job ;)

## Gérer l'output
Bon c'est cool tout ça, mais comment faire pour accéder à l'output de mes tasks, si par exemple ma task est un scrappeur ? Il existe une multitude de solutions (s'envoyer des emails, enregistrer dans un spreadsheet), mais nous allons voir deux solutions qui merchent bien

### Enregistrer dans les logs de Heroku
En faisant la commande suivante :

```shell
$ heroku logs --psh scheduler
```

Il est possible de voir tous les logs de notre application. Heroku reporte notamment toutes les lignes du genre : `puts "Bonjour ceci est une ligne`. Donc pour avoir le report de ce qui nous intéresse, il suffit de faire :

```ruby
puts "Début de mon programme"
# Tout le programme tourne ici
puts "Le programme a bien tourné, voici la data qui m'intéresse :"
puts nom_de_la_variable_qui_contient_toutes_les_donnees
```

### Enregistrer dans les views de notre application
Si l'on créé un model `IScrappedThat`, avec comme attributs les datas qui nous intéressent, il est possible de créer à chaque appel de notre task une instance de notre _IScrappedThat_. Puis il suffit de faire une view index qui a comme contenu :

```ruby
def index
    @mymodels = Mymodel.all
    render json: @mymodels
end
```

Et à partir de là, je peux accéder à mon programme. En ligne via http://www.nom-de-mon-app.herokuapp.com/mymodels.


## Code plus propre

### Services
C'est mieux de mettre tout le code des tasks dans des [services](https://github.com/felhix/cheat_sheets/blob/master/Ruby/Services.md)

### Regrouper les tasks

Au cas où il y ait plusieurs tasks qui appartiennent à la même catégorie (ex: Newsletter), il est mieux de les regrouper. Ainsi, on fera un commun aux tasks.

```shell
$ rails generate task nom_de_ma_categorie nom_de_ma_task
```

Ce qui va créer un fichier `lib/tasks/nom_de_ma_category.rake`. Dedans il suffit d'y rentrer les lignes suivantes :

```ruby
namespace :nom_de_ma_categorie do
  desc "Ma super task 1 !!"
  task nom_de_ma_task: :environment do
    # vends-moi du rêve ici
  end

  desc "Ma super task 2 !!"
  task nom_de_ma_task_numero_2: :environment do
    # vends-moi du rêve ici, volume 2
  end

end

```

Pour appeler mes tasks, il suffit de faire `$ rake nom_de_ma_categorie:nom_de_ma_task`.

## Articles à ce sujet

- La [doc de Heroku](https://devcenter.heroku.com/articles/scheduler) explique très bien comment faire des jobs
- Les Ruby Snack correspondants montrent comment brancher ses rake : [Ruby Snack #63](https://www.youtube.com/watch?v=u8le514tAJM), [Ruby Snack #68](https://www.youtube.com/watch?v=S5hsL7t70ls), 
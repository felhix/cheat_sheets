# L'Asset Pipeline
## Ne pas enclencher l'asset pipeline
Pour ne pas enclencher l'asset pipeline, il faut rentrer : 
```shell
rails new appname --skip-sprockets
```

Ainsi ça va virer les gems suivantes :
```ruby
gem 'sass-rails', '~> 5.0'
gem 'uglifier', '>= 1.3.0'
gem 'coffee-rails', '~> 4.2'
gem 'turbolinks', '~> 5'
```

## Avant de partir en prod
Avant de partir en production, il est indispensable de précompiler les assets :
```shell
rails assets:precompile
```

## Remarques

### Application.scss
Le fichier `app/assets/stylesheets/application.scss` permet d'y mettre quelques lignes de CSS qui concernent toute l'application.

### Controllers
Quand on génère un controller, il créé un fichier `app/assets/stylesheets/nom_du_controller.scss` et un fichier `app/assets/javascripts/nom_du_controller.coffee` qui s'occupent du JS et CSS des controllers précis.

### Lancer le serveur en production
Pratique : 
```shell
rails server -e production
```

## Liens utiles

- [Vidéo de Grafikart](https://www.youtube.com/watch?v=tgI7_BIXcM8)
- [Doc de Rails](http://guides.rubyonrails.org/asset_pipeline.html)
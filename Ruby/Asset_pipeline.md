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

## Comment mettre des fichiers à moi là-dedans ?
Dans mon fichier `app/assets/stylesheets/application.scss`, j'ai les lignes suivantes :
```css
/*
 * Du blabla
 *= require_tree .
 *= require_self
 */
```
La ligne `*= require_tree .` indique que ce fichier prend tous les fichiers dans le dossier `stylesheets`.

La ligne `*= require_self` indique que ce fichier prend les lignes qu'il y aura dans ce fichier.


## J'ai envie de charger du CSS custom
Que faire si j'ai envie de charger mon CSS custom ? Ajouter la ligne suivante :
```ruby
<%= stylesheet_link_tag    'mon_css', media: 'all', 'data-turbolinks-track': 'reload' %>
```

Puis dans `app/assets/stylesheets`, je mets mon fichier `mon_css.css`. Dans ce fichier je mets :
```css
/*
* = require_self
*/
```
Puis à la suite j'envoie mon css.


⚠️ Tu risques de te prendre une erreur du genre `Sprockets::Rails::Helper::AssetNotPrecompiled`. Pour résoudre ceci, il faut aller dans `config/initializers/assets.rb`, puis de mettre la ligne suivante :
```ruby
Rails.application.config.asset.precompile += %w(mon_css.css)
```

## J'ai envie d'avoir des css en fonction de mon controller
Avec la ligne suivante, on peut avoir du css custom :
```ruby
<%= stylesheet_link_tag params[:controller] %>
```
Et il suffit de virer notre ami application, qui require_tree. Ne pas oublier de mettre dans le fichier `config/initializers/assets.rb` la ligne suivante :
```ruby
Rails.application.config.asset.precompile += %w(mon_css.css)
```


## J'ai envie de charger des libs qu'on m'a filés
Par exemple, un lib que j'aime bien. Pour ceci, je vais mettre dans mon fichier `application.scss` :
```css
*= require mon_fichier
```
Pour les dossiers externes à l'application, il est recommandé de prendre le dossier `lib/assets` (on le mettra dans un dossier `stylesheets` pour les css, et un dossier `javascripts` pour les scripts). Ainsi, pour faire un bon appel à mon app, je vais devoir mettre mon_fichier dans `lib/assets/stylesheets/mon_fichier.extension`.

Ne pas oublier de mettre dans le fichier `config/initializers/assets.rb` la ligne suivante :
```ruby
Rails.application.config.asset.precompile += %w(mon_css.css)
```
Puis de relancer le serveur.

## Avant de partir en prod
Avant de partir en production, il est indispensable de précompiler les assets :
```shell
rails assets:precompile
```
(on peut voir à quoi ça ressemble dans le dossier `public/assets`)

## Remarques

### Application.scss
Le fichier `app/assets/stylesheets/application.scss` permet d'y mettre quelques lignes de CSS qui concernent toute l'application.

### Controllers
Quand on génère un controller, il créé un fichier `app/assets/stylesheets/nom_du_controller.scss` et un fichier `app/assets/javascripts/nom_du_controller.coffee` qui s'occupent du JS et CSS des controllers précis.

### Les images
Pour gérer une image dans un fichier : `image-url('nom_du_fichier.extension')`, ou `<%= image_tag 'nom_du_fichier.extension', width: 100 %>`

### Lancer le serveur en production
Pratique : 
```shell
rails server -e production
```

## Liens utiles

- [Vidéo de Grafikart](https://www.youtube.com/watch?v=tgI7_BIXcM8)
- [Doc de Rails](http://guides.rubyonrails.org/asset_pipeline.html)
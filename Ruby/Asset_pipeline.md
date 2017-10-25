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


## Liens utiles

- (Vidéo de Grafikart)[https://www.youtube.com/watch?v=tgI7_BIXcM8]
- (Doc de Rails)[http://guides.rubyonrails.org/asset_pipeline.html]
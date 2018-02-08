# Dotenv
Dotenv est une gem qui permet de stocker des clés d'API dans un joli fichier contenant toutes nos clés secrêtes, pour éviter de les push sur Github à l'arrache.

## Komment que ça marche ?
En gros on va mettre toutes les clés dans un fichier `.env`, puis on pourra les appeler en faisant `ENV['NOM_DE_LA_CLÉ']` dans notre programme Ruby. Puis afin d'éviter de pousser le bousin sur Github, on mettra le fichier `.env` dans notre `.gitignore`

## Exemple avec un dossier Ruby simple
Il faut commencer par créer un dossier avec nos fichiers, puis le lier à Github. Ne pas oublier d'installer la gem :
```shell
$ gem install dotenv
```

Créer un fichier `.env` qui contiendra nos clés d'APIs au format suivant :
```ruby
TWITTER_API_KEY="my-twitter-api-key"
TWITTER_API_SECRET="shh-so-secret"

BEST_WEBSITE_EVER="http://zombo.com"
```

En appelant la gem dotenv, tout fichier Ruby dans le dossier pourra accéder à ces données. Voici un exemple de programme qui s'appelle `kikou.rb` et qui appelle les variables :
```ruby
# Voici ton programme

# Appelle la gem dotenv
require 'dotenv'

# Ceci appelle le fichier .env grâce à la gem dotenv, et enregistre toutes les données enregistrées dans une hash ENV
Dotenv.load

# Il est très facile d'appeler les données sensibles du fichier .env, par exemple là je vais afficher TWITTER_API_SECRET
puts ENV['TWITTER_API_SECRET']
```

Et avant de faire un commit et de push sur Github, nous allons mettre le fichier `.env` dans le `.gitignore`. Ainsi, ce fichier ne sera pas push sur Github et il sera très facile d'y gérer nos clés d'API secrètes. Mettre la ligne suivante dans un fichier qui `.gitignore` :
```
.env
```

Et voilà, notre dossier devrait ressembler à ceci :

```
└── TON_DOSSIER
    ├── .env
    ├── .gitignore
    └── kikou.rb
```

## Exemple avec une app Rails
Hyper simple, mettre dans les gems de development et tests :
```ruby
gem 'dotenv-rails'
```
Puis créer un fichier `.env` dans lequel on mettra les variables. Et mettre `.env` dans le fichier `.gitignore`


Et puis c'est tout ! Les variables sont appelées pendant le `before_configuration`, donc on peut faire notre `ENV['TA_VARIABLE']` quand on veut. On peut les mettre avant en checkant [la doc](https://github.com/bkeepers/dotenv#installation).


## Quelques liens sympas

- [Un super répo](https://github.com/codeunion/dotenv-example) d'une personne qui explique comment lancer son dotenv
- [La gem dotenv](https://github.com/bkeepers/dotenv)
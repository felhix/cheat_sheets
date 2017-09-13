# Devise
## Setup
Dans le Gemfile :


```
gem 'devise'
```


Puis installer le bundle :


```
$ bundle install --without production
```

Installer Devise dans l'app Rails :


```
$ rails g devise:install
```


## Users
Pour générer un model User avec Devise :


```
$ rails g devise User
```

(ne pas oublier de faire une migration)

### Autoriser un attribut en plus dans le signup / edit
Aller dans `app/controllers/application_controller.rb` et avoir les lignes de code suivantes :

```ruby
class ApplicationController < ActionController::Base
  protect_from_forgery with: :exception

  before_action :sanitize_devise_params, if: :devise_controller?

  def sanitize_devise_params
    devise_parameter_sanitizer.permit(:sign_up, keys: [:truc_qui_nous_intéresse])
  end
end
```


Puis ajouter dans les views du dossier `registration` le field qui nous intéresse.


## Views
Pour générer nos views :

```
$ rails g devise:views
```

## Devise et Stripe
Comment faire pour faire un stripe dans le signup du user ?


Générer le controller user :

```
$ rails generate controller users
```


Puis dans le controller, dans `create`, balancer le Stripe :

```ruby
customer = Stripe::Customer.create(
  :email => params[:stripeEmail],
  :source  => params[:stripeToken]
)
```

Dans les routes, mentionner que l'on va utliser le controller pour les users.

```ruby
devise_for :users, controllers {  }
```
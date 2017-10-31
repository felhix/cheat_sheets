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


### Changer les url des méthodes de Devise
On peut changer les urls des méthodes de Devise, en mettant dans le fichier Routes `devise_scope`.
```ruby
devise_for :users, skip: [:registrations, :sessions]
devise_scope :user do
	get 'inscription', to: 'devise/registrations#new', as: :new_user_registration
	post 'inscription', to: 'devise/registrations#create', as: :user_registration
	get 'profil/editer', to: 'devise/registrations#edit', as: :edit_user_registration
	post 'profil/editer', to: 'devise/registrations#update', as: :users_edit
	get 'login', to: 'devise/sessions#new', as: :new_user_session
	post 'login', to: 'devise/sessions#create', as: :user_session
	delete 'logout', to: 'devise/sessions#destroy', as: :destroy_user_session
end
```
Ainsi, là j'ai dit à Devise de `skip` les routes pour registrations (`#new` et `#edit` les users), et les sessions. Puis dans le devise_scope, je lui paremètre les méthodes importantes. Pour s'inscrire ce ne sera plus `/users/new`, mais `inscription`.

### Changer de controller
Pour changer de controller dans Devise pour certaines fonctions, rien de plus simple. Il faut mettre dans le fichier `routes.rb` :
```ruby
devise_for :users, controllers: { registrations: 'registrations' }
```

Ainsi, le controller `app/controllers/registrations_controller.rb` sera le controller de Devise pour tout ce qui concerne les registrations.


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

## Creation d'un user en production
Comment créer un user en production ? Se mettre en console de prod avec `heroku run rails console`. Puis écrire la ligne suivante :

```
u = User.new(:email => 'test@example.com', :password => 'password', :password_confirmation => 'password')
u.save
```
(bien entendu cela marche que s'il n'y a pas d'autre attribut en paramètre !)
# Validation de paramètres

Vous souhaiterez probablement accéder aux données envoyées par l'utilisateur ou d'autres paramètres dans votre contrôleur. Il y a deux types de paramètres possibles dans une application Web.
Les premiers sont les paramètres qui sont envoyés comme partie de l'URL, appelés requête de chaîne de carateres.
La chaîne de carateres est tout ce qui se trouve après "?" dans l'URL. Le second type de paramètre fait généralement référence a des données POST. Ces informations proviennent généralement d'un formulaire HTML qui a été rempli par l'utilisateur.
C'est appellé POST donnee, car il ne peut être envoyé que dans le cadre d'une requête HTTP POST. Amber ne fait aucune distinction entre les paramètres de la chaîne de requête et les paramètres POST, et les deux sont disponibles dans le hash `params` de votre contrôleur:

```crystal
class UsersController < ApplicationController
    def create
        unless params.valid?
            response.puts {errors: params.errors}.to_json
            response.status_code 400
        end

        user = User.new params.validate!
        user.save!

        @client = Client.new
        redirect_to :index
    end

    # Définir les paramètres dans les actions
    def update
        update_params = params do
          required(:name, "Your First Name is missing!") { |p| p.name? & !p.name.empty? }
          required(:email, "Your email address is invalid!") { |p| p.email? & p.size.between? 1..10 }
          required(:last_name) { |p| p.last_name? }
        end

        unless update_params.valid?
            response.puts {errors: params.errors}.to_json
            response.status_code 400
        end

        user = User.new update_params.validate!
        user.save!

        redirect_to :index
    end
end
```

Avec Amber les paramètres de validation, il est facile de garder le code organisé

```crystal
#Vous pouvez définir des modules pour regrouper vos validations de paramètres
module UserParams
    def create_paramms
        params do
          required(:name, "Your First Name is missing!") { |p| p.name? & !p.name.empty? }
          required(:email, "Your email address is invalid!") { |p| p.email? & p.size.between? 1..10 }
          required(:last_name) { |p| p.last_name? }
        end
    end

    def self.update
        params do
          required(:name, "Your First Name is missing!") { |p| p.name? & !p.name.empty? }
          required(:email, "Your email address is invalid!") { |p| p.email? & p.size.between? 1..10 }
          required(:last_name) { |p| p.last_name? }
        end
    end
end

class UsersController < ApplicationController
    include UserParams

    def create
        unless UserParams.create.valid?
            response.puts {errors: params.errors}.to_json
            response.status_code 400
        end

        user = User.new UserParams.create.validate!
        user.save!

        @client = Client.new
        redirect_to :index
    end

    # Définitions des paramètres par action
    def update

        unless UserParams.update.valid?
            response.puts {errors: params.errors}.to_json
            response.status_code 400
        end

        user = User.new UserParams.update.validate!
        user.save!

        redirect_to :index
    end
end
```




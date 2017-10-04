# Cookies
Les cookies sont créés et écrit à travers  **Amber::Base::Controller\#cookies**

Les cookies en cours de lecture sont ceux reçus en même temps que la requête, le cookies qui sera écrit sera envoyé avec la réponse. La lecture du cookies n'aura pas d'object en retour, mais juste la valeur qu'il détient.


Il est conseillé de ne stocker que des données simples \ (chaîne de caractères et nombres \) dans les cookies. Si vous devez stocker des objets complexes, vous devrez gérer la conversion manuellement lorsque vous lirez les valeurs sur les demandes ultérieures.


Amber dispose également d'un cookie encrypté pour stocker les données sensibles. Le cookie encrypté jar chiffre les valeurs en plus de les signer,afin qu'elles ne puissent pas être lues par l'utilisateur final.

### Exemples d'écriture:

```crystal
class CommentsController < ApplicationController
  def new
    # Remplit automatiquement le nom du comment s'il a été stocké dans un cookie
    @comment = Comment.new(author: cookies[:commenter_name])
  end

  def create
    @comment = Comment.new(params[:comment])
    if @comment.save
      flash[:notice] = "Merci de votre commentaire!"
      if params[:remember_name]
        # Se rappeler du nom du comment.
        cookies[:commenter_name] = @comment.author
      else
        # Supprime le cookie pour le nom du comment si il y a. 
        cookies.delete(:commenter_name)
      end
      redirect_to @comment.article
    else
      render action: "new"
    end
  end
end
```

### Exemples de lecture 

```crystal
cookies[:user_name]           # => "david"
cookies.size                  # => 2
JSON.parse(cookies[:lat_lon]) # => [47.68, -122.37]
cookies.encrypted[:discount]  # => 45
```

Notez que si vous spécifiez un :domaine lors du réglage d'un cookie, vous devez également spécifier le domaine lors de la suppression du cookie:

```crystal
cookies[:name] = {
  value:   'a yummy cookie',
  expires: 1.year.from_now,
  domain:  'domain.com'
}
cookies.delete(:name, domain: 'domain.com')
```
Les différents symboles d'option pour configurer les cookies sont:

```crystal
:value  - La valeur du cookie.
:path   - Le chemin pour lequel ce cookie s'applique. Par défaut, la racine de l'application.
:domain - Le domaine pour lequel ce cookie s'applique afin que l'on puisse restreindre le niveau du domaine.
           Si vous utilisez un schéma comme www.example.com et que vous voulez partager la session avec user.exemple.com
	   définissez :domain à :all . Assurez-vous de spécifier l'option :domain avec :all ou encore un tableau lorsque vous supprimerez les cookies.

domain: nil                           # Ne définit pas le domain du cookie. (default)
domain: :all                          # Autorise le cookie pour le plus haut niveau du domaine ainsi que pour les sous-domaines.
domain: %w(.example.com .example.org) # Autorise le cookie pour les noms de domaine concrets.

:tld_length - Lors de l'utilisation de :domain => :all, cette option peut être utilisée pour définir explicitement la longueur du TLD  
              Lorsque l'on utilise un domaine court (<= 3 caractères) qui est interprété comme partie d'un TLD 
	      Par exemple si l'on partage un cookie entre user1.lvh.me et user2.lvh.me, il faut définir :tld_lenght à 1 

:expires    - L'heure à laquelle le cookie expire, comme objet dans le temps. 
:secure     - Si ce cookie doit uniquement etre transmis aux serveur HTTPS.  Par défaut il est défini a false.
:httponly   - Si le cookie se trouve être  accessible par scripting ou seulement par HTTP. Par défaut il est défini a false.
```

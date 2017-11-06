# Déployer Ambre sur Heroku

Si vous ne l'avez pas encore fait, lisez les guides pour installer la [CLI Amber] (/ getting-started / installation / heroku.md) 


## Heroku
Si vous avez généré votre projet avec la CLI de Ambre et que vous l'avez exécute localement avec succès, nous sommes maintenant à un point où nous pouvons créer une application sur [Heroku] (https://www.heroku.com/). Si vous ne l'avez pas encore fait, créez un compte sur Heroku et installez la [Heroku Toolbelt] (https://toolbelt.heroku.com/).

Le langage Crystal n'est pas pris en charge par Heroku, nous devons donc utiliser un buildpack personnalisé.
Vous pouvez créer une application dans Heroku avec le buildpack de Crystal en lançant la commande suivante:


```
$ heroku create myapp --buildpack https://github.com/crystal-lang/heroku-buildpack-crystal.git
```
Le comportement par défaut est d'utiliser la [dernière version de Crystal] (https://github.com/crystal-lang/crystal/releases/latest). Si vous avez besoin d'une version spécifique, créez un fichier `.crystal-version` dans le répertoire racine de votre application avec la version à utiliser \ (par exemple ``0.17.1` \).



### Exigences

Pour que le buildpack fonctionne correctement, vous devriez avoir un fichier `shard.yml`, car c'est ainsi qu'il détectera que l'application est une application Crystal.

Votre application doit écouter sur un port défini par Heroku. Il vous est donné via l'option de ligne de commande `- port` et la variable d'environnement'PORT` \ (accessible via `ENV [" PORT "]` dans Crystal \). Cependant, Amber devrait gérer cela pour vous.

Il ne reste plus qu'à créer un dépôt git, ajouter la télécommande Heroku et l'y pousser. N'oubliez pas d'ajouter `.deps, .crystal` et `libs`to`.gitignore.`


```
$ git init
$ heroku git:remote -a [app-name]
$ git add -A
$ git commit -m "My first Amber app"
$ git push heroku master
```

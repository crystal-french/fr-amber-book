# Filtres



Les filtres sont des méthodes qui s'exécutent "before", "after" ou "around" d'une action du contrôleur.

Les filtres sont hérités,donc si vous définissez un filtre sur `ApplicationController`, il sera exécuté sur chaque contrôleur de votre application.

Les filtres "before" peuvent arrêter le cycle de la requêtes. Un filtre commun "before" est requis pour qu'un utilisateur soit connecte est qu'une action puisse soit exécutée. 

```crystal
# Les filtres sont des méthodes qui sont lancés "before","after" l'action d'un contrôleur.
before_action do
  # runs for specified actions
  only [:index, :world, :show] { increment(1) }
  # runs for all actions
  all { increment(1) }
end

after_action do
  # runs for specified actions
  only [:index, :world] { increment(1) }
end
```


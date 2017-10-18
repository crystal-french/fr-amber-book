# Redirection

Souvent, nous avons a rediriger vers une nouvelle URL au milieu d'une requête. Une action `create` qui a réussie pour une instance, sera généralement redirigée vers l'action `show` pour le modèle que nous venons de créer. Autrement, elle pourrait etre rediriger vers l'action `index` pour montrer toutes les choses du même type. Il y a de nombreux autres cas où la redirection est également utile.

Appel de ** redirect \ _to ** va interrompre le cycle de vie de la requête.

**Redirection vers un URL**

```crystal
redirect_to(location: "", status: 302, params: { "key" => "value" }, flash: { "user_id" => "1" })
```

**Redirection vers une action**

```crystal
redirect_to(action: :index, status: 302, params: { "key" => "value" }, flash: { "user_id" => "1" })
```

**Redirection a un contrôleur d'action**

```crystal
redirect_to(
  controller: :symbol, 
  action: :index, 
  status: 302, 
  params: { "key" => "value" }, 
  flash: { "user_id" => "1" })
```

**Renvoie**

```crystal
redirect_back(status: 302, params: { "key" => "value" }, flash: { "user_id" => "1" })
```




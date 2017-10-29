#Requête et retour des objets  

Le retour des objets et les requêtes sont disponibles au contrôleur comme méthodes `request and response`

### Les méthodes Request Object et Controller Helper 

Les deux servent à la fois pour effectuer des requêtes par un [`HTTP::Client`](https://crystal-lang.org/api/0.23.0/HTTP/Client.html)et pour redonner les requêtes reçues par un [`HTTP::Server`](https://crystal-lang.org/api/0.23.0/HTTP/Server.html).Une requête contient toujours un [`IO`](https://crystal-lang.org/api/0.23.0/IO.html).Lorsque vous créez une requête avec un [`String`](https://crystal-lang.org/api/0.23.0/String.html) ou [`Bytes`](https://crystal-lang.org/api/0.23.0/Bytes.html) son corps sera un [`IO::Memory`](https://crystal-lang.org/api/0.23.0/IO/Memory.html) enveloppant ceci, et le `Content-Length` ainsi l'en-tête sera défini correctement.

L'objet de la requête contient beaucoup d'informations utiles sur la requête en provenance du client. Pour obtenir une liste complète des méthodes disponibles, reportez-vous à la section Crystal API Documentation [https://crystal-lang.org/api/0.23.0/HTTP/Request.html](https://crystal-lang.org/api/0.23.0/HTTP/Request.html)

| Controler Request Helper Methods | Purpose | Implemented? |
| :--- | :--- | :--- |
| host | Le nom d'hôte utilisé pour cette requête. | Yes |
| domain\(n=2\) | Les premiers segments du nom d'hôte, en partant de la droite \(le TLD\). | No |
| format | Le type de contenu demandé par le client. | Yes |
| method | La méthode HTTP utilisée pour la requête. | Yes |
| get?, post?, patch?, put?, delete?, head? | renvoie true si la méthode HTTP est  GET/POST/PATCH/PUT/DELETE/HEAD. | Yes |
| headers | Renvoie un hach contenant les en-têtes associés à la requête. | Yes |
| port | Le numéro de port \ (integer \) utilisé pour la requête. | Yes |
| protocol | Retourne une chaîne contenant le protocole utilisé plus "://", par exemple "http://". | No |
| query\_string | La partie de la chaîne de caractere de la requête de l'URL, c'est-à-dire tout ce qui suit apres "?". | Yes |
| client\_ip |  l @IP du client via les en-têtes de cookie. | Yes  |
| requested\_url | L'URL complète utilisée pour les requetes. | No |

### l'objet de réponse 


L'objet de la réponse n'est généralement pas utilisé directement, mais il est construit pendant l'exécution de l'action et le rendu des données renvoyées à l'utilisateur,  parfois comme dans un post-filtre, il peut être utile d'accéder à la réponse directement. Certaines de ces méthodes accessor ont également des setters, vous permettant de changer leurs valeurs. Pour obtenir une liste complète des méthodes disponibles, reportez-vous a [https://crystal-lang.org/api/0.23.0/HTTP/Server/Response.html](https://crystal-lang.org/api/0.23.0/HTTP/Server/Response.html)

| Propriété de réponse | Purpose |
| :--- | :--- |
| body | Il s'agit de la chaîne de données qui est renvoyée au client. C'est le plus souvent en HTML. |
| status | Le code de status  HTTP pour la réponse, 200 pour une requete réussie ou  404  pour  file not found. |
| location | L'URL vers laquelle le client est redirigé, le cas échéant. |
| content\_type | Le type de contenu de la réponse. |
| charset | Le jeu de caractères utilisé pour la réponse. Par défaut c'est "utf-8". |
| headers | Les en-têtes utilisés pour la réponse.|

# Petit guide du HTTP Restful

## HTTP, vous dites ?

Prenons un peu de temps pour expliquer le format du [HTTP]. Ou une partie, afin de commencer sur un vocabulaire commun.
Si le HTTP n'a plus de secrets pour vous, n'hésitez pas à [passer cette section](#restful).

Le [HTTP] est un protocole de communication basé sur du [TCP].
Il s'agit ici d'une simplification : l'[HTTP/3] utilise également de l'[UDP].
Ne m'en veuillez pas, mais j'ai la flemme.

Pour simplifier, le TCP est un protocole de communication permettant d'envoyer et de recevoir des informations depuis internet.
Il a l'énorme avantage d'être _fiable_, autrement dit, les données envoyées par celui-ci seront obligatoirement reçues.
Il se situe dans la couche de transport dans le [modèle OSI]. Vous vous souvenez du modèle OSI ? Moi non plus.

Le HTTP, quant à lui, est dans la couche d'application.

### Requête

Une requête HTTP ressemble vaguement à ceci :

```
GET /my-resource HTTP/1.1
Host: my-host.com
```
_Dans cet exemple, on souhaite à récupérer le document `my-resource` sur le serveur `my-host.com`. Il devrait donc se trouver à `http://my-host.com/my-resource`._

`GET` correspond à la _méthode_ HTTP. C'est elle qui détermine l'action associée à une _ressource_.

`my-resource` correspond à la _ressource_ qu'on souhaite traiter.

`HTTP/1.1` correspond au protocole qu'on utilise pour utiliser la ressource.
Il existe plusieurs versions du protocoles HTTP, les plus répandues sont [HTTP/1.1] et [HTTP/2].
Le [HTTP/3] n'est toujours pas complètement spécifié au moment où j'écris ces lignes.

`Host` est un _header_ et `my-host.com` sa valeur. Il s'agit d'éléments permettant de donner des informations supplémentaires au serveur.

Notons également que vous pouvez passer du contenu dans votre requête. Pour plus d'infos, n'hésitez pas à regarder [ici][message_http].

### Réponse

Une fois la requête envoyée, le serveur se doit aimablement de renvoyer une réponse à cette requête.

La réponse contient le contenu demandé par la requête ainsi qu'un [status] permettant d'identifier rapidement le type de réponse. (Erreur, redirection, ...).
Elle contient également, comme pour la requête, des _headers_ permettant de donner des informations supplémentaires.

## Restful

Le restful est une utilisation particulière du protocole HTTP visant à mettre à disposition des ressources par le web via des [URI].
Il permet de traiter ces ressources de manière indépendante via un set d'opération défini : les [méthodes](#méthodes).

### Ressource

Une _ressource_ est une entité abstraite ou réelle identifiée grâce à une convention d'URL sur un serveur web.

On distingue généralement une collection (au pluriel) d'un item (identifié depuis une collection).
Par exemple, `/files` est une collection, alors que `/files/video.mp4` est un item.

Dans le cadre d'une API restful, chaque URL se doit d'être une ressource.
Ainsi, les verbes sont à proscrire, que ce soit dans les URL ou dans les paramètres de query.

Il est également important de noter qu'une ressource devrait conserver un format de réponse identique.
Dans le cas d'une ressource [polymorphique](https://fr.wikipedia.org/wiki/Polymorphisme_(informatique)), certains attributs peuvent être `null`.
Cependant, si celle-ci change en fonction des usages, il ne s'agit probablement pas de la même ressource.
N'hésitez pas à composer si nécessaire. (i.e `/resources` et `/something-resources`).

### Méthodes

Il y a plusieurs [méthodes] en HTTP. Les méthodes et leurs usages pour le _restful_ sont les suivants :

| Méthode | Description                           | Exemple                 |
| ------- | ------------------------------------- | ----------------------- |
| GET     | Récupérer un item                     | `GET /resources/123`    |
| GET     | Récupérer une collection              | `GET /resources`        |
| POST    | Créer une ressource                   | `POST /resources`       |
| PUT     | Mettre à jour une ressource complète  | `PUT /resources/123`    |
| PATCH   | Mettre à jour une ressource partielle | `PATCH /resources/123`  |
| DELETE  | Supprimer un item                     | `DELETE /resources/123` |
| DELETE  | Supprimer une collection              | `DELETE /resources`     |

Afin de donner un exemple, nous pouvons imaginer une API permettant de lister des chats.
Cette ressource aurait par exemple un âge, un nom et un pelage.

Afin de conserver un semblant de professionnalisme, l'API sera en anglais.

| Action            | Faire                  | Ne pas faire                                      |
| ----------------- | ---------------------- | ------------------------------------------------- |
| Créer un chat     | `POST /cats`           | `POST /cats/create`<br>`POST /cats?action=create` |
| Récupérer un chat | `GET /cats/123`        | `GET /cats?id=123`                                |
| Chercher un chat  | `GET /cats?name=Felix` | `GET /cats/search?name=felix`                     |

Afin de clarifier la différence entre un `PUT` et un `PATCH`, voici un exemple.

Pour mettre à jour un chat, on peut alors envoyer la requête suivante :
```json
PUT /cats/123
{
    "age": 3,
    "name": "Felix",
    "fur": "striped"
}
```
Et afin de mettre à jour uniquement l'âge du même chat, on peut utiliser cette requête :
```json
PATCH /cats/123
{
    "age": 4
}
```

### GET ou POST ?

Jusqu'à présent, j'ai sous-entendu que pour récupérer une ressource, il fallait _forcément_ utiliser un `GET`.
Hors, je vous ai MENTI !

Il y a un détail dont j'ai négligé de parler jusqu'à maintenant, la notion de requête [sûre].

Une requête sûre est définie comme une requête qui n'entraîne pas de modification d'une ressource côté serveur.
Par définition, `GET` et `HEAD` sont considérés comme des méthodes sûres dans la [spécification HTTP][sûre].

Hors, on peut imaginer assez simplement des cas d'utilisations où le serveur peut créer et renvoyer une ressource temporaire.
C'est généralement le cas dans des flux d'autorisation où le serveur renvoie une clé d'identification.

Dans ces cas-là, on privilégiera l'utilisation du `POST` au `GET`.

**Note**: N'oubliez pas de considérer également l'[idempotence] pour vos requêtes !

### Passer des données

Il y a 3 moyens de faire transiter des données en HTTP :

- Paramètre de query (_parameters_)
- Headers
- Contenu de la requête (_body_)

Les paramètres de query (i.e les valeurs passées après le `?` dans la ressource) doivent être utilisées comme des "filtres" sur la ressource.
Si un paramètre de query n'est _pas_ relatif à la ressource demandée, celui-ci doit impérativement être passé dans les _headers_ afin d'éviter le _cache busting_.

Le _cache busting_ est le fait d'invalider le cache (navigateur ou serveur) alors que celui-ci devrait être toujours valide.
En effet, lorsqu'un navigateur récupère une ressource depuis une URL, celui-ci peut potentiellement la garder en [cache] grâce à son URL afin d'éviter de faire des requêtes supplémentaires via internet. Si l'URL change alors que la ressource est la même, on ira chercher cette ressource sur internet au lieu de récupérer celle qui est sauvegardée en local. C'est dommage.

**Note**: Une URL peut-être considérée [trop longue](https://developer.mozilla.org/fr/docs/Web/HTTP/Status/414) pour un serveur web. Réfléchissez bien à ce que vous envoyez dans votre query. Considérez qu'une longueur de 2000 caractères est le "maximum".

Les [headers] permettent de faire transiter des informations associées à la requête, l'utilisateur courant ainsi que des metadatas associées à la ressource.
Ils permettent également de gérer le [cache](#cache).

Enfin, le _body_ de votre requête devrait contenir la ressource, partielle ou complète, lors d'une création ou d'une mise à jour de celle-ci. Si possible, le format [json] devrait être privilégié en raison de sa simplicité.

### Code de réponse

Il y a de multiples codes possibles à renvoyer lors de la réponse d'un serveur.

Ce code permet de donner une indication au client du status de sa requête.
Par convention, on utilise les codes suivants :

| Usage                                                           | Status |
| --------------------------------------------------------------- | ------ |
| Récupération d'une ressource                                    | 200    |
| Création d'une nouvelle ressource                               | 201    |
| Mise à jour d'une ressource                                     | 204    |
| Suppression d'une ressource                                     | 204    |
| La requête est invalide                                         | 400    |
| L'utilisateur n'est pas authentifié                             | 401    |
| L'utilisateur n'a pas les permissions pour effectuer une action | 403    |
| La ressource ciblé n'existe pas                                 | 404    |

Notons que le status [204] est spécial car il n'autorise pas à renvoyer du contenu avec la réponse.
Il est utilisé pour signifier que la requête a bien été prise en compte mais sans avoir besoin de renvoyer d'informations supplémentaires au client.

En fonction du contexte, d'autres codes peuvent être utilisés, n'hésitez pas à consulter la [liste][status].

### Cache

Le cache est une composante importante d'un service web pour fluidifier l'expérience utilisateur d'une part, et pour éviter de surcharger les serveurs d'autre part.
Il y a plusieurs types de [cache], mais je compte me concentrer sur celui du navigateur (appelé _local cache_).

En effet, la façon la plus simple de gérer un cache est de renvoyer depuis le serveur des headers particuliers permettant de cacher les réponses du serveur.

Pour ce faire, il existe un header simple, [Cache-Control] permettant de le contrôler.
On utilise un `Cache-Control: private` lorsque la ressource est relative à un utilisateur et `public` lorsque ce n'est pas le cas.
Il faut également spécifier la durée du cache associé.
Une fois que cela est fait, la client ne doit pas renvoyer de requête associée à cette ressource au serveur tant que son cache n'est pas expiré.

De plus, le serveur peut utiliser un [ETag] associé à chaque ressource permettant de valider si une ressource a changé.
C'est alors au serveur de stocker pour chaque URL une chaîne de caractère unique (par exemple, un UUID) et de le renvoyer.

Lorsque le client demande une nouvelle fois cette ressource et que le serveur détecte qu'il n'y a aucun changement grâce à l'_ETag_, il peut renvoyer un status [304], sans la ressource associée.
C'est alors au navigateur de récupérer la ressource dans son cache.

### Documenter

Pour documenter des API, on utilise généralement [openapi].
Ce format permet de décrire dans des fichiers `json` ou `yaml` les ressources disponibles pour une API, spécifiant les requêtes et réponses associées.

Un éditeur en ligne est disponible [ici](https://editor.swagger.io/).

# Ressources

[Messages HTTP][message_http]

[Liste des status HTTP](status)

[Méthodes HTTP](méthodes)

[Cache](cache)

[Headers](headers)

[Évolution du HTTP](https://developer.mozilla.org/fr/docs/Web/HTTP/Basics_of_HTTP/Evolution_of_HTTP)

[Safe and Idempotent Methods](https://datatracker.ietf.org/doc/html/rfc2616#section-9.1)

[HTTP]: https://developer.mozilla.org/en-US/docs/Web/HTTP/Messages
[TCP]: https://datatracker.ietf.org/doc/html/rfc793
[UDP]: https://datatracker.ietf.org/doc/html/rfc768
[modèle OSI]: https://fr.wikipedia.org/wiki/Mod%C3%A8le_OSI
[HTTP/1.1]: https://datatracker.ietf.org/doc/html/rfc2616
[HTTP/2]: https://datatracker.ietf.org/doc/html/rfc7540
[HTTP/3]: https://datatracker.ietf.org/doc/html/draft-ietf-quic-http-34
[méthodes]: https://developer.mozilla.org/fr/docs/Web/HTTP/Methods
[message_http]: https://developer.mozilla.org/fr/docs/Web/HTTP/Overview#les_messages_http
[status]: https://developer.mozilla.org/fr/docs/Web/HTTP/Status
[URI]: https://developer.mozilla.org/fr/docs/Glossary/URI
[204]: https://developer.mozilla.org/fr/docs/Web/HTTP/Status/204
[sûre]: https://datatracker.ietf.org/doc/html/rfc2616#section-9.1.1
[idempotence]: https://datatracker.ietf.org/doc/html/rfc2616#section-9.1.2
[cache]: https://developer.mozilla.org/fr/docs/Web/HTTP/Caching
[headers]: https://developer.mozilla.org/fr/docs/Web/HTTP/Headers
[json]: https://www.json.org
[Cache-Control]: https://developer.mozilla.org/fr/docs/Web/HTTP/Headers/Cache-Control
[ETag]: https://developer.mozilla.org/fr/docs/Web/HTTP/Headers/ETag
[304]: https://developer.mozilla.org/fr/docs/Web/HTTP/Status/304
[openapi]: https://spec.openapis.org/oas/latest.html

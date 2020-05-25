## Envoyer des messages

Pour envoyer un message, il y a deux façons de s'y prendre
```py
Channel.send(message)
# Façon largement préférée, facile d'utilisation et simple à comprendre.

bot.send_message(channel_id, message)
# Ancienne forme, dépréciée, a utiliser le moins possible, sauf dans des cas très précis.
```
Dans ce tutoriel, on s'attardera sur la première forme.

Le premier argument de Channel.send() est le contenu (content) du message.
Il n'est pas obligé d'être spécifié en tant que kwarg (sous forme `Channel.send(content = "contenu")`) 
mais peut simplement être mis directement `Channel.send("contenu")`.

Cependant, pour le reste des arguments, il faut spécifier le nom.

### Arguments

Il y a différents arguments que l'on peut mettre dans le send :
- tts : Une valeur True ou False, si le message envoyé est un text-to-speech.
- files : Une liste des nom de fichiers que l'on souhaite envoyer (si l'on en envoie).
- embed : Un embed, nous verrons plus loin comment en faire.
- allowed_mentions : Un objet Allowed_Mentions, nous verrons également comment le faire plus loin.

### Allowed_Mentions

Les mentions autorisés permettent d'empêcher que le bot mentionne par mégarde quelque-chose qu'il ne devrait pas
(ex : mentionner everyone parce qu'il a les perms et qu'un malin lui fait répéter une mention qu'il ne peut pas utiliser).

Il a plusieurs paramètres : 
Parse : Parse est essentiel quand on joue avec la classe. Elle indique les types de mentions a autoriser, même si on doit les détailler après.
C'est une liste qui peut prendre les arguments que l'on veut selon ce que l'on souhaite faire.
- "everyone" : Autorise les mentions d'`@everyone` et `@here`
- "users" : Permet de mentionner les utilisateurs.
- "roles" : Permet de mentionner les rôles.

Ainsi, par exemple, vous pouvez faire :
```py
Allowed_Mentions.parse = []
# Supprime toutes les mentions

Allowed_Mentions.parse = ["everyone"]
# Ne permet au bot de mentionner seulement everyone/here (s'il en as les permissions)
```

Users : Users est un paramètre de la classe qui, si on ne permet pas de mentionner tous les utilisateurs (en mettant "users"),
permet de quand même en mentionner, mais en précisant les id des utilisateurs a laisser (avec un maximum de 100).

Roles : Roles est comme Users, mais pour les rôles. Si on ne permet pas au bot de mentionner tous les rôles,
permet de spécifier l'id des rôles a pouvoir mentionner quand même.

#### Exemple

Voici un exemple de commande que l'on peut faire, ou cela se trouve utile :

```py
@bot.event
def on_message(message):
  content = message.content.split()
  if content[0] == "!me":
    if len(content) > 1:
      allow = Allowed_Mentions() # ou Allowed_Mentions({}) selon la version.
      allow.parse = []
      allow.users = [message.author.id]
      message.channel.send(" ".join(content[1:]),allowed_mentions=allow.to_json()) #Ou allowed_mentions=allow.__dict__ selon la version.

# Envoie le message que l'utilisateur a mis après la commande !me, tout en ne pouvant mentionner que l'auteur de la commande.
```

### Les embeds

Les embeds sont quelque chose de très importants par rapport au messages, et mérite que l'on s'y attarde.

Ces derniers ont de nombreuses propriétés, et l'on s'attardera pas sur toutes. Pour plus d'informations,
visitez [La Documentation relative aux Embed](https://piscord.astremy.com/#Embed) ou [Les informations de Discord sur les Embed](https://discord.com/developers/docs/resources/channel#embed-object).

Voici les plus utiles :
- title : Une chaine de caractère correspondante au titre de l'Embed.
- description : Le texte dans l'Embed.
- color : La couleur de l'Embed (sous format hexadécimal passé en décimal).
- Image : Un objet image correspondant à une image principale de l'Embed. (Voir la documentation)

De plus, les Embed ont ce que l'on appelle les fields.
Ce sont des zones qui contiennent chacun leur titre et texte que l'on met dans un Embed.
Pour ajouter un field a un embed, on utilise Embed.add_field(name = name, value = value, inline = inline)

#### Exemple
```py
embed = Embed() # Crée l'embed

embed.title = "Description des commandes" # Défini le titre de l'embed
embed.color = 3375070 # Défini la couleur

embed.add_field(name="!me",value="Commande qui répète du texte, en ne pouvant mentionner que soit-même")
# Le name correspond au titre du field, la value à son contenu. Mettre inline = True permet de faire revenir le bloc a la ligne.
embed.add_field(name="!ping",value="Commande de base qui répond 'Pong !'")

Channel.send(embed=embed.to_json())
```
Quand l'on envoie un embed dans un channel, il ne faut pas oublier de mettre un .to_json, comme pour le Allowed_Mentions.



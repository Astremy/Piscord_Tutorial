## Premier Bot

### Mettre en ligne le bot

Pour mettre en ligne votre premier bot, récupérez le __token__ de ce dernier et faites comme ceci :

```py
from Piscord import Bot
# Import de la classe Bot a partir de la librairie

bot = Bot("Token")
# Création d'un Bot avec votre token

@bot.event
def on_ready(ready):
  print(f"Bot {ready.user.name} Connected")
# Créer un event correpondant à celui de "on_ready" (quand le bot est connecté a discord), et dire que le bot est lancé.

bot.run()
# Lancer le bot en mode bloquant
```

#### Information :
Vous avez deux façons de créer un event :
- En utilisant @bot.event et en renommant la fonction du nom de l'event
- En utilisant @bot.event("nom_de_l_event") et en renommant la fonction comme vous le souhaitez
Ce qui donne ça :
```py
@bot.event
def on_ready(ready):...

ou

@bot.event("on_ready")
def ready(ready):...
```

Aussi, vous pouvez lancer le bot en mode bloquant (le code après ne sera pas exécuté) ou non bloquant.
```py
bot.run()
# Manière bloquante, a utiliser si l'on ne compte rien faire en même temps que lancer le bot.

bot.start()
# Manière non bloquante, si on lance d'autres bots après, ou un serveur web en parallèle.
```

### Ping Pong
Une manière souvent utilisé pour illustrer la création d'un pemier bot est une commande ("ping") qui fera répondre une autre ("pong") au bot.
Voyons comment le faire avec Piscord :
```py
from Piscord import Bot

bot = Bot("Token")

@bot.event
def on_ready(ready):
  print(f"Bot {ready.user.name} Connected")

@bot.event
def on_message(message):
  if message.content == "!ping":
    message.channel.send("Pong !")
# Quand un message est envoyé, on vérifie si son contenu est "!ping".
# Si c'est le cas, on envoi dans le salon le message "Pong !"

bot.run()
```

### Objet Message

Quand on déclare un event, on récupère en argument un objet Event nous permettant de récupérer des informations de ce dernier.

Pour les voir je vous invite a aller consulter [La Documentation au sujet des Messages](https://piscord.astremy.com/#Message).
Sinon, voyons ensemble les plus utiles :

```py
Message.content
# Le contenu du message, ce qu'à saisi l'utilisateur. Cela permet d'identifier des éventuels commandes, gros mots...
# C'est le message en lui-même (une chaine de caractères).

Message.author
# L'auteur du message. Un objet User se rapportant à l'utilsateur qui a éxécuté la commande,
# permettant d'avoir diverses informations sur lui (pseudo, id..)

Message.channel
# Le salon dans lequel a été envoyé le message. Permet d'y renvoyer un message,
# Ou de récupérer des informations sur le salon.

Message.guild
# Le serveur dans lequel a été envoyé le message.
# Permet d'en récupérer des informations.
```

### Un bot basique

Voici un exemple un peu plus développé de l'utilisation de l'argument (je ne montre pas tout le code, seulement la partie event) :
```
@bot.event
def on_message(message):
  if message.content == "!infos":
    if message.guild:
      server = message.guild.name
    else:
      server = "Aucun, nous sommes en messages privés"
    message.channel.send(f"Informations :\nUtilisateur : {message.author}\nServeur : {server}")
```

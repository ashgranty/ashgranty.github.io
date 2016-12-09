---
layout: post
title: ISPConfig
description: "Service Web"
tags: [réseau, serveur, web, admin]
---

> Prérequis :

> - Debian Jessie
> - Apache2


Nom d'hôte :
Modifier le nom d’hôte (hostname) d’une machine

```lua
sudo vim /etc/hosts

127.0.0.1       localhost
#127.0.1.1       ancien_nom.home      ancien_nom
127.0.1.1       nouveau_nom.home      nouveau_nom
```

```lua
sudo vim /etc/hostname

nouveau_nom
```

On utilise la commande hostname pour valider les changements :

```lua
sudo hostname nouveau_nom
```

Si vous vous déconnectez de la machine et vous y reconnectez, vous devriez voir nom_utilisateur@nouveau_nom au début de chaque ligne du terminal.

Téléchargement de ISPConfig :

```lua
sudo wget https://sourceforge.net/projects/ispconfig/files/ISPConfig%203/ISPConfig-3.1.1p1/ISPConfig-3.1.1p1.tar.gz
sudo tar -xvzf ISPConfig-3.1.1p1.tar.gz
```

```lua
cd ispconfig3_install/install/
```

en cours

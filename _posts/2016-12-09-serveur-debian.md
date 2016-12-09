---
layout: post
title: Serveur Debian neuf
description: "Debian server"
tags: [serveur, Debian]
---

Télécharger une archive : 

```lua
http://ftp.nl.debian.org/debian/dists/jessie/main/installer-amd64/current/images/netboot/mini.iso
```

Partitionning method:

```lua
Guided - use entire disk
Separate /home, /var and /tmp partitions
```
Ne pas installer de programme.

Installer Grub automatiquement.

Installer la commande sudo :

```lua
su root
apt-get install sudo
adduser debian sudo
su debian
```

```lua
sudo apt-get update
sudo apt-get upgrade
```


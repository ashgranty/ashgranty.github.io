---
layout: post
title: OCS Inventory NG
description:
tags: [administration, réseau, serveur]
catégories: [serveur, réseau, administration]
---

Télécharger l'archive :

```lua
sudo wget https://github.com/OCSInventory-NG/OCSInventory-Server/archive/2.3.tar.gz
suo mv OCSInventory-Server-2.3/ /var/www/
```

Installer les paquets suivants :

```lua
sudo apt-get install make
sudo apt-get install libapache2-mod-perl2
sudo apt-get install libapache2-mod-php5
sudo apt-get install GCC
sudo apt-get install php-pclzip
sudo apt-get install libxml-simple-perl libdbi-perl libdbd-mysql-perl libapache-dbi-perl libnet-ip-perl
sudo apt-get install libarchive-zip-perl
sudo apt-get install libsoap-lite-perl
```

Lancer l'installation :

```lua
sudo /var/www/OCSInventory-Server-2.3/setup.sh
```


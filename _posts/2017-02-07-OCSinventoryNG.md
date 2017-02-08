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
sudo apt-get install php5-dev
```
libcompress-zlib-perl --> libio-compress-perl

```lua
sudo wget http://www.cpan.org/authors/id/P/PH/PHRED/Apache-DBI-1.12.tar.gz
sudo tar -xvzf Apache-DBI-1.12.tar.gz
sudo perl Makefile.PL
sudo make
sudo make install
```

```lua
sudo wget http://www.cpan.org/authors/id/S/SI/SIXTEASE/XML-Entities-1.0002.tar.gz
sudo tar -xvzf XML-Entities-1.0002.tar.gz
sudo perl Makefile.PL
sudo make
sudo make install
```

EN COURS 

sudo wget http://www.cpan.org/authors/id/R/RK/RKOBES/Apache2-SOAP-0.73.tar.gz
sudo tar -xvzf Apache2-SOAP-0.73.tar.gz
sudo perl Makefile.PL
sudo make
sudo make install



Lancer l'installation :

```lua
sudo /var/www/OCSInventory-Server-2.3/setup.sh
```


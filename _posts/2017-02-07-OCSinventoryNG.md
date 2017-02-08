---
layout: post
title: OCS Inventory NG
description:
tags: [administration, réseau, serveur]
catégories: [serveur, réseau, administration]
---

```lua
sudo wget https://github.com/OCSInventory-NG/OCSInventory-Server/archive/2.3.tar.gz
suo mv OCSInventory-Server-2.3/ /var/www/
````

```lua
sudo apt-get install make
sudo apt-get install libapache2-mod-perl2
sudo apt-get install libapache2-mod-php5
sudo apt-get install GCC
sudo apt-get install php-pclzip
sudo apt-get install libxml-simple-perl libdbi-perl libdbd-mysql-perl libapache-dbi-perl libnet-ip-perl
sudo apt-get install Archive-Zip*
sudo apt-get install libsoap-lite-perl
```

php-gd php-xml php-mbstring

```lua
sudo /var/www/OCSInventory-Server-2.3/setup.sh
```

Checking for SOAP::Lite Perl module...
*** ERROR: PERL module SOAP::Lite is not installed !
Checking for Archive::Zip Perl module...
*** ERROR: PERL module Archive::Zip is not installed !




sudo apt-get install ocsinventory-server
sudo mv /usr/share/ocsinventory* /var/www/

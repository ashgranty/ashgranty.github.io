---
layout: post
title: GLPI, première installation
description: "choix de l'outil de ticketting"
tags: [ticketing, glpi]
---

> Prérequis :
> Apache2
> MySQL
> sudo apt-get install apache2 php5 libapache2-mod-php5
> sudo apt-get install php5-imap php5-ldap php5-curl
> sudo apt-get install mysql-server php5-mysql

Télécharger et installer GLPI :

```lua
sudo wget https://github.com/glpi-project/glpi/releases/download/9.1.1/glpi-9.1.1.tgz
sudo tar -xvzf glpi-9.1.1.tgz
cp glpi/ /var/www/
sudo chown /var/www/glpi
sudo /etc/init.d/apache2 restart
```

Création de la base MySQL :

```lua
mysql -u root -p
mysql> create database glpi;
mysql> grant all privileges on glpi.* to glpi@localhost identified by 'glpi';
mysql> quit;
```

localhost/glpi

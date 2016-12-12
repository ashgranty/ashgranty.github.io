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

```lua
sudo apt-get install ntp ntpdate
apt-get install postfix postfix-mysql postfix-doc mysql-client mysql-server courier-authdaemon courier-authlib-mysql courier-pop courier-pop-ssl courier-imap courier-imap-ssl libsasl2-2 libsasl2-modules libsasl2-modules-sql sasl2-bin libpam-mysql openssl courier-maildrop getmail4 rkhunter binutils sudo gamin
```

```lua
apt-get install amavisd-new spamassassin clamav clamav-daemon zoo unzip bzip2 arj nomarch lzop cabextract apt-listchanges libnet-ldap-perl libauthen-sasl-perl clamav-docs daemon libio-string-perl libio-socket-ssl-perl libnet-ident-perl zip libnet-dns-perl
```

```lua
apt-get install apache2 apache2.2-common apache2-doc apache2-mpm-prefork apache2-utils libexpat1 ssl-cert libapache2-mod-php5 php5 php5-common php5-gd php5-mysql php5-imap phpmyadmin php5-cli php5-cgi libapache2-mod-fcgid apache2-suexec php-pear php-auth php5-mcrypt mcrypt php5-imagick imagemagick libapache2-mod-suphp libruby libapache2-mod-ruby
```

```lua
apt-get install pure-ftpd-common pure-ftpd-mysql quota quotatool
```

```lua
apt-get install bind9 dnsutils
```

```lua
apt-get install vlogger webalizer awstats geoip-database
```

```lua
apt-get install build-essential autoconf automake1.9 libtool flex bison debhelper
sudo wget http://olivier.sessink.nl/jailkit/jailkit-2.19.tar.gz
```

```lua
apt-get install fail2ban
```

"
1.Etape Installation Apache :

Problèmes avec libapache2-mod-suphp libapache2-mod-ruby. (non trouvées) – j’ai du les virer pour que ça passe.

2. FTP / Quota
le fichier FSTab ne contient que : # UNCONFIGURED FSTAB FOR BASE SYSTEM
il faut donc le remplir avec les partitions.

3. Installation Jailkit
– pas de package automake1.9 – installer automake donc comme suit :
apt-get install build-essential autoconf automake libtool flex bison debhelper
"


Téléchargement de ISPConfig :

```lua
sudo wget https://sourceforge.net/projects/ispconfig/files/ISPConfig%203/ISPConfig-3.1.1p1/ISPConfig-3.1.1p1.tar.gz
sudo tar -xvzf ISPConfig-3.1.1p1.tar.gz
```

```lua
cd ispconfig3_install/install/
sudo php -q install.php
```

en cours

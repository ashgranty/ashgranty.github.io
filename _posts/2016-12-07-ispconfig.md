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

Parmi les autres vérifications importantes à faire avant l’installation d’ISPConfig, il convient de s’assurer que bash est le shell par défaut. ISPConfig, ne supporte pas encore dash et c’est pour cela que cette modification est nécessaire. On l’effectuera par la commande suivante:

```lua
sudo dpkg-reconfigure dash
Use dash as the default system shell (/bin/sh)? <– No
```

La vérification du shell par défaut se fait de la façon suivante:

```lua
sudo echo $SHELL
```

doit retourner

```lua
/bin/bash
```

Installation de Postfix, Courier, Saslauthd, MySQL, rkhunter, binutils

```lua
sudo apt-get install ntp ntpdate
sudo apt-get install postfix postfix-mysql mysql-client mysql-server courier-authdaemon courier-authlib-mysql courier-pop courier-pop-ssl courier-imap courier-imap-ssl libsasl2-2 libsasl2-modules libsasl2-modules-sql sasl2-bin libpam-mysql openssl courier-maildrop getmail4 rkhunter binutils sudo gamin
```

Vous aurez à répondre aux questions suivantes:

```lua
General type of mail configuration: <– Internet Site
System mail name: <– nomdeserveur.exemple.com
New password for the MySQL « root » user: <– votremdpsql
Repeat password for the MySQL « root » user: <– votremdpsql
Create directories for web-based administration? <– No
SSL certificate required <– Ok
```

Nous allons également modifier la configuration de MySQL pour que l’écoute se fasse sur toutes les interfaces du serveur (et non uniquement Localhost) : 

```lua
vi /etc/mysql/my.cnf
Commentez la ligne bind-address = 127.0.0.1

[…]
# Instead of skip-networking the default is now to listen only on
# localhost which is more compatible and is not less secure.
#bind-address = 127.0.0.1
[…]
```

On peut dès à présent vérifier le bon fonctionnement de MySQL:

```lua
sudo netstat -tap | grep mysql

tcp        0      0 *:mysql                 *:*                     LISTEN      10457/mysqld
```

Il est également nécessaire de recréer les certificats SSL pour courier. (Ceux-ci sont créés avec le hostname localhost. On souhaite les recréer pour intégrer le nom de notre serveur (nomdeserveur.exemple.com.)
On commence par supprimer les certificats présents:

```lua
cd /etc/courier
sudo rm -f /etc/courier/imapd.pem
sudo rm -f /etc/courier/pop3d.pem
```

On modifie ensuite les deux fichiers imapd.cnf et pop3d.cnf en remplacant le paramètre CN=localhost par CN=nomdeserver.exemple.com

```lua
sudo vim /etc/courier/imapd.cnf
[…]
CN=server1.example.com
[…]
```

```lua
sudo vim /etc/courier/pop3d.cnf

[…]
CN=server1.example.com
[…]
```

Puis on recrée les certificats :

```lua
sudo mkimapdcert
sudo mkpop3dcert
```

Il ne reste plus qu’à redémarrer le serveur courier :

```lua
sudo /etc/init.d/courier-imap-ssl restart
sudo /etc/init.d/courier-pop-ssl restart
```

Installation de Amavisd-new, SpamAssassin, And Clamav

Suit l’installation de l’antispam et antivirus, plus leur dépendances, que l’on lancera avec la commande suivante :

```lua
sudo apt-get install amavisd-new spamassassin clamav clamav-daemon zoo unzip bzip2 arj nomarch lzop cabextract apt-listchanges libnet-ldap-perl libauthen-sasl-perl clamav-docs daemon libio-string-perl libio-socket-ssl-perl libnet-ident-perl zip libnet-dns-perl
```

Nous pouvons stopper le processus spamassassin et le supprimer du script de démarrage automatique car ISPConfig a sa propre façon de le lancer via amavisd.

```lua
sudo /etc/init.d/spamassassin stop
sudo update-rc.d -f spamassassin remove
```

Installation de Apache2, PHP5, phpMyAdmin, FCGI, suExec, Pear, And mcrypt

```lua
sudo apt-get install apache2 apache2.2-common apache2-mpm-prefork apache2-utils libexpat1 ssl-cert libapache2-mod-php5 php5 php5-common php5-gd php5-mysql php5-imap phpmyadmin php5-cli php5-cgi libapache2-mod-fcgid apache2-suexec php-pear php-auth php5-mcrypt mcrypt php5-imagick imagemagick libruby
```

Vous devriez avoir à répondre aux questions suivantes :

```lua
Web server to reconfigure automatically: <– apache2
Configure database for phpmyadmin with dbconfig-common? <– No
```

Nous pouvons dès à présent activer les modules qui nous seront utile dans apache :

```lua
sudo a2enmod suexec rewrite ssl actions include
sudo a2enmod dav_fs dav auth_digest
```

Puis, enfin, redémarrer le serveur Apache pour prendre en compte les modifications.

```lua
sudo /etc/init.d/apache2 restart
```

Installation de PureFTPd And Quota

```lua
sudo apt-get install pure-ftpd-common pure-ftpd-mysql quota quotatool
```

Deux vérifications sont à effectuer dans le fichier pure-ftpd-common


```lua
sudo vim /etc/default/pure-ftpd-common

[…]
STANDALONE_OR_INETD=standalone
[…]
VIRTUALCHROOT=true
[…]
```

On peut également vérifier que Inetd ne démarre pas ftp en controlant le fichier inetd.conf :

```lua
sudo vim /etc/inetd.conf

[…]
#:STANDARD: These are standard services.
#ftp stream tcp nowait root /usr/sbin/tcpd /usr/sbin/pure-ftpd-wrapper
[…]
```

Il vous faut alors redémarrer le service.

```lua
sudo /etc/init.d/openbsd-inetd restart
```

Il est maintenant important de configurer notre serveur FTP de manière à utiliser l’authentification TLS. Sans l’utilisation de ce protocole, les noms d’utilisateurs et mot de passe sont transférés en clair lors des communications FTP, ce qui rend le protocole bien peu sécuritaire. L’utilisation de l’encryptage permet de palier grandement à cette lacune.

```lua
sudo bash -c 'echo 1 > /etc/pure-ftpd/conf/TLS'
```

La commande précédente active le protocole TLS. (On crée un fichier nommé TLS dans le répertoire /etc/pure-ftpd/conf . Ce fichier ne contient que la valeur 1).
Il nous faut toutefois créer un certificat SSL pour pouvoir utiliser TLS. Nous créons celui-ci dans le répertoire /etc/ssl/private/ Nous utilisons la commande openssl pour créer notre certificat.

```lua
sudo mkdir -p /etc/ssl/private/
sudo openssl req -x509 -nodes -days 7300 -newkey rsa:2048 -keyout /etc/ssl/private/pure-ftpd.pem -out /etc/ssl/private/pure-ftpd.pem
```

Nous aurons à renseigner un certain nombre de paramètres:

```lua
Country Name (2 letter code) [AU]: <– Code de votre pays (ex: FR)
State or Province Name (full name) [Some-State]: <– Etat ou Département (pas de codification)
Locality Name (eg, city) []: <– Ville (ex: PARIS.)
Organization Name (eg, company) [Internet Widgits Pty Ltd]: <– Société ou organisation.
Organizational Unit Name (eg, section) []: <– Département dans votre société (IT)
Common Name (eg, YOUR name) []: <– Nom qualifié du système (server1.example.com)
Email Address []: <– Adresse email.
```

Nous supprimons ensuite les droits sur le certificat pour les autres membres que root, puis redémarrons le serveur ftp.

```lua
sudo chmod 600 /etc/ssl/private/pure-ftpd.pem
sudo /etc/init.d/pure-ftpd-mysql restart
```

Pour activer le système de quota, il est nécessaire de modifier le fichier /etc/fstab. Sans rentrer dans le détail, il est nécessaire d’ajouter le code ,usrjquota=aquota.user,grpjquota=aquota.group,jqfmt=vfsv0 sur toutes les partitions ou les utilisateurs peuvent potentiellement avoir accès. Si vous n’avez pas partitionné votre disque, il suffit d’ajouter ce code sur la ligne de votre partition de montage (à savoir / )

```lua
sudo vim /etc/fstab

# /etc/fstab: static file system information.

# <file system> <mount point> <type> <options> <dump> <pass>
proc                      /proc                     proc      defaults 0 0
UUID= […]        /                               ext3       errors=remount-ro,usrjquota=aquota.user,grpjquota=aquota.group,jqfmt=vfsv0 0 1
UUID=[…]         none                       swap        sw 0 0
/dev/scd0        /media/cdrom0        udf,iso9660 user,noauto 0 0
/dev/fd0           /media/floppy0        auto rw,user,noauto 0 0
```

Dans ce cas présent, les partitions sont référencée avec leur UUID. On remarque bien que la ligne sur laquelle se trouve le point de montage / comporte notre ajout ,usrjquota=aquota.user,grpjquota=aquota.group,jqfmt=vfsv0 0 1

Si votre serveur est un serveur dédié, et/ou que vous n’avez pas référencé les partitions par leur UUID, il est également possible que votre fstab ressemble à ceci:

```lua
# <file system> <mount point> <type> <options> <dump> <pass>
/dev/md1             /                              ext4        errors=remount-ro,relatime,usrjquota=aquota.user,grpjquota=aquota.group,jqfmt=vfsv0 0 1
/dev/md2            /home                   ext4        defaults,relatime,usrjquota=aquota.user,grpjquota=aquota.group,jqfmt=vfsv0 1 2
/dev/md3            /var                       ext4        defaults,quota,relatime,usrjquota=aquota.user,grpjquota=aquota.group,jqfmt=vfsv0 1 2
/dev/sda7           swap                      swap     defaults 0 0
/dev/sdb7           swap                      swap     defaults 0 0
proc                        /proc                     proc       defaults 0 0
```

On remarquera que dans ce cas précis, j’ai plusieurs partitions (on a donc ajouté le quota a toutes les différentes partitions montables)

Pour activer le système de quota, on lance la commande :

```lua
sudo mount -o remount /
```

```lua
sudo quotacheck -avugm
sudo quotaon -avug
```

Installation de BIND DNS Server

```lua
sudo apt-get install bind9 dnsutils
```

Installation de Vlogger, Webalizer, And AWstats

```lua
sudo apt-get install vlogger webalizer awstats geoip-database
```

Modifiez le fichier /etc/cron.d/awstats

```lua
sudo vim /etc/cron.d/awstats

#*/10 * * * * www-data [ -x /usr/share/awstats/tools/update.sh ] && /usr/share/awstats/tools/update.sh
# Generate static reports:
#10 03 * * * www-data [ -x /usr/share/awstats/tools/buildstatic.sh ] && /usr/share/awstats/tools/buildstatic.sh
```

Installation de Jailkit

```lua
sudo apt-get install build-essential autoconf automake libtool flex bison debhelper
```

```lua
cd /tmp
sudo wget http://olivier.sessink.nl/jailkit/jailkit-2.19.tar.gz
sudo tar -xvfz jailkit-2.19.tar.gz
cd jailkit-2.19
sudo ./debian/rules binary
cd ..
sudo dpkg -i jailkit_2.19-1_*.deb
sudo rm -rf jailkit-2.19*
```
Installation de fail2ban

```lua
apt-get install fail2ban
```

On va configurer le service pour monitorer automatiquement PureFTPd, SASL, et Courier. On commence par le fichier de configuration global:

```lua
vi /etc/fail2ban/jail.local
[pureftpd]

enabled = true
port = ftp
filter = pureftpd
logpath = /var/log/syslog
maxretry = 3
[sasl]

enabled = true
port = smtp
filter = sasl
logpath = /var/log/mail.log
maxretry = 5
[courierpop3]

enabled = true
port = pop3
filter = courierpop3
logpath = /var/log/mail.log
maxretry = 5
[courierpop3s]

enabled = true
port = pop3s
filter = courierpop3s
logpath = /var/log/mail.log
maxretry = 5
[courierimap]

enabled = true
port = imap2
filter = courierimap
logpath = /var/log/mail.log
maxretry = 5
[courierimaps]

enabled = true
port = imaps
filter = courierimaps
logpath = /var/log/mail.log
maxretry = 5
```

Puis on crée les fichiers de filtre associés comme suit:

```lua
vi /etc/fail2ban/filter.d/pureftpd.conf
[Definition]
failregex = .*pure-ftpd: \(.*@<HOST>\) \[WARNING\] Authentication failed for user.*
ignoreregex =
```

```lua
vi /etc/fail2ban/filter.d/courierpop3.conf
# Fail2Ban configuration file
#
# $Revision: 100 $
#

[Definition]

# Option: failregex
# Notes.: regex to match the password failures messages in the logfile. The
# host must be matched by a group named « host ». The tag « <HOST> » can
# be used for standard IP/hostname matching and is only an alias for
# (?:::f{4,6}:)?(?P<host>\S+)
# Values: TEXT
#
failregex = pop3d: LOGIN FAILED.*ip=\[.*:<HOST>\]

# Option: ignoreregex
# Notes.: regex to ignore. If this regex matches, the line is ignored.
# Values: TEXT
#
ignoreregex =
```

```lua
vi /etc/fail2ban/filter.d/courierpop3s.conf
# Fail2Ban configuration file
#
# $Revision: 100 $
#

[Definition]

# Option: failregex
# Notes.: regex to match the password failures messages in the logfile. The
# host must be matched by a group named « host ». The tag « <HOST> » can
# be used for standard IP/hostname matching and is only an alias for
# (?:::f{4,6}:)?(?P<host>\S+)
# Values: TEXT
#
failregex = pop3d-ssl: LOGIN FAILED.*ip=\[.*:<HOST>\]

# Option: ignoreregex
# Notes.: regex to ignore. If this regex matches, the line is ignored.
# Values: TEXT
#
ignoreregex =
```

```lua
vi /etc/fail2ban/filter.d/courierimap.conf
# Fail2Ban configuration file
#
# $Revision: 100 $
#

[Definition]

# Option: failregex
# Notes.: regex to match the password failures messages in the logfile. The
# host must be matched by a group named « host ». The tag « <HOST> » can
# be used for standard IP/hostname matching and is only an alias for
# (?:::f{4,6}:)?(?P<host>\S+)
# Values: TEXT
#
failregex = imapd: LOGIN FAILED.*ip=\[.*:<HOST>\]

# Option: ignoreregex
# Notes.: regex to ignore. If this regex matches, the line is ignored.
# Values: TEXT
#
ignoreregex =
```

```lua
vi /etc/fail2ban/filter.d/courierimaps.conf
# Fail2Ban configuration file
#
# $Revision: 100 $
#

[Definition]

# Option: failregex
# Notes.: regex to match the password failures messages in the logfile. The
# host must be matched by a group named « host ». The tag « <HOST> » can
# be used for standard IP/hostname matching and is only an alias for
# (?:::f{4,6}:)?(?P<host>\S+)
# Values: TEXT
#
failregex = imapd-ssl: LOGIN FAILED.*ip=\[.*:<HOST>\]

# Option: ignoreregex
# Notes.: regex to ignore. If this regex matches, the line is ignored.
# Values: TEXT
#
ignoreregex =
```

Il ne reste alors plus qu’à redémarrer le service.

```lua
/etc/init.d/fail2ban restart
```

Téléchargement de ISPConfig :

```lua
sudo wget https://sourceforge.net/projects/ispconfig/files/ISPConfig%203/ISPConfig-3.1.1p1/ISPConfig-3.1.1p1.tar.gz
sudo tar -xvzf ISPConfig-3.1.1p1.tar.gz
```

```lua
cd ispconfig3_install/install/
sudo php -q install.php
```

http://nomdeserver.votrenomdedomaine.com:8080


en cours

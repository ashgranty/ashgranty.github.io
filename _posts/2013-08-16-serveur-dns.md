---
layout: post
title: Serveur DNS
description: "DNS"
modified: 2014-12-23
tags: [dns, serveur, server]
---
Text

{% highlight css %}
Plus par volonté d’apprendre que réelle nécessité, j’ai décidé de mettre en place un serveur DNS sur mon réseau local.
Voici donc un tutoriel sur comment mettre en place un tel serveur.
Le serveur sera capable de résoudre des noms en ip des ips en noms et ceci dans 3 zones.
Première chose télécharger et installer Bind:

sudo apt-get install bind9
Une fois ceci fait on passe à la configuration qui se fait en premier lieu dans /etc/bind/named.conf

vim /etc/bind/named.conf
zone "mon_domaine.fr" {
type master;
file "/etc/bind/db.mon_domaine.fr";
};

zone "0.168.192.in-addr.arpa" {
type master;
file "/etc/bind/db.192.168.0";
};

zone "0.0.10.in-addr.arpa" {
type master;
file "/etc/bind/db.10.0.0";
};
Voici donc les 3 zones créées. Quelques explications:

La première zone correspond à la résolution des noms par exemple si vous cherchez à atteindre la machine machine1 ayant pour IP 10.0.0.51 vous taperez machine1 au lieu de son IP. Le champ « type » concerne la redondance du serveur. Ici il s’agit du master.
Enfin le champ file correspond à la location du fichier de configuration de la zone, fichier dont le nom commence toujours par db.

Les deuxième et troisième zones correspondent à la résolution inverse. A savoir que si vous tapez 10.0.0.51 le serveur saura que ca correspond à la machine machine1.mon_domaine.fr
Prêtez attention au format pour les noms de zones: 0.0.10.in-addr.arpa correspond au réseau 10.0.0.0 représentant une zone.
Si votre réseau était en 172.168.0.0 vous devriez écrire 0.168.172.in-addr.arpa.

Une fois les zones créées on passe ensuite aux fichiers de configuration des zones.

vim /etc/bind/db.mon_domaine.fr

@	IN	SOA	ns.mon_domaine.fr.	julien.mon_domaine.fr. (
2009110701
24H
2H
1000H
2D )

@	IN	NS	ns.mon_domaine.fr.

ns	A	192.168.0.250
machine1	A	10.0.0.51
machine2	A	192.168.0.1
machine3	A	192.168.0.248
machine4	A	10.0.0.60
machine5	A	192.168.0.250
La première ligne fait référence au server DNS primaire:
@ : tient pour la zone en question ici mon_domaine.fr
IN : signifie que c’est une zone de type internet
SOA : Start Of Authority qui signifie que l’on a à faire au serveur principal
julien.technoaddict.fr. : adresse email du responsable (le . à la fin est primordial)
La parenthèse à la fin fait partie de la syntaxe.

Ensuite on trouve une série de chiffres cabalistiques !

2009110701 : est le numéro de série ici une date mettez la même chose à la date du jour où vous créez le serveur
24H : le rafraichissement du serveur
2H : la période entre 2 tentatives de rafraichissement
1000H : la valeur d’expiration
2D : la valeur time to live
On n’oublie pas la parenthèse à la fin.

@	IN	NS	ns.mon_domaine.fr. : est l’adresse du serveur à interroger pour la zone définie vous noterez le . à la place de @.

On liste ensuite les différentes entrées de la zone, à savoir les machines susceptibles d’être contactées.
On rentre les hostnames des machines puis une tabulation,le champ A,une tabulation et l’IP associée.
Dans le fichier la première ligne ubuntu est suivie du champ NS (Name Server) indiquant que l’on a à faire au serveur de nom.

Afin de pouvoir réaliser la résolution inverse nous créons un fichier par zone d’adresses:

vim /etc/bind/db.192.168.0
@ IN SOA ns.mon_domaine.fr. julien.mon_domaine.fr. (
2009110701
24H
2H
1000H
2D )

@ IN NS ns.mon_domaine.fr.

XX	IN	PTR	ns.mon_domaine.fr.
XX	IN	PTR	machine1.mon_domaine.fr.
XX	IN	PTR	machine2.mon_domaine.fr.
XX	IN	PTR	machine3.mon_domaine.fr.
Vous remarquerez qu’il s’agit quasiment de la même chose que le premier fichier à ceci près que nous utilisons le IN et PTR à la place de A.

La même chose pour la 2e zone:

vim /etc/bind/db.10.0.0

@ IN SOA ns.mon_domaine.fr. julien.mon_domaine.fr. (
2009110701
24H
2H
1000H
2D )

@ IN NS ns.mon_domaine.fr.

XX	IN	PTR	machine1.mon_domaine.fr.
XX	IN	PTR	machine2.mon_domaine.fr.
XX	IN	PTR	machine3.mon_domaine.fr.
XX	IN	PTR	machine4.mon_domaine.fr.
Une fois tous les fichiers de configuration écrits vous pouvez redémarrer le serveur :

/etc/init.d/bind9 restart
Ca y est votre réseau est désormais équipé avec un serveur dns ! Vous pouvez dès lors faire ssh user@machine1 plutôt que user@10.0.0.1
{% endhighlight %}

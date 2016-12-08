---
layout: post
title: Serveur DNS
description: "DNS"
modified: 2014-12-23
tags: [dns, serveur, server]
---

Télécharger Bind :

```lua
sudo apt-get install bind9
```

Configuration :

```lua
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
```

Fichiers de configuration des zones :

```lua
vim /etc/bind/db.mon_domaine.fr

@	IN	SOA	ns.mon_domaine.fr.	nom.mon_domaine.fr. (
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
```

Afin de pouvoir réaliser la résolution inverse nous créons un fichier par zone d’adresses :

```lua
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
```

```lua
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
```

```lua
/etc/init.d/bind9 restart
```

Vérification :

```lua
ssh user@machine1
```

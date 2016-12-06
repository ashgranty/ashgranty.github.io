---
layout: post
title: Nginx
description: "Serveur Web"
tags: [réseau, serveur, web, apache, nginx]
---

Coupler Nginx (front-end) à Apache 2.4 (back-end)

> Prérequis :

> - Debian Jessie
> - Apache2

Modification du port d'écoute de Apache :
sudo vim /etc/apache2/ports.conf

Listen 8080

<IfModule ssl_module>
        Listen 443
</IfModule>

<IfModule mod_gnutls.c>
        Listen 443
</IfModule>


Modifier les VirtualHost :
sudo vim /etc/apache2/sites-available/000-default.conf

Téléchargemebt de Nginx :
sudo vim /etc/apt/sources.list
deb http://packages.dotdeb.org jessie all
deb http://nginx.org/packages/mainline/debian/ jessie nginx

wget http://www.dotdeb.org/dotdeb.gpg
sudo apt-key add dotdeb.gpg

wget http://nginx.org/keys/nginx_signing.key
sudo apt-key add nginx_signing.key


sudo apt-get update


sudo apt-get install nginx





Configuration de Nginx
Fichier proxi :
sudo vim /etc/nginx/conf.d/proxy.conf

proxy_redirect          off;
proxy_set_header        Host            $host;
proxy_set_header        X-Real-IP       $remote_addr;
proxy_set_header        X-Forwarded-For $proxy_add_x_forwarded_for;
client_max_body_size    10m;
client_body_buffer_size 128k;
proxy_connect_timeout   90;
proxy_send_timeout      90;
proxy_read_timeout      90;
proxy_buffer_size   16k;
proxy_buffers       32   16k;
proxy_busy_buffers_size 64k;


EN COURS

<!-- more -->

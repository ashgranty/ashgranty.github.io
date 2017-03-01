---
layout: post
title: Fusion Inventory et GLPI
description:
tags: [GLPI, Inventaire, FusionInventory, Debian, server]
---


Tache Cron pour GLPI
Il suffit d’éditer le cron de l’utilisateur du serveur web qui fait tourner GLPI.

```lua
sudo crontab -u www-data -e
```

Le chemin de GLPI doit correspondre à celui de votre installation

```lua
*/1 * * * * /usr/bin/php5 /var/www/glpi/front/cron.php &>/dev/null
```

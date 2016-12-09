---
layout: post
title: Serveur de test sous virtualbox
description: "Debian server"
tags: [debian, serveur, test, virtualbox]
---

Prérequis :
> Une debian fraîchement installée

I. Présentation
II. Un snapshot, c’est quoi ?
III. Prendre un snapshot
IV. Détail du snapshot
V. Effet sur l’arborescence de fichier
VI. Restauration à partir d’un snapshot
VII. Cloner une machine à partir d’un snapshot
VIII. Supprimer les snapshot

I. Présentation
Avec VirtualBox, il est possible de prendre des snapshots de nos machines virtuelles très simplement. Nous allons voir dans ce tutoriel le détail de l’utilisation des snaphosts avec VirtualBox.

Il est important de savoir que le mot « snapshot » est traduit par « Instantané » en français.

II. Un snapshot, c’est quoi ?
Un snapshot est une sorte de point de restauration que l’on fait sur n’importe quelle machine virtuelle. Cela va nous permettre de pouvoir revenir sur ce point de restauration plus tard.

III. Prendre un snapshot
Il est très facile de prendre un snapshot sur VirtualBox. Il suffit en effet de sélectionner la machine virtuelle sur laquelle vous souhaitez faire une sauvegarde puis de cliquer sur l’icône en forme d’appareil photo à droite.


Vous pouvez alors nommer et décrire votre snapshot pour faciliter l’organisation.

Vous verrez ensuite un début d’arborescence apparaitre sur la fenêtre de droite quand vous êtes sur l’onglet snapshot.


On y voit le nom des snapshots faits ainsi que le « Current State » qui est l’état sur lequel la machine tourne actuellement.

IV. Détail du snapshot
Il est possible de voir le détail d’un snapshot en cliquant dessus puis en cliquant sur l’appareil photo avec le point jaune.


Nous voyons alors la description du snapshot ainsi que ses caractéristiques matérielles au moment où celui-ci a été fait.

V. Effet sur l’arborescence de fichier
Si vous vous rendez dans le répertoire contenant les fichiers de votre VM, vous verrez qu’un dossier « Snapshots » a été créé.


En effet, les snapshots se stockent dans le répertoire de la VM dont ils dépendent. Un snapshot = un fichier, ainsi la suppression ou la création d’un snapshot correspond à la suppression ou la création d’un fichier.


Quand un snapshot est fait, toute l’écriture sur le disque virtuel se fait sur ce nouveau fichier. Quand on souhaite revenir sur un ancien snapshot, le fichier est supprimé et VirtualBox se met à lire sur le dernier fichier en date (qui sera notre snapshot le plus récent ou celui sur lequel nous sommes revenu).

VI. Restauration à partir d’un snapshot
Si vous souhaitez restaurer votre VM au snapshot que vous avez pris, il suffit de faire clic droit sur le snapshot voulu puis « restaurer » ou alors cliquer sur l’écran avec une flèche montante au dessus de l’arborescence.

Vous verrez alors le « Current State » changer de place est se lier directement au snapshot à partir duquel vous l’avez restauré. Cela est visible si vous avez fait plusieurs snapshots.


VII. Cloner une machine à partir d’un snapshot
Vous pouvez créer une toute nouvelle machine virtuelle à partir d’un snapshot que vous avez pris, cela est pratique dans le cadre de tests par exemple.

Pour cela il faut faire clic droit sur le snapshot voulu puis cliquer sur « Cloner » ou cliquer sur le petit mouton en haut, renommez votre clone est lancez la copie.


Lors de la création d’un clone à partir d’un snapshot, c’est le fichier du snapshot qui se transforme en disque dur virtuel à part entière. Pour créer la nouvelle machine, VirtualBox « consolide » le snapshot dans un disque dur virtuel et une machine dédiée

VIII. Supprimer les snapshot
Une fois qu’un snapshot devient trop ancien ou inutile, vous pouvez le supprimer pour vous faire un peu de place. Il suffit pour cela de faire clic droit sur votre snapshot puis « Supprimer » ou clic sur l’écran avec la croix rouge.

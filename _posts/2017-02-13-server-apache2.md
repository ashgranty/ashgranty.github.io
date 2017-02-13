---
layout: post
title: Serveur Apache2
description:
tags: [apache2, server]
---


```lua
debian@debian:~$ sudo apt-get install apache2
Reading package lists... Done
Building dependency tree
Reading state information... Done
The following extra packages will be installed:
  apache2-bin apache2-data apache2-utils file libalgorithm-c3-perl libapr1
  libaprutil1 libaprutil1-dbd-sqlite3 libaprutil1-ldap libarchive-extract-perl
  libcgi-fast-perl libcgi-pm-perl libclass-c3-perl libclass-c3-xs-perl
  libcpan-meta-perl libdata-optlist-perl libdata-section-perl libfcgi-perl
  libldap-2.4-2 liblog-message-perl liblog-message-simple-perl liblua5.1-0
  libmagic1 libmodule-build-perl libmodule-pluggable-perl
  libmodule-signature-perl libmro-compat-perl libpackage-constants-perl
  libparams-util-perl libpod-latex-perl libpod-readme-perl
  libregexp-common-perl libsasl2-2 libsasl2-modules libsasl2-modules-db
  libsoftware-license-perl libsqlite3-0 libsub-exporter-perl
  libsub-install-perl libterm-ui-perl libtext-soundex-perl
  libtext-template-perl libxml2 mime-support openssl perl perl-modules rename
  sgml-base ssl-cert xml-core
Suggested packages:
  www-browser apache2-doc apache2-suexec-pristine apache2-suexec-custom
  libsasl2-modules-otp libsasl2-modules-ldap libsasl2-modules-sql
  libsasl2-modules-gssapi-mit libsasl2-modules-gssapi-heimdal ca-certificates
  perl-doc libterm-readline-gnu-perl libterm-readline-perl-perl make
  libb-lint-perl libcpanplus-dist-build-perl libcpanplus-perl
  libfile-checktree-perl libobject-accessor-perl sgml-base-doc
  openssl-blacklist debhelper
Recommended packages:
  libarchive-tar-perl
The following NEW packages will be installed:
  apache2 apache2-bin apache2-data apache2-utils file libalgorithm-c3-perl
  libapr1 libaprutil1 libaprutil1-dbd-sqlite3 libaprutil1-ldap
  libarchive-extract-perl libcgi-fast-perl libcgi-pm-perl libclass-c3-perl
  libclass-c3-xs-perl libcpan-meta-perl libdata-optlist-perl
  libdata-section-perl libfcgi-perl libldap-2.4-2 liblog-message-perl
  liblog-message-simple-perl liblua5.1-0 libmagic1 libmodule-build-perl
  libmodule-pluggable-perl libmodule-signature-perl libmro-compat-perl
  libpackage-constants-perl libparams-util-perl libpod-latex-perl
  libpod-readme-perl libregexp-common-perl libsasl2-2 libsasl2-modules
  libsasl2-modules-db libsoftware-license-perl libsqlite3-0
  libsub-exporter-perl libsub-install-perl libterm-ui-perl
  libtext-soundex-perl libtext-template-perl libxml2 mime-support openssl perl
  perl-modules rename sgml-base ssl-cert xml-core
0 upgraded, 52 newly installed, 0 to remove and 0 not upgraded.
Need to get 11.0 MB of archives.
After this operation, 46.2 MB of additional disk space will be used.
Do you want to continue? [Y/n]
```

```lua
sudo vim /etc/apache2/sites-available/000-default.conf
DocumentRoot /var/www
```

---
layout: post
title: Debian et les GuestAdditions
description:
tags: [VirtualBox, Debian, server]
---


enable the contrib repositories; e.g. for Debian 8, make sure your /etc/apt/sources.list contains something like

```lua
deb http://ftp.debian.org/debian jessie main contrib
```

install virtualbox-guest-dkms, kernel headers, and, optionally, virtualbox-guest-x11 (for the graphical guest utilities):

```lua
sudo apt-get update
sudo apt-get install virtualbox-guest-dkms virtualbox-guest-x11 linux-headers-$(uname -r)
```

This will build the appropriate kernel modules automatically (and rebuild them when the kernel is upgraded), and install the guest additions.

(Note that this will install the version of the guest additions available in whichever version of Debian you're using in the VM, which may not match the version of Virtual Box running the VM — but the guest additions should still work fine.)

---
title: "Migration of Server & Swap Space"
author: "Russ Mckendrick"
date: 2015-03-08T16:37:38.000Z
lastmod: 2021-07-31T12:32:41+01:00

tags:
 - Tech
 - Centos
 - Digital Ocean
 - Server

cover:
    image: "/img/2015-03-08_migration-of-server-swap-space_0.png" 
images:
 - "/img/2015-03-08_migration-of-server-swap-space_0.png"


aliases:
- "/migration-of-server-swap-space-ef1da66dc9f1"

---

I noticed yesterday that my DigitalOcean monthly spend was a little more than I was expecting, turns out I have launched the wrong instance size when I last rebuilt my machine, so to save myself a few dollars a month I migrated the instance to a fresh server with a lower spec.

By default DigitalOcean servers do not come with [swap](http://en.wikipedia.org/wiki/Virtual_memory "Swap") enabled, because of this you run the risk of [oomkiller](http://en.wikipedia.org/wiki/Out_of_memory "oomkiller") killing processes to try and keep the server accessible. Luckily as all DigitalOcean servers are SSD backed and come with a decent amount of disk space so enabling swap isn’t an issue;

```
fallocate -l 4G /swapfile
chmod 600 /swapfile
mkswap /swapfile
swapon /swapfile
echo “/swapfile none swap sw 0 0” >> /etc/fstab
```

as you can see the server now has a 1GB of RAM and 4GB of swap;

```
[root@server ~]# swapon -s
Filename Type Size Used Priority /swapfile file 4194300 96460 -1 
root@server ~]# free -m
total used free shared buffers cached Mem: 
994 919 74 3 2 55
-/+ buffers/cache: 860 133
Swap: 4095 94 4001
```

There are also a few other settings you can tune to help with performance;

```
sysctl vm.swappiness=10
sysctl vm.vfs_cache_pressure=50
echo “## Make Swap betterer” >> /etc/sysctl.conf
echo “vm.swappiness = 10” >> /etc/sysctl.conf
echo “vm.vfs_cache_pressure = 50” >> /etc/sysctl.conf
```

These two options do the following;

- swappiness — this control is used to define how aggressively the kernel swaps out anonymous memory relative to pagecache and other caches. Increasing the value increases the amount of swapping.
- vfs_cache_pressure — this variable controls the tendency of the kernel to reclaim the memory which is used for caching of VFS caches, versus pagecache and swap. Increasing this value increases the rate at which VFS caches are reclaimed.

```
[root@server ~]# sysctl vm.swappiness
vm.swappiness = 10
[root@server ~]# sysctl vm.vfs_cache_pressure
vm.vfs_cache_pressure = 50
[root@server ~]#
```

This will hopefully save me a little money and make sure that things like MySQL don’t randomly get killed :)

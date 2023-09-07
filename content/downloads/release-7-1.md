---
title: "Release 7.1.X"
draft: false
menu:
    side:
        weight: -71
---
dCache 7.1 is a Feature Release, introducing following highlights:
- The NFS door is updated to allow create, update and remove of directory tags as extended attributes
- This version introduces `chunked unpinning`
#### Incompatibilities
- Dropped compatibility with version 5.2 and older
- Dropped sysV-like service file `/etc/rc.d/init.d/dcache-server`
- Dropped `remove-precious-files-on-delete` property
- the debian package explicitly depends on `rsyslog` package
dCache v7.1 requires a JVM supporting  Java 11.

{{< release-table >}}

{{< releases-7-1 >}}

{{</ release-table >}}

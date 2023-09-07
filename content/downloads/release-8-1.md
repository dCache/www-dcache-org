---
title: "Release 8.1.X"
draft: false
menu:
    side:
        weight: -81
---
dCache 8.1 is a Feature Release introducing following highlights:
- `webdav.redirect.allow-https` and `pool.enable.encrypted-transfers` are now set by default to true
#### Incompatibilities
- the `org.dcache.net.localaddresses` accepts host names
- NFS transfers will fail with EIO for broken files
dCache v8.1 requires a JVM supporting  Java 11.

{{< release-table >}}

{{< releases-8-1 >}}

{{</ release-table >}}

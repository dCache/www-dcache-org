---
title: "Release 9.0.X"
draft: false
menu:
    side:
        weight: -90
---
dCache 9.0 is a Feature Release introducing following highlights:
- dCache now supports Java 17 as its platform
#### Incompatibilities
- DCAP and NFS doors will fail the request if file's storage unit is not configured in PoolManager
- linklocal and localhost interfaces are not published by doors and pools
- DCAP movers always start in passive mode
- removed experimental message encoding format
- removed default HSM operation timeout

{{< release-table >}}

{{< releases-9-0 >}}

{{</ release-table >}}

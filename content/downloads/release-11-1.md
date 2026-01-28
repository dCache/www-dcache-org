---
title: "Release 11.1.X"
draft: false
---
dCache 11.1 is a Feature Release introducing following highlights:
- NFSv4.1 Read-delegation support


#### Incompatibilities
-  Dropped support for nfsv4_1_files layout type
-  Dropped admin commands to set/get/list `flags` associated with files
-  `pool.enable.hsm-flag` became obsolete
-  `t_storageinfo` chimera table is deprecated 
-  Removed legacy admin shell

_Starting with dcache 10.1.0 Java 17 is required for runtime_

{{< release-table >}}

{{< releases-11-1 >}}

{{</ release-table >}}


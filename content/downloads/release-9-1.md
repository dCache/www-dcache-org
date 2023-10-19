---
title: "Release 9.1.X"
draft: false
---
dCache 9.1 is a Feature Release introducing following highlights:
- dCache now supports Java 17 as its platform
- Improve concurrent file create rate in a single directory
#### Incompatibilities
- Starting version 9.1 the nlink count for directories shows only number of subdirectories. Thus, the existing
- dropped gplazma support for XACML
- pool binds TCP port for `http` and `xroot` movers on startup

{{< release-table >}}

{{< releases-9-1 >}}

{{</ release-table >}}

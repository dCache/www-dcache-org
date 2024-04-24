---
title: "Release 7.0.X"
draft: false
---
dCache 7.0 is a Feature Release, introducing following highlights:
- dCache internal copy triggerd by `srmcp` uses HTTP, was DCAP
- The high available services PinManager and Cleaner have acquired new debug commands
#### Incompatibilities
- The argument of `kill client` admin command of NFS door accepts client session id
- Dropped support for pcells GUI
dCache v7.0 requires Java 11.

{{< release-table >}}

{{< releases-7-0 >}}

{{</ release-table >}}

---
title: "Release 11.2.X"
draft: false
---
dCache 11.2 is a Golden Release introducing following highlights:

- NFS read performance improvement
- Kafka logging for billing
- Introduce hot file replication mechanism on pools, where the pool itself triggers replication of hot (popular) files

#### Incompatibilities
- Xrootd PrepareRequest will return Unsupported

_Starting with dcache 10.1.0 Java 17 is required for runtime_

{{< release-table >}}

{{< releases-11-2 >}}

{{</ release-table >}}


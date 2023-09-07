---
title: "Release 8.2.X"
draft: false
menu:
    side:
        weight: -82
---
dCache 8.2 is a Golden Release introducing the following highlights:
- Support for nested groups in the PoolManager
- The bulk service has undergone a major revision
- A new TAPE API has been made available for the purposes of bulk staging
#### Incompatibilities
- pool group names cannot start with `@` symbol
- hard links on directories is not allowed
- GET `api/v1/bulk-requests` has a new return value
dCache v8.2 requires a JVM supporting  Java 11.

{{< release-table >}}

{{< releases-8-2 >}}

{{</ release-table >}}

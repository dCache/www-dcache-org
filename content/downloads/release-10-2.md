---
title: "Release 10.2.X"
draft: false
---
dCache 10.2 is a Golden Release introducing the following highlights:

- Expose user/group quotas via remote quota protocol
- Java 21 is supported as runtime environment
- PoC support for firefly network markers
- Support for label-based virtual read-only directories

#### Incompatibilities

- Poolmanager partitions by default allow staging
- Dropped the gPlazma ARGUS plugin
- Cancelling a stage request in poolmanager will propagate the cancellation to the pool
- The pool-internal health check is disabled if an external check command is configured
- Access to files with QoS HSM-only will be denied, even if they are still available on disk

_Starting with dcache 10.1.0 Java 17 is required for runtime_


{{< release-table >}}

{{< releases-10-2 >}}

{{</ release-table >}}


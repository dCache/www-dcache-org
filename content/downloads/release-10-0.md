---
title: "Release 10.0.X"
draft: false
---
dCache 10.0 is a Feature Release introducing following highlights:
- Added pool metadata directory configuration option
- Use environment variables as configuration properties

#### Incompatibilities
- dropped native CEPH support. Sites must migrate their pools before updating dCache
- dropped idle timeout handler in netty based movers (xroot, http)

dCache v10.0 requires a JVM supporting Java 11 or Java 17.

{{< release-table >}}

{{< releases-10-0 >}}

{{</ release-table >}}

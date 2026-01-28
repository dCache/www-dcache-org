---
title: "Release 10.0.X"
draft: false
---
dCache 10.0 is a Feature Release introducing following highlights:
- Added pool metadata directory configuration option
- Use environment variables as configuration properties

#### Incompatibilities
- Dropped native CEPH support. (_Sites must migrate their pools before updating dCache_ )
- Dropped idle timeout handler in netty based movers (xroot, http)

_dCache 10.0 requires a JVM supporting Java 11 or Java 17_

{{< release-table >}}

{{< releases-10-0 >}}

{{</ release-table >}}

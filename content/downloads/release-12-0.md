---
title: "Release 12.0.X"
draft: false
---
dCache 12.0 is a Feature Release introducing following highlights:

- Zone-aware pool selection (PoC) – dCache now supports directing clients to pools closest to them geographically or topologically, useful in multi-site deployments.
- OIDC Authorization Code Flow support in dCache View (PoC).

#### Incompatibilities
- Removed SRM and SRM server functionality.
- Removed NIS support
- Removed the gridmap-file gPlazma plugin. Sites must migrate to the multimap plugin.

_Starting with dcache 10.1.0 Java 17 is required for runtime_

{{< release-table >}}

{{< releases-12-0 >}}

{{</ release-table >}}

---
title: "Release 9.2.X"
draft: false
---
dCache 9.2 is a Golden Release introducing following highlights:
- Performance improvements for the concurrent directory creation and removal

#### Incompatibilities
- Prior to version 9.2 dCache NFSv4.1 door published only `nfs4_1_files` layout. Now the door publishes all available layout types
- Dropped reference count tracking for directory tags
- Dropped support of java options `chimera_soft_update` and `chimera_lazy_wcc` in favor of `chimera.attr-consistency` property

dCache v9.2 requires a JVM supporting Java 11 or Java 17.

{{< release-table >}}

{{< releases-9-2 >}}

{{</ release-table >}}

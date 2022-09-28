---
title: "Main"
draft: false
menu: main
weight: 1
---

## Latest 8.2 LTS (Golden) release

We are very happy to announce the release of [dCache 8.2.0][1], the first release in the 8.x series.
The 8.2 series is a long term supported branch and will be maintained until fall 2024.

### Some of the highlights of dCache 8.2

- support for nested groups in the PoolManager
- a new TAPE API has been made available for the purposes of bulk staging

### Incompatibilities

- pool group names can’t start with @ symbol
- hard links on directories is not allowed
- GET api/v1/bulk-requests has a new return value

For full details on these highlights and all other changes, check the [8.2 release notes][2].

[1]: https://www.dcache.org/old/downloads/1.9/index.shtml#server-8.2
[2]: https://www.dcache.org/old/downloads/1.9/release-notes-8.2.shtml

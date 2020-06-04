---
title: "Release 6.1.4"
date: 2020-06-02T00:17:15+02:00
draft: false
date: 2020-06-01T22:42:28+02:00
package: dCache
version: "6.1.4"
series_base: "6.1"
url_base: "https://www.dcache.org/downloads/1.9/repo"
notes: "https://www.dcache.org/downloads/1.9/release-notes-6.1.shtml"
packages:
    -
        type: "rpm"
        filename: "dcache-6.1.4-1.noarch.rpm"
        checksum: "772408613df65e8d353a51fe8aa913e9"
    -
        type: "deb"
        filename: "dcache_6.1.4-1_all.deb"
        checksum: "0fb637f7dcd158bfd92dcd9b9c56a1e6"
    -
        type: "tgz"
        filename: "dcache-6.1.4.tar.gz"
        checksum: "145f1d9b578adb508eefe8d64a55fadc"
---

## frontend

The frontend’s inotify-over-SSE now honour the ‘frontend.root’ configuration property and any user-specific root. This means that, for any inotify subscription, the path is calculated relative to the doors’ root and the user-specific root. For many dCache sites, this has no impact as they are using the default for both; however, the user root is used to implement the macaroon’s ‘root’ caveat. The current release fixed how inotify subscription requests are processed when made with a macaroon with a ‘root’ caveat.

## pool

When making an HTTP third-party copy (HTTP-TPC), dCache no longer sends the ‘Authorization’ HTTP request header in any subsequent request when the remote server responds with a redirection.

The unix xrootd tpc security plugin was included in order to enable the dCache TPC client to use a dCache pool as source when signed hash verification is on. However, this is now fixed and no special configuration necessary for organizations (like Tier 1) needing to communicate with EOS.
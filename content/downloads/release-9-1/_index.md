---
title: "Server 9.1"
draft: false
weight: 3
---
## Highlights

- Improve concurrent file create rate in a single directory

## Incompatibilities

- Starting with version 9.1 the nlink count for directories shows only number of subdirectories. Thus, the existing
  nlink count can be out-of-sync with we no automatic re-synchronization is performed
- dropped gplazma support for XACML
- pool binds TCP port for `http` and `xroot` movers on startup
- With version 9.1.16+, empty and non-existent banfiles will be treated the same



### Bulk

There have been a few smaller bug fixes which improve consistency in the request info returned to the user,
in treating non-existent vs invalid paths differently, and in counting targets according to state
in the admin shell.  Propagation of the Subject has also been added to PIN and STAGE requests so
as to assure that PinManager does the correct permission checks.  Finally, the request policy
defaults have been changed to conform with the most prevalent usage (WLCG TAPE); concurrent requests per user
is now set to 5000, with the size of each request set to 500 targets.

### Chimera, Core, Security, Bulk, Frontend, Xroot

With dCache 9 we have introduced symlink resolution on both target paths and user roots/prefixes.
This means that restrictions for ACLs as well as token claims will now resolve both
the allowable path as well as the target path before making a determination as to permissions.

### Frontend

A new REST API has been added which allows one to retrieve information concerning pool migration jobs,
`/api/v1//migrations`.  See the SWAGGER page for description and parameters.

Several other enhancements have been made, including a more detailed description of Bulk request objects,
improved error messages, improved SWAGGER annotations for the TAPE API and for paging offsets on Bulk requests.
Finally, support for relative paths in the target list of Bulk and TAPE API requests has been enabled.

### gPlazma

Besides dropping the XACML plugin and discontinuing gPlazma 1 (making Revocation entries defunct),
the OIDC plugin has been modified to support the suppress option, to allow suppression of offline
verification, and to allow the suppression of audience claim verification.  Documentation
on OIDC has been added ("Introduction to multimap and OIDC Plug-ins in dCache") to `The Book`.
Selected JWT claims are now also printed for failed logins.

### Packaging

The rpm package will call `systemctl daemon-reload` after install or update.

### Pinmanager

### PnfsManager

The chimera layer supports two new Java properties that control parent directory attribute update on create:

 - chimera_lazy_wcc
 - chimera_soft_update
 
 The `chimera_lazy_wcc` option instructs dCache not to update directory attributes immediately. Instead, the desired information is stored
 in an additional `t_lazy_wcc` table that is periodically merged into `t_inodes`. This behavior improves the concurrent file creation rate for a single directory.
 The attribute syncronization time is hard-coded to 30 seconds.
 
 The `chimera_soft_update` option tells dCache to check for pending updates in `t_lazy_wcc` on `stat` calls. This option should be enabled if the 30-second
 attribute merge time can't be used (typically for NFS write based workloads).


### Pool

Added possibility to dynamically add and remove nearline storage providers; this allows one to update the provider without
restarting dCache:

```
    (pool_write@dCacheDomain) admin > hsm show providers
    PROVIDER DESCRIPTION                                                 DYNAMIC
    script   Calls out to an HSM integration script.                     no
    link     Hard links flushed files in another directory.              no
    copy     Copies files to and from another directory.                 no
    tar      Bundles files into tar archives (not ready for production). no
    
    (pool_write@dCacheDomain) admin > hsm load provider /tmp/dcache-cta-0.6.0
    New providers:
    PROVIDER   DESCRIPTION
    dcache-cta dCache Nearline Storage Driver for CTA. Version: 0.6.0 2022-07-20T09:33:38Z
    
    (pool_write@dCacheDomain) admin > hsm show providers
    PROVIDER   DESCRIPTION                                                                 DYNAMIC
    script     Calls out to an HSM integration script.                                      no
    link       Hard links flushed files in another directory.                               no
    copy       Copies files to and from another directory.                                  no
    tar        Bundles files into tar archives (not ready for production).                  no
    dcache-cta dCache Nearline Storage Driver for CTA. Version: 0.6.0 2022-07-20T09:33:38Z yes
    
    (pool_write@dCacheDomain) admin > hsm unload provider dcache-cta
    Removed providers:
    PROVIDER   DESCRIPTION
    dcache-cta dCache Nearline Storage Driver for CTA. Version: 0.6.0 2022-07-20T09:33:38Z
    
    (pool_write@dCacheDomain) admin > hsm show providers
    PROVIDER DESCRIPTION                                                 DYNAMIC
    script   Calls out to an HSM integration script.                     no
    link     Hard links flushed files in another directory.              no
    copy     Copies files to and from another directory.                 no
    tar      Bundles files into tar archives (not ready for production). no
    
    (pool_write@dCacheDomain) admin > hsm load provider /tmp/dcache-cta-0.7.0
    New providers:
    PROVIDER   DESCRIPTION
    dcache-cta dCache Nearline Storage Driver for CTA. Version: 0.7.0 2022-11-04T08:32:39Z
    
    (pool_write@dCacheDomain) admin > hsm show providers
    PROVIDER   DESCRIPTION                                                                 DYNAMIC
    script     Calls out to an HSM integration script.                                      no
    link       Hard links flushed files in another directory.                               no
    copy       Copies files to and from another directory.                                  no
    tar        Bundles files into tar archives (not ready for production).                  no
    dcache-cta dCache Nearline Storage Driver for CTA. Version: 0.7.0 2022-11-04T08:32:39Z yes
```

The `mover ls` command has been updated to display the remote peer's IP address for Xroot and HTTP movers.

The `st/rh/rm kill` admin commands accept `*` to cancel all running requests.

The Kafka message includes the pool's local socket address used by the transfer.

Before release 9.1, Xroot and HTTP movers were binding their TCP port upon the first request, shutting down the
socket when no movers were left running. Henceforward, dCache will bind these ports on startup and keep them
for the entire JVM lifetime.

### SystemCell

The system cell provides two new commands, `jfr start` and `jfr stop`, to start and stop the Java Flight
Recorder directly from the admin interface. More Information about Flight Recorder and dCache can be found at
[debug section](https://github.com/dCache/dcache/blob/master/docs/TheBook/src/main/markdown/cookbook-debugging.md) in the dCache Book.
The JFR can be started with one of the pre-defined JVM-supplied configurations or with a custom configuration file.

### WebDav

Subsequent to an upgrade of the bootstrap library, certain visual features of the GUI were broken; these
have now been repaired.  Two other bugs were fixed (return empty list instead of null for
empty collections; preserving URL of the request in protocol information).

### Xroot

Two bug fixes have been made, one to restore the proper handling of single-slash paths, the
other to fix erroneous permission denied exceptions when `POSC` (`persist on successful close`) 
is used. 

Xroot now also supports the use of user-root relative paths like other doors (e.g., WebDav and FTP).

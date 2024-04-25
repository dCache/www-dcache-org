---
title: "Server 9.2"
draft: false
weight: 3
---
## Highlights

- Performance improvements for the concurrent directory creation and removal

## Acknowledgements

Many Thanks to Lars Jansse and Sandro Grizzo for their contributions.

## Incompatibilities

- prior to version 9.2 dCache NFSv4.1 door will publish only `nfs4_1_files` layout. Now on
  the door publishes all available layout types
- dropped reference count tracking for directory tags
- dropped support of java options `chimera_soft_update` and `chimera_lazy_wcc` in favor of `chimera.attr-consistency` property
- If upgrading from 8.2, be sure to read also the release notes for 9.0 and 9.1, important changes are described there. See the [9.2 migration guide](https://github.com/dCache/upgrade-guide/blob/master/UPGRADE92.md) for more complete information
- With version 9.2.8+, empty and non-existent banfiles will be treated the same


## Known issues

Cancel is currently broken in bulk service and recursive requests could freeze up.

### Admin

Paging capability was added to the AnsiTerminalCommand (and DirectCommand),
where results exceeding 10K lines prompt the user with [Y/N] for
further results.   When executing a shell command in non-terminal
mode, all results are streamed back continuously.  The Bulk commands have
been converted to use this feature.

### Bulk

There have been many small fixes and improvements to this service since 8.2.

The most significant changes or additions include:

-   The storage layer has been redesigned to conserve space and for efficiency/throughput.
    Please note that moving up to the new database schema may require some time.  
    The amount of time can be generally computed in terms of the number of entries in
    the `request_target` table; figure about 1 hour for every 10 million.  If it is
    necessary to maintain all such entries, we recommend you do the upgrade offline,
    using the `dcache database update` command-line tool.  If there is no need to keep 
    completed requests in the database, we would advise truncation or at least deletion 
    of the completed requests before the upgrade.

-   Periodic archiving of requests has been added; this is configurable via properties
    and admin commands.  The archive table maintains an abbreviated summary of the 
    requests, and can be purged via admin command as well.

-   The container job has been rewritten to bring it in line with SRM bringonline performance;
    multithreaded directory listing has also been implemented to improve recursive
    request time-to-completion.

-   Path resolution (for relative paths) has been integrated into bulk request processing.

-   Activity providers now can capture the environment so that defaults can be 
    customized (this presently pertains only to pin and stage lifetime attributes).

-   Support has been added for the QoS update request to handle policy arguments.

-   Bulk has been made replicable (HA).  Please read the requirements
    in the cookbook section on High Availability.

-   The `delay clear` option has been eliminated from bulk requests.

-   The `prestore` option has also been eliminated, as it is no longer necessary
    (since all initial targets are immediately batch stored synchronously)
    before the submission request returns.

A few minor points:  

-   Default limits for request size and number of requests per user have been set on
    the basis of WLCG requirements.

-   The counts displayed by the `info` command are from startup and are cumulative; for
    actual (current) counts based on the data store, use the `status counts` command.

There are numerous properties which have been deprecated, particularly those dealing
with limits.  Please review the bulk.properties file for details.   It should not
be necessary under normal usage to adjust the limits for thread pools, database
connections and semaphores from the defaults.  Depending on the volume of activity
and the site requirements, the archiver period and window may need to be shortened
from the default.  Note that the options for `clearOnSuccess` and `clearOnFailure`
that can be included in an individual bulk request to indicate immediate deletion
are not available for STAGE requests as this feature is not part of the WLCG specification,
so the only way to remove such requests automatically is through the archiver.

### Chimera

With dcache version 9.0 chimera has introduced `chimera_soft_update` and `chimera_lazy_wcc` Java properties to control the behavior
of the parent directory attribute update policy. Now those properties are obsolete and replaced by a regular dcache configuration
property `chimera.attr-consistency`, which takes the following values:

| policy | behaviour                                                                                                                                                                                                                                   |
|:-------|----------------------------------------------------------------------------------------------|
| strong | a creation of a filesystem object will right away update parent directory's mtime, ctime, nlink and generation attributes                                                                                                                   |
| weak   | a creation of a filesystem object will eventually update (after 30 seconds) parent directory's mtime, ctime, nlink and generation attributes. Multiple concurrent modifications to a directory are aggregated into a single attribute update. |
| soft   | same as weak, however, reading of directory attributes will take into account pending attribute updates.                                                                                                                                    |

Read-write exported NFS doors SHOULD run with `strong consistency` or `soft consistency` to maintain POSIX compliance. Read-only NFS doors might
run with `weak consistency` if non-up-to-date directory attributes can be tolerated, for example, when accessing existing data, or  `soft consistency`,
if up-to-date information is desired, typically when seeking newly arrived files through other doors.

### Frontend

Aside from bug fixes, the following changes should be noted:

-   Support for .well-known/security.txt was added to both the frontend and WebDav ports.
-   More detailed description of request objects for bulk and stage.
-   Improved error messages in several places.
-   Support for relative paths and symlink prefix resolution for bulk and namespace resources.
-   The frontend.wellknown paths have been deprecated in favor of dcache.wellknown. 
-   Authz checks have been removed in Quota GET methods.
-   Use of RolePrincipal (see under gPlazma) replaces reliance on LoginAttributes and the
    old admin role (special gid) as defined by the roles plugin.
-   Support for QoS Rule Engine policies.  A new qos-policy resource allows one to
    add, remove, list and retrieve policy definitions.  See the Swagger pages for details.
-   An -optional query parameter was added to the namespace resource to retrieve extra information 
    about a file; the new QoS Policy file attributes (QOS_POLICY and QOS_STATE) are included
    with this option.
-   Support for pool migration has been introduced (migrations resource).
    See Swagger pages for details.

### gplazma

A RolePrincipal has been added to support the use of role definitions in the multimap file.
Currently the available roles are:  admin, qos-user and qos-group.  The second allows
the user to transition files owned by the user's uid; the third allows the user
to transition files whose group is the user's primary gid.  The two qos roles
can be combined.  Admin grants full admin privileges.  A multimap example:

```
dn:"/DC=org/DC=cilogon/C=US/O=Fermi National Accelerator Laboratory/OU=People/CN=Al Rossi/CN=UID:arossi" username:arossi  uid:8773  gid:1530,true roles:admin
```

These roles do not depend on the presence of the roles plugin.  Since the frontend
has been changed to use the new RolePrincipal and not the old role definitions
(where a special gid must be defined), one can easily drop that plugin from
the gplamza.conf file.  

There was also an important fix for a bug that was preventing
upload (e.g., xroot POSC) when using tokens.

The gplazma-xacml plugin has been dropped.

### NFS

Prior to version 9.2 dCache, to support RHEL6 based clients, if no species export options are specified, the NFSv4.1 door
were publishing only `nfs4_1_files` layout. Now on the door publishes all available layout types. If for whatever reason
RHEL6 clients are still used, the old behavior can be enforced by `lt=nfsv4_1_files` export option.

### PNFS Manager

To remove unused directory tags chimera keeps the reference count (nlink) of tags. This approach creates a 'hot' record that
serializes all updates to a given top-level tag. Starting 9.2 dCache doesn't rely on ref count anymore and uses conditional DELETE,
which should improve the concurrent directory creation/deletion rate.

### Pool

The `mover ls` command is updated to display with the `-u` option the subject associated with the mover.

Added `pool.mover.https.port.min` and `pool.mover.https.port.max` to control TPC port number used by HTTPS mover.

Pools `info` command displays HTTP, HTTPS, NFS and XROOT movers listen ports and interface.

Billing information from DCAP and FTP movers now on include local socket end point

### Poolmanager

Pnfsid and path options have been added to ac psu match.

Re-introduce `wrandom` partition type - a weighted-random pool selection partition, which works as a `WASS` partition, but ignores LRU metric and number of movers.

### QoS

Aside from bug fixes, the following significant changes to QoS should be noted:

-   QoS was made to support migration using a new pool mode, "DRAINING"; please consult the
    book chapter for further details.

-   The DB namespace endpoint can now be configured to be separate from the main Chimera
    database (originally introduced/changed for Resilience). In this way, the scanner,
    whose namespace queries are read-only, could be pointed at a database replica.

    This remains possible for QoS, even though the QoSEngine now also is responsible
    for updating Chimera with file policy state.  These writes are all done via
    messaging (PnfsHandler) rather than by direct DB connection, so they go to
    the master Chimera instance.

-   Requests for QoS transitions are now authorized on the basis of role (see under gPlazma).

-   The first version of the QoS Rule Engine has been added.   With this, one can
    define a QoS policy to apply to files either through a directory tag or via
    a requested transition; the engine tracks the necessary changes in state over time.
    A new database table has been added to the qos database.  Remember that if you are
    deploying QoS for the first time, you need to create the database:

    createdb -U \<user\> qos
    
    Fuller explanation of the rule/policy engine is forthcoming in the Book chapter.

-   In conformity with the new rule engine changes, the scanner has been modified in terms of
    how it runs scans.   There are now two types of system scans, the (NEARLINE) QoS scans
    (to ensure that files with a policy requiring them to be flushed have indeed been written
    to tape) and a system-wide ONLINE scan (there are two versions of this and an option to 
    choose which one).  Details in the Book; also refer to the relevant admin commands.

Note that the singleton QoS service (where all four components are plugged into each other 
directly) is no longer available; the four services can, however, still be run together or in
separate domains, as with any dCache cell.

### Resilience

Resilience is still available in 9.2, but should be considered as superseded by the
QoS services.   We encourage you to switch to the latter as soon as is feasible.
Remember *_not_* to run Resilience and QoS simultaneously.

A recipe has been added to the cookbook for migration/draining of resilient pools.
This continues to apply in QoS.

### WebDAV

The WebDAV door now provides limited support for metalink format.
Metalink is an XML format, described by RFC 5854, that describes how
to download multiple files.  The initial support is limited to
describing the files within the targeted directory: there is no
support for recursion.  The metalink information is available via HTTP
Content Negotiation (client sends a `Accept:
application/metalink4+xml` header) or via a `Link` HTTP response
header (see RFC 6249) when generating the normal HTML output.

### XRootD

Aside from bug fixes and various documentation additions and clarifications,
the following should be noted:

- Proxying through the xroot door is now available.
- Relative paths are supported in the xroot URL.
- Resolution of symlinks in path prefixes and paths is supported.
- The efficiency of the stat list (ls -l) has been greatly improved.

An Xroot User Guide page has been started.
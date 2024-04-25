---
title: "Server 9.0"
draft: false
weight: 3
---
## Highlights

- dCache now supports Java 17 as its platform

## Incompatibilities
- the `cleaner` service, originally a single cell, now consists of two parts: one cell for disk cleaning (`cleaner-disk`), one for hsm
cleaning (`cleaner-hsm`)
- DCAP and NFS doors will fail the request if file's storage unit is not configured in PoolManager
- linklocal and localhost interfaces are not published by doors and pools
- DCAP movers always start in `passive` mode
- removed experimental message encoding format
- removed default HSM operation timeout
- With version 9.0.24+, empty and non-existent banfiles will be treated the same



### Admin

Paging capability has been added to certain admin commands.  This means
that large lists of results that exceed a certain limit are returned
in chunks <= to the limit, and the user is prompted as to whether
more should be displayed [Y/N].  These commands (see, for instance,
bulk's `target ls`), when invoked via a non-terminal script, will
continue automatically to stream chunks back until the end of the list.


### bulk
* [41a9e807d7] dcache-bulk: fix missing pnfsid on cancel of PIN/STAGE

The move to a revised (version 2) bulk service was made with 8.2, but
since then there have been a number of optimizations and improvements 
some, but not all, of which were backported to 8.2.

NOTE:  Most importantly, there was a significant refactoring of the database
tables to provide for more efficient use of space (via normalization).
To preserve existing data, several somewhat costly queries are thus
run on restart after upgrade from 8.2 to 9.0.  As a benchmark for 
the amount of downtime necessary for the upgrade, figure approximately
one hour per every 20M rows in the `request_target` table.  It is always
possible, in order to avoid overly long downtime delays, is to eliminate
all requests which have completed; in the postgres interpreter:

```
DELETE FROM bulk_requests WHERE status='COMPLETED';
```

An important bug fix which was included in 8.2 was to change the behavior
of the initial submit to store explicit targets (not those found recursively,
but specified in the target list of the request) immediately/synchronously.
This was needed to support WLCG requirements.

We have dropped the 'delay' option for request clearing for generic bulk
requests. The admin command for STAGE activities now also automatically
sets `prestore` to true, as with the request submitted through the REST API.

The admin commands for `request ls` and `target ls` now use the new paging
capability.

LINK types are now handled correctly when the paths are refetched
from prestored targets.

The bulk request target list JSON was fixed to be backward compatible with 7.2
(it now accepts both a string and a JSON array).

The target-by-target error reporting in the bulk request GET response JSON object
is now less confusing.

### Cells

The expereimantal message encoding format is removed. As a result, the property `dcache.broker.channel.msg-payload-serializer`
is **immutable** and set to value **standard**.

### Cleaner

The `cleaner` service, originally a single cell, now consists of two parts: one cell for disk cleaning (`cleaner-disk`), one for hsm
cleaning (`cleaner-hsm`). They can be deployed as desired, be assigned different resources and each run in HA mode.
This will hopefully improve performance issues and help admins configure and understand cleaner behaviour.

Be aware that the property names have changed their prefixes from `cleaner.<something>` to `cleaner-disk.<something>` and
`cleaner-hsm.<something>`, while some admin commands have lost the "hsm" String from their name.
Note: As not all previously existing parameters were used to control the behaviour of both the disk and hsm parts of the
old, combined `cleaner` cell, please check which parameters are carried over to `cleaner-hsm` and `cleaner-disk`, respectively.

Example setup:

```
[dCacheDomain]
[dCacheDomain/cleaner-disk]
cleaner-disk.cell.name=cleaner-disk1

[dCacheDomain/cleaner-disk]
cleaner-disk.cell.name=cleaner-disk2

[dCacheDomain/cleaner-hsm]
```

Also, an admin command was added to the hsm-cleaner cell that allows forgetting a tape-resident pnfsid, meaning removing any corresponding delete target entries from the cleaner’s trash table database.



### Core

To reduce network-configuration-based issues, starting with version 9.0, dCache does not publish IPv6 link-local
or locahost IP addresses in the login broker. However, the connections for those interfaces are acepted.

### DCAP

The DCAP door will fail the request if a file's storage unit is not configured at the PoolManager, as all other doors already do.

Since dCache 1.7.0 the DCAP door was supporting two modes of establishing data connection  between a client and a pool. Either
a pool is connected to the client, or the client initiates the connection to a pool. The latter is a firewall-friendly behavior,
but should be requested by the client. Starting with dCache 9.0, the DCAP movers will always start in a `passive` mode, independent
of client preference.

### Frontend

There have been several bug fixes to the frontend bulk API, particularly to make
it backward compatible with 7.2 and to fix regressions in the new /api/v1/stage
resource.   A bug blocking STAGE submissions where fileLifetime was not specified
has been fixed. 

A bug with `mv` (rename) and relative paths in the /api/v1/namespace resource was
corrected.

Support was added for .well-known/security.txt, exposed both on the frontend 
and webdav ports.

Support for maintaining the pre-8.2 implementation for /api/v1/namespace qos transitions
(via a dcache property) was added; also, if this is not set to `false` and the QosEngine service
is not running, the namespace qos transition will now fail fast.

The default type parameter to ``/api/v1/pools/{pool}/nearline/queues (=all types) 
now works.  

It is now possible to filter and sort the results of the /api/v1/restores 
by path, owner and group.  Since this, as with other RESTful commands, is
implemented as a string 'contains', it is possible to match all restore
activity for a directory.

The User Guide has been updated to reflect the correct WLCG URL prefix and to
add more information concerning support for .well-known.


### gplazma

Upload (as in xrootd's -P / persist on successful close) permissions with JWT token claims now work correctly.

### History

A bug handling NaN (not a number) errors in histogram conversion has been fixed,
as well as one for index-out-of-bounds errors in timeseries histograms.


### NFS

The NFS door will fail the request if a file's storage unit is not configured in PoolManager, as all other doors already do.
The new `show open files` command lists all open files whether or not a mover exists for them (the latter is displayed by `show transfers`).



### Pool

Typically, when dCache interacts with an HSM, there is a timeout on how long such requests can stay in the HSM queue. Despite the
fact that those timeouts are HSM-specific, dCache comes with its own default values, which are usually incorrect, so admins usually
end up explicitly setting them.

Starting with version 9.0, the default timeout has been removed. This means that there is no timeout for HSM operations unless explicitly set by admins.
> NOTE: this change is unlikely to break existing setups, as previous timeout values are already stored in the pools setup file.

In addtion, a new command `sh|rh|rm unset timeout` has been added to drop defined timeouts.


### QoS Engine

Some regressions which occurred during the adaptation of resilience code have been fixed;
these involved the monitoring of pool status changes and support for regex resolution in
the poolmanager configuration.

Support for migration using a new pool mode, "DRAINING", was added.  As with Resilience,
a separately configurable namespace endpoint is now possible.

### Resilience

Now allows for a separately configurable namespace endpoint (for instance, if resilience were to
use an RDBMs replica instance rather than the live instance of chimera).   

A recipe for the migration/draining of resilient pools has been added to the dCache cookbook.


### XRootD

There have been a number of bug fixes since 8.2.0.  These have all been backported.  They involve
handling of the door address and proper shutdown on error for the door proxy, returning the correct
error code when a file exists, making the handling of anonymous restrictions by the door consistent
(for multiple authentication protocol doors), correct handling of error logging, 
and not publishing link local addresses.

Defaults have been changed for number of mover threads (now uses Netty defaults) and the order of the
door's gplazma plugins (ztn before gsi).

Improvements to documentation concerning TLS and host cert/key, recommendations for direct memory
usage with door proxy; we have also added an Xroot User Guide page (only minimally populated at the
moment).
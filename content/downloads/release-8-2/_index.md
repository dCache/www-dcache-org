---
title: "Server 8.2"
draft: false
weight: 3
---
## Highlights

- support for nested groups in the PoolManager

## Incompatibilities

- pool group names can't start with `@` symbol
- hard links on directories is not allowed
- GET api/v1/bulk-requests has a new return value (see below)
- The QoS API functionality now requires some services to operate. These are described in the dCache Book version 8.2.
- With version 8.2.40+, empty and non-existent banfiles will be treated the same




### Bulk

The bulk service has undergone a major revision.  There have
been numerous internal changes and enhancements, but the most
important have to do with persistence and scalability.

The bulk service now requires a database which holds
request information (instead of raw files, as in version 1).
Bulk requests are serviced concurrently, but each request
runs in its own container in order to optimize threading
and I/O.  

The UPDATE_QOS activity communicates with (and thus requires
the presence of) the new QoS Engine, but the other bulk
activities can be run without QoS being present.  

Aside from being accessed through frontend REST APIs,
the bulk service has a full set of admin shell commands
which are useful for spot-checking its operational state
and the status of requests (along with their individual targets).

A fuller explanation is available in The Book
chapter on the bulk service.


### Cleaner

An admin command was added to the `cleaner` cell that allows forgetting a pool, meaning removing any corresponding delete target entries from the cleaner's trash table database. If the requested pool is not currently blacklisted, the admin is required to explicitly enforce the operation.



### Frontend

For the bulk-requests API:  the typing of primitive parameters in the JSON
bulk request has been loosened so that booleans and numerics can also be 
specified as strings. 

The RESTful options for QoS (for instance, in api/v1/namespace) as of 
version 8.1 communicate with the new QoS Engine.

The JSON returned from GET api/v1/bulk-requests is no longer a
simple array of URLs, but an array of objects with fuller info 
concerning each request (hence this return value now requires 
parsing; see the SWAGGER page for fuller information).  All matching
requests are now returned (rather than those owned only by the user), 
though it is possible to filter the result by a comma-delimited list 
of user/owner ids.

A new TAPE API has been made available for the purposes of
bulk staging (api/v1/tape); it implements the WLCG TAPE specification,
and thus includes api/v1/tape/archiveinfo [=locality for dCache], 
api/v1/tape/stage and api/v1/tape/release [=unpin for dCache].  
See the SWAGGER pages for fuller info.  There is also a section 
in the User Guide (Frontend/BulkRequests) which details the APIs 
for bulk-requests and tape along with their differences.



### PNFS Manager

Historically, the dCache namespace was allowing hard links on directories. This violates
POSIX restrictions and is a source of file system cyclic loops. With dCache release 8.2
creation of hard links on directories is not allowed any more. The existing hard links will
work as before; however, it is recommended to replace them with soft links.

To allow pool, nearline storage and other components to make scheduling decisions based on
file creation time, PnfsManager will include the CREATION_TIME attribute whenever STORAGEINFO 
is requested.

### Pool

Added `rm kill` command that canceles a pending file remove request from HSM.

When a pool decides which storage class to flush it uses the last submission time. However, submission
time is re-set when a pool fails to flush the file and re-submit the file. Starting with 8.2, 
a pool will sort the flushing based on the time when files are created on the pool.

### Poolmanager

PoolManager has been updated to support pool groups that contain other pool groups. For example:

```
psu create pgroup -dynamic -tags=hw-class=IO io-pools
psu create pgroup -dynamic -tags=hw-class=replica replica-pools
psu create pgroup all-pools
psu addto pgroup all-pools @io-pools @replica-pools
```

The values prefixed by `@` in the `psu addto pgroup` command are treated as groups.


### Resilience / QoS Engine

The fully refactored QoS Engine first became available in 8.1.  Among
other things, its components are intended to replace Resilience.   While
Resilience is still available in 8.2, it will eventually be deprecated, so it
is advisable to begin switching to QoS with this Golden Release.  

There is a chapter on the QoS service components in The Book, which includes
instructions on how to set it up to emulate Resilience (the differences are minor).

Note that a major change between Resilience and QoS is that the QoS Verifier
uses a database whereas none was used by Resilience.




### XRootD

Several improvements have been made so that the door endpoint address is consistently reported.

An important oversight in the door code has been corrected.  A client disconnect now also
interrupts any thread waiting on a response from PoolManager; otherwise, all available threads
could be tied up, potentially blocking further client connections.

The xroot door is now capable of running in "proxy" mode; when this switch is turned on,
the client connects through the door instead of directly to the pool in order to read or write data 
(allowing for outside communication with pools protected on an internal network, for instance).  An
internal address (subnet) can also be given which will be used for the Pool Manager pool selection.
Details are available in the xroot chaper of The Book.

### Zookeeper

The Zookeeper version has been updated to the latest stable release 5.7.1
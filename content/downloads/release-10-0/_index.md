---
title: "Server 10.0"
draft: false
weight: 3
---

## Highlights

- Added pool metadata directory configuration option
- Use environment variables as configuration properties

## Incompatibilities

- dropped native CEPH support. Sites must migrate their pools before updating dCache
- dropped idle timeout handler in netty-based movers (xroot, http)

## Acknowledgments

Many Thanks to Petter A. Urkedal and Lukas Mansour for their contributions.


### Billing

The property `billing.format.json` was added to write the logs in JSON format instead of plain text if it's set to true.

### Configuration

In the container world, it's common to use environment variables to pass the
configuration parameters. With release 10.0 dCache support injects of environment
variables into a configuration with special values prefixed with `env.`. For example,
to control wan port numbers with environment variables DCACHE_WAN_PORT_MIN and 
DCACHE_WAN_PORT_MAX the layout file might have the following configuration:

```
dcache.net.wan.port.min=${env.DCACHE_WAN_PORT_MIN}
dcache.net.wan.port.max=${env.DCACHE_WAN_PORT_MAX}
```

&gt; NOTE: the nested variables are not allowed. E.g. `${env.$VAR1-$VAR2}` will not be resolved.


### DCAP

Added support for additional Kafka parameters.

### Frontend

### FTP

Added support for additional Kafka parameters.

### gplazma

Update OIDC-related documentation.

### Pinmanager

A pin request via admin interface no longer waits for the pin to be established before returning.

### Pool

Sites that have tried to run dCache with built-in CEPH decided to go for CephFS (mounted as regular FS)  as it provides better performance,
scalability and erasure encoding. Thus, built-in CEPH is not recommended for deployments and is removed in version 10.0. Sites that have
such a setup should migrate pools before updating the dCache.

Admin command `csm info` will display enabled digests for checksum calculation.

Added property `pool.path.meta` to specify the location of pools metadata directory, defaults to `pool.path`.

Added pool command `csm use directio on|off` to enable/disable O_DIRECT to bypass the filesystem cache during checksum calculation.

Prior version 10.0 for xroot and http movers dcache hat two ways to detect and cancel idle connections: Job Timeout Manager (jtm)and
idleHandler. Now, the latter one is removed and only jtm-baes one is used. The following pool properties are obsolete:

```
pool.mover.xrootd.timeout.idle
pool.mover.xrootd.timeout.idle.unit
pool.mover.http.timeout.idle
pool.mover.http.timeout.idle.unit
```

Starting version 3.0 has dropped the limit on the maximal number of concurrent p2p transfers for a single pool. This is typically not a problem,
however, in some situations, namely multiple transfers from many pools to a single one, during rebalancing, for example, a pool might become
unresponsive due to IO starvation. With version 10.0 the maximal number of concurrent p2p transfers was re-introduced and `pp set max active`
command is undeprecated. The default value is `unlimited`.

### Dependencies

Updated external dependencies in preparation for Java17 migration.

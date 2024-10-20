title: "Release 10.2.X"
draft: false
---
dCache 10.2 is a Golden  Release introducing following highlights:

- Expose user/group quotas via remote quota protocol
- Java 21 is supported as runtime environment
- PoC support for firefly network markers
- Support of Label-based virtual read-only directories

#### Incompatibilities

- Pool Manager partitions by default allowed  to stage
- Dropped gPlazma ARGUS plugin
- A stage request canceled in the poolmanager will propagate the cancel to pool
- Pool internal health check is disabled if an external check command is configured.
- Access to files with QoS 'HSM'-only will be denied, even if they are still available on disks.


{{< release-table >}}

{{< releases-10-2 >}}

{{</ release-table >}}


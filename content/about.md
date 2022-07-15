---
title: About us
draft: false
menu: main
weight: 7
---

The goal of this project is to provide a system for storing and retrieving huge amounts of data, distributed among a large number of heterogenous server nodes, under a single virtual filesystem tree with a variety of standard access methods. Depending on the persistency model, dCache provides methods for exchanging data with backend (tertiary) storage systems as well as space management, pool attraction, dataset replication, hot spot determination and recovery from disk or node failures. Connected to a tertiary storage system, the cache simulates unlimited direct access storage space. Data exchanges to and from the underlying HSM are performed automatically and invisibly to the user. Beside HEP specific protocols, data in dCache can be accessed via NFSv4.1 (pNFS) as well as through WebDav. [Read more >>](/docs/dcache-whitepaper-light.pdf)

### Project partners and funding

dCache is a joint venture between the Deutsches Elektronen-Synchrotron, [DESY](http://www.desy.de/), the Fermi National Accelerator Laboratory, [FNAL](http://www.fnal.gov/) and the Nordic e-Infrastructure collaboration, [NeIC](https://neic.no/nt1/).

### Contributing

As an open source project, dCache welcomes community contributions. It uses the linux kernel model where git is not only source repository, but also the way to track contributions and copyrights. A good place to start is our [GitHub](https://github.com/dcache/) or the [Developer's Corner](../development).

## Where we are

### Live map of dCache installations around the world

![dcache map][dcache-map]

### Project status

Since the end of 2001 our full production release is in use at an increasing number of sites world-wide and is delivering terabytes of data from over hundreds of distributed server nodes.

### Contact

- `support(at)dcache.org`
- `security(at)dcache.org`

[dcache-map]:  ../img/dcache-map.gif

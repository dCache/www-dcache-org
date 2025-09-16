---
title: "dCache turned 25!"
date: 2025-09-16T10:15:18+02:00
tags: ["web", "dcache.org"]
categories: ["info"]
thumbnail: "img/dcache-project-proposal-2000.png"
draft: false
---


On the 16th of September 2000, at the *Computer User Committee* at DESY, a new project, **A Homogeneous Distributed Storage Environment for Physics Data Processing**, was presented. Its main objectives were to address storage system concerns after HERA and Tevatron accelerator upgrades at DESY and Fermilab. 


<!--more-->

![dcache project proposal 2020][dcache-porposal]


The new system had the following goals:

- Optimized usage of existing tape drives through transfer rate adaptation.
- Support for slower, cheaper drive technologies without reducing overall performance.
- Improved utilization of robot systems by coordinating read and write requests.
- Better use of network bandwidth by locating data at the most suitable site.
- Direct data access without requiring explicit staging.
- Uniform access methods for data, independent of its storage medium.
- A unified system view, even without the backend HSM, presenting a large data store with a unique namespace and standardized access methods. Valuable data would remain safely on disk until an HSM copy was created. Transfers to the HSM could be policy-driven or triggered manually.
- A collaborative effort, ensuring more efficient use of manpower and guaranteeing long-term support and maintenance of the software.

This vision led to the official approval of the dCache project, cementing a lasting collaboration between Fermilab and DESY.

Since then, dCache is not only used by Fermilab and DESY, but has become a major storage system deployed at many sites around the world.

During the past 25 years, we have always tried to address new challenges as soon as they showed up on the horizon:  GRID computing, distributed deployments, new authentication methods and access protocols. And while dCache is open to new technologies, we have always advocated for standard solutions. Today, dCache can expose data through standard WebDAV or NFSv4.1 protocols, protect in transit data with TLS, manage the user base in an LDAP and authenticate via OAuth2 or OpenID-Connect.

But none of this journey would have been possible without our users and collaborators. Their trust, feedback, and ambitious use cases have continuously pushed us to innovate, adapt, and grow.

The journey so far has been remarkable — but even greater challenges and opportunities are still to come.

🎉 **Happy Birthday, dCache!** 🚀

[dcache-porposal]: img/dcache-project-proposal-2000.png

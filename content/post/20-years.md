---
title: "dCache is 20!"
date: 2020-08-31T20:37:12+02:00
draft: false
tags: ["web", "dcache.org"]
categories: ["info"]
---

On the 16th of September 2000, at the *Computer User Committee* at DESY, a new project, **A Homogeneous Distributed Storage Environment for Physics Data Processing**, was presented. Its main objectives were to address storage system concerns after HERA and Tevatron accelerator upgrades at DESY and Fermilab. The new system had the following goals:


  *   Optimized usage of existing tape drives due to transfer rate adaption.
  *   Possible usage of slower and cheaper drive technology without overall performance reduction.
  *   Optimized usage of the robot systems by coordinated read and write requests.
  *   Better usage of network bandwidth by exploring the best location for the data.
  *   No explicit staging required to access the data.
  *   Access methods for data that would be unique, independent of its media location.
  *   A view of the system, even without the backend HSM, as a huge data store with a unique namespace and standardized access methods. Care would be taken that valuable data reside on safe disks as long as no HSM copy exists. Backend storage to the HSM could be done regularly (policy based) or by manual intervention only.
  *   A joint effort, making the use of manpower more efficient and guaranteeing continued support and maintenance of the developed software.

Thus the dCache project was officially approved, initiating a long standing collaboration between Fermilab and DESY on the development of a common storage solution.

Since then, dCache is not only used by Fermilab and DESY, but has become a major storage system deployed at many sites around the world.

During the past two decades, we have always tried to address new challenges as soon as they showed up on the horizon:  GRID computing, distributed deployments, new authentication methods and access protocols. And while dCache is open to new technologies, we have always advocated for standard solutions. Today, dCache can expose data through standard WebDAV or NFSv4.1 protocols, protect in transit data with TLS, manage the user base in an LDAP and authenticate via OAuth2 or OpenID-Connect.

The road behind us is a long one, but we also have new challenges in front of us.

Happy Birthday, dCache!

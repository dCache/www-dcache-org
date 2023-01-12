---
title: "Releases"
draft: false
menu: main
weight: 4
---

## Release policy

dCache follows a time-boxed two dimensional release policy. Every four months we produce a *feature release*. Feature releases have version numbers like **8.x.0**, where **x** is a positive integer. A new feature release marks the start of a *branch*; for example, the release of dCache **8.1.0** marked the start of the
**8.1**-branch.

A feature release is followed up by a number of *maintenance releases* containing bug fixes or high priority feature tweaks. Maintenance releases for release **8.x.0** have
version numbers **8.x.y**, with **y** being some number larger than 0; for example, **8.1.0** is a feature release, **8.1.1** is the first maintenance release for the **8.1**-branch and **8.1.2** is the second maintenance release.

## Support period

Typically once a year, we designate one branch to a long-term supported release: a *golden release*.  The only difference between long-term releases and other branches is the support
period.  A golden release is supported until the second subsequent long-term release, which is typically two years. Other branches are supported until the end of the previous
golden release support period.

Choosing between golden releases and other branches is a matter of site policy and a matter of taste.  Newer branches contain newer features, but they also have shorter support periods and often fewer dCache instances using them.  However, upgrading from a golden release to a non-golden branch does not affect the end-of-support date.

Some sites use dCache that is repackaged by another distribution. Such a distribution may recommend a specific version. Sites should follow their distribution's recommended version.

---
title: "Developer's Corner"
draft: false
menu: main
weight: 8
---

dCache is an open source project that may be found on [GitHub](https://github.com/dCache), which not only hosts the repository but is used to track contributions, copyright and issues. This page intends to summarize the basic information of interest for development, including helpful links to repositories and tools, but will also be used to document our workflows and link to the meeting notes. External contributions to the dCache project are welcome!

## Getting started

The formalities on [how to contribute a patch](https://github.com/dcache/dcache/#how-to-contribute) are described in our GitHub README. We use use an adapted version of the Google style guide for Java to format code before committing it. The formatting document for IntelliJ can be found in the repository root.

Another document describes [how to build dCache](https://github.com/dCache/dcache/blob/master/BUILDING.md), which not only includes how to build various packages, but also how to create the *system-test* deployment, which provides a quick and easy way to create a small, ready-to-start test instance. To build dCache, the following tools are required:
- Java 11 (for building and running)
- Maven 3.5.0 or newer

## Workflows and best practices

Following the dCache [release policy](../release), several production branches are maintained at any point in time, including the lastest feature releases up to the newest golden release. Changes are first added to the master branch, bug fixes are then backported to supported branches and published as maintenance releases once a week.

In order to add a patch to the dCache codebase, external contributors must submit a pull request. Core team members use the Review Board software to inspect and discuss changes before allowing submission into the master branch.

Testing is an important aspect of software quality. We use JUnit for functional testing, SpotBugs for static code analysis and the Robot framework for black-box integration testing. When committing a patch to the GitHub repository, a continuous integration process is triggered via Jenkins that automatically builds dCache and runs this test suite to catch regression errors, giving feedback on the commit.

## What to work on

There are three main areas that are addressed on on a regular basis. The first one is *bug fixing* -- users open tickets or we discover issues ourselves that need to be corrected and backported. These issues are usually reported either via opening a GitHub issue, creating a ticket via our `support(at)dcache.org` mailing list or writing directly to our `dev(at)dcache.org` mailing list.

The second area is continual *software maintenance*, which includes keeping the used libraries and general code base up to date, but also experimenting with more modern approaches and frameworks for existing functionalities.

Lastly, dCache is enriched with *new capabilities*. These may be based on feature requests, for example via GitHub or discussions with users, but are also often motivated by the general roadmap of the dCache project. These goals that we set ourselves include extending functionality that certain components already have to other ones, anticipating upcoming developments of relevance for dCache and in general striving to make the lives of admins easier.

## dCache core team meeting notes

The core development team meets weekly to discuss current issues and tickets, ongoing developments and future plans. The meeting notes are tracked in a [GitHub repository](https://github.com/dCache/developer-meeting-notes) that is accessible to core team members.

---
title: "Migrating to a new website engine"
date: 2020-05-31T18:01:39+02:00
draft: false
tags: ["web", "dcache.org"]
categories: ["info"]
---

After almost two decades, the dCache.org web content has been migrated from regular
HTML pages written by hand to a modern static site generator based on [hugo](https://gohugo.io) with the [Mainroad](https://github.com/Vimux/Mainroad) template.

This change will simplify the maintenance of the web pages and will allow developers to keep
the content up-to-date. New pages and documentation can be added by simply dropping a file written in markdown format into the desired directory. The source code of the pages is available on [GitHub](https://github.com/dCache/www-dcache-org.git) and can be directly edited in a browser.

The new pages are following the `responsive design` concept, which makes them mobile device friendly.

![Mobile view][mobile-screenshot]

[mobile-screenshot]: ../../img/new-site-mobile.jpg

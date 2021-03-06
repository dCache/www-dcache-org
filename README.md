# dCache org web page

The static pages based on [Hugo](https://gohugo.io)

## Bootstrap the site

Clone repo into a local directory:

```
$ git clone --recurse-submodules https://github.com/dCache/www-dcache-org.git
$ cd www-dcache-org
```

## Working with Hugo

First of all you need the hugo binary from the project [release page](https://github.com/gohugoio/hugo/releases).

Start the local server:

```
$ hugo server -D
```

The `-D` flags tells hugo to render draft pages as well. To generate static pages in the web server root directory:

```
$ hugo -d </path/to/root/dir>
```

or

```
$ hugo --cleanDestinationDir -b https://dcache.org/ -d /var/www/htdocs
```

### Adding new page

To add a new page/post use `hugo new` command:

For post:

```
$ hugo new post/workshop.md
```

or for a page

```
$ hugo new apage.md
```

The new elements are created relative to `content` directory. Special [front matter](https://gohugo.io/content-management/front-matter/) can be used to control page/post behavior. For posts:

- tags
- categories

Both are arrays of strings that can  be used later to identify posts. Example:

```yaml
---
title: "Workshop"
date: 2020-05-31T18:11:34+02:00
draft: false
tags: ["workshop"]
categories: ["workshop"]
---
```

> Note: posts are created with front matter `draft` test to `true`. To make posts visible is have to be update to `false` or start hugo with flag `--buildDrafts` to build site with draft posts.

The regular pages have:

- menu
- weight

that tells hugo to include page into top menu and control the position. The `weight` is an increasing number and controls the position from left-to-right. Example:

```yaml
---
title: "Main"
menu: main
weight: 1
---
```

Remove `date` front matter from the regular pages to avoid extra header with timestamp.

## Adding release

To add a new release run:
```
$ hugo new release/release-6.1.4.md
```
or create the corresponding file in `content/release/` directory. The release must have the following front matters defined:

- version - the version number
- url_base
- packages

For example:

```yaml
---
title: "Release 6.1.4"
date: 2020-06-02T00:17:15+02:00
draft: true
package: dCache
version: "6.1.4"
url_base: "https://www.dcache.org/downloads/1.9/repo"
notes: "https://www.dcache.org/downloads/1.9/release-notes-6.1.shtml"
packages:
- type: "rpm"
  filename: "dcache-6.1.4-1.noarch.rpm"
  checksum: "772408613df65e8d353a51fe8aa913e9"
- type: "deb"
  filename: "dcache_6.1.4-1_all.deb"
  checksum: "0fb637f7dcd158bfd92dcd9b9c56a1e6"
- type: "tgz"
  filename: "dcache-6.1.4.tar.gz"
  checksum: "145f1d9b578adb508eefe8d64a55fadc"
---
```

The download url constructed as:

```sh
${url_base}/$SERIES/${filename}
```

Where SERIES is derived from the version number.

## Adding Documentation article

To add new documentation article, simply add a new file into documentation folder, like:

```
hugo new documentation/zone-setup.md
```

A permanent, top level info can go into `documentation/_index.md`

## Page template and layout

The dCache pages use [Mainroad](https://github.com/Vimux/Mainroad) template. The template ins enabled as submodule. To override template, copy desired file from the template directory into the same location in the project. For example, to customize the behavior of `logo.html`

```
$ cp themes/Mainroad/layouts/partials/logo.html layouts/partials/logo.html
```

### The promotion frame

The middle of the main page has a promotion section that is always on top of the news feed. The content of the promotion is taken from `_index.md` file.

### Custom css

The look and feel of the pages can be adjusted by `css` located in `static/css/custom.css` file.
# dCache org web page

The static pages based on [Hugo](https://gohugo.io)

## Working with Hugo

### Adding new page

To add a new page/post/news use `hugo new` command:

For news:

```
$ hugo new news/workshop.md
```

Fore post:

```
$ hugo new news/workshop.md
```

or for a page

```
$ hugo new apage.md
```

The new alaments are created relative to `content` directory. Special [front matter](https://gohugo.io/content-management/front-matter/) can be used to control page/post behavior. For news and posts:

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

> Note: posts are created with front matter `draft` test to `true`. To make posts visible is have to be update to `false` or start hugo with flag `--buildDrafts` to build site with darf posts.

The regular pages have:

- menu
- weight

that tells hugo tu include page into top menu and control the position. The `weight` is an increasing numner and controls the position from left-to-right. Example:

```yaml
---
title: "Main"
menu: main
weight: 1
---
```

Remove `date` front matter from the regular pages to avoid extra header with timestamp.

## Page template and layout

The dCache pages use [Mainroad](https://github.com/Vimux/Mainroad) template. The template ins enabled as submodule. To override template, copy desired file from the template directory into the same locatio in the project. For example, to customize the behavoir of `logo.html`

```
$ cp themes/Mainroad/layouts/partials/logo.html layouts/partials/logo.html
```

### The promotion frame

The middle of the main page has a promotion section that is always on top of the news feed. The content of the promotion is taken from `_index.md` file.


### Custom css

The look and feel pf the page can be modified by `css` located in `static/css/custom.css` file.
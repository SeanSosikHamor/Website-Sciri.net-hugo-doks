---
title: "New Jamstack Hugo Site Bringup"
description: "Simple step by step instructions to bring up a basic Jamstack Hugo site."
lead: "Simple step by step instructions to bring up a basic Jamstack Hugo site."
date: 2022-05-09T13:54:30-04:00
draft: false
images: []
menu:
  docs:
    parent: "jamstack"
weight: 100
toc: true
---

## Install Hugo

Install Hugo by following the [quickstart](https://gohugo.io/getting-started/quick-start/) or [verbose](https://gohugo.io/getting-started/installing) installation instructions.

## Create a New Site and Initialize Git Repository

Choose a directory name for your site. This can be something arbitrary like ```example.com``` or ```example-site```, and does not need to exactly match the domain name.

```
hugo new site example.com
cd example.com
git init
```

## Install a Theme

Create your own theme, or select a pre-made theme from a site like [https://themes.gohugo.io](https://themes.gohugo.io) or [https://hugothemesfree.com/](https://hugothemesfree.com/).

Themes don't necessarily need to be copied to your site's Git repository, and can be added as Git submodules. This allows changes to the theme to be tracked separately from changes to your site's content.

When adding a theme submodule, remember to specify the path to the themes directory.

```
git submodule add https://github.com/kishaningithub/hugo-creative-portfolio-theme themes/hugo-creative-portfolio-theme
```

## Follow the Theme-Specific Configuration Instructions

Most themes require additional steps to copy their ```config.toml``` into place, or to create a ```config.toml``` from scratch. These instructions are usually outlined in the [README.md](https://github.com/kishaningithub/hugo-creative-portfolio-theme#installation) of the theme. For example:

- [https://github.com/kishaningithub/hugo-creative-portfolio-theme#installation](https://github.com/kishaningithub/hugo-creative-portfolio-theme#installation) 

```
cp themes/hugo-creative-portfolio-theme/exampleSite/config.toml .
```

## Create Content

### Create Your First Post From Scratch

```
hugo new posts/first-post.md
```

### Or Use the exampleSite Placeholder Content

Be sure to rsync all exampleSite directories. Using the ```--delete``` will delete all existing/default content, and sync contents from exampleSite.

```
rsync -aPvHx --delete themes/hugo-creative-portfolio-theme/exampleSite/content/ content/
rsync -aPvHx --delete themes/hugo-creative-portfolio-theme/exampleSite/static/ static/
```

## Start Local Hugo Server to Preview Content

```
hugo server -D
```

Open a web browser and visit [http://localhost:1313/](http://localhost:1313/).

## Modify Example Content

Many themes provide an abundance of example content pages to show off the theme's capabilities. These pages usually exactly match the pages from the Demo site (if there is one for the theme). Modify, delete, or create new pages as required for your site. 

For the first time working with a theme, don't assume that your first installation will be deployed to a live website. Instead, play around with the theme to get a feel for its capabilities, then start over from scratch when it's time to create production-ready content. Content can always be copied or moved from the first, experimental installation to the production installation.


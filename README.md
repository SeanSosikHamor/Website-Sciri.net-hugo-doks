[![Netlify Status](https://api.netlify.com/api/v1/badges/d114f6c6-0842-444b-80b3-dbeb24ff7751/deploy-status)](https://app.netlify.com/sites/sciri-net/deploys)

# Website-Sciri.net-hugo-doks

Sean's miscellaneous documentation site.

## Miscellaneous

Use [date-lastmod.sh](https://github.com/SeanSosikHamor/Snippets-Collection/blob/master/Jamstack/Scripts/date-lastmod.sh) to generate date: and lastmod: for Markdown files.

## Configuration

### Build Environment Versions and Base URL

- ./netlify.toml
- ./config/_default/config.toml

### Basic Site Description

Manually sync title, description, etc. between the following two files:

- ./config/_default/params.toml
- ./content/en/_index.md

## Content

### Menus

- ./config/_default/menus/menus.en.toml

### Front Page

- ./layouts/index.html

### Adding Pages

- [Doks Tutorial — Adding Pages](https://getdoks.org/tutorial/add-pages/)

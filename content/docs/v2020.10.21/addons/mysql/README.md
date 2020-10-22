---
title: MySQL Addon Overview | Stash
description: MySQL Addon Overview | Stash
menu:
  docs_v2020.10.21:
    identifier: stash-mysql-readme
    name: Readme
    parent: stash-mysql
    weight: -1
product_name: stash
menu_name: docs_v2020.10.21
section_menu_id: stash-addons
url: /docs/v2020.10.21/addons/mysql/
aliases:
- /docs/v2020.10.21/addons/mysql/README/
info:
  catalog: v2020.10.21
  cli: v0.11.3
  community: v0.11.3
  elasticsearch:
  - 5.6.4-v3
  - 6.2.4-v3
  - 6.3.0-v3
  - 6.4.0-v3
  - 6.5.3-v3
  - 6.8.0-v3
  - 7.2.0-v3
  - 7.3.2-v3
  enterprise: v0.11.3
  installer: v0.11.3
  mongodb:
  - 3.4.17-v3
  - 3.4.22-v3
  - 3.6.13-v3
  - 3.6.8-v3
  - 4.0.11-v3
  - 4.0.3-v3
  - 4.0.5-v3
  - 4.1.13-v3
  - 4.1.4-v3
  - 4.1.7-v3
  - 4.2.3-v3
  mysql:
  - 5.7.25-v3
  - 8.0.14-v3
  - 8.0.3-v3
  percona-xtradb:
  - 5.7-v3
  postgres:
  - 10.14.0-v1
  - 11.9.0-v1
  - 12.4.0-v1
  - 9.6.19-v1
  version: v2020.10.21
---

{{< notice type="warning" message="This is an Enterprise-only feature. Please install [Stash Enterprise Edition](/docs/v2020.10.21/setup/install/enterprise) to try this feature." >}}

# Stash MySQL Addon

Stash 0.9.0+ supports extending its functionality through addons. Stash MySQL addon enables Stash to backup and restore MySQL databases.

This guide will give you an overview of which MySQL versions are supported and how the docs are organized.

## Supported MySQL Versions

Stash supports backup and restore of the following MySQL versions:

{{< versionlist "mysql" "/docs/v2020.10.21/addons/mysql/guides/%s/mysql" >}}

## Documentation Overview

Stash MySQL documentations are organized as below:

- [How does it works?](/docs/v2020.10.21/addons/mysql/overview) gives an overview of how backup and restore process for MySQL database works in Stash.
- [Setup](/docs/v2020.10.21/addons/mysql/setup/install) shows how to install and uninstall MySQL addon for Stash.
- [Guides](/docs/v2020.10.21/addons/mysql/guides/8.0.14/mysql) contains step by step guides to backup and restore different versions of MySQL databases.

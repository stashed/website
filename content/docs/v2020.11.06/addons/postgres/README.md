---
title: PostgreSQL Addon Overview | Stash
description: PostgreSQL Addon Overview | Stash
menu:
  docs_v2020.11.06:
    identifier: stash-postgres-readme
    name: Readme
    parent: stash-postgres
    weight: -1
product_name: stash
menu_name: docs_v2020.11.06
section_menu_id: stash-addons
url: /docs/v2020.11.06/addons/postgres/
aliases:
- /docs/v2020.11.06/addons/postgres/README/
info:
  catalog: v2020.11.06
  cli: v0.11.6
  community: v0.11.6
  elasticsearch:
  - 5.6.4-v4
  - 6.2.4-v4
  - 6.3.0-v4
  - 6.4.0-v4
  - 6.5.3-v4
  - 6.8.0-v4
  - 7.2.0-v4
  - 7.3.2-v4
  enterprise: v0.11.6
  installer: v0.11.6
  mongodb:
  - 3.4.17-v4
  - 3.4.22-v4
  - 3.6.13-v4
  - 3.6.8-v4
  - 4.0.11-v4
  - 4.0.3-v4
  - 4.0.5-v4
  - 4.1.13-v4
  - 4.1.4-v4
  - 4.1.7-v4
  - 4.2.3-v4
  mysql:
  - 5.7.25-v4
  - 8.0.14-v4
  - 8.0.3-v4
  percona-xtradb:
  - 5.7-v4
  postgres:
  - 10.14.0-v2
  - 11.9.0-v2
  - 12.4.0-v2
  - 9.6.19-v2
  version: v2020.11.06
---

{{< notice type="warning" message="This is an Enterprise-only feature. Please install [Stash Enterprise Edition](/docs/v2020.11.06/setup/install/enterprise) to try this feature." >}}

# Stash PostgreSQL Addon

Stash 0.9.0+ supports extending its functionality through addons. Stash PostgreSQL addon enables Stash to backup and restore PostgreSQL databases.

This guide will give you an overview of which PostgreSQL versions are supported and how the docs are organized.

## Supported PostgreSQL Versions

Stash supports backup and restore of the following PostgreSQL versions:

{{< versionlist "postgres" "/docs/v2020.11.06/addons/postgres/guides/%s/standalone" >}}

## Documentation Overview

Stash PostgreSQL documentations are organized as below:

- [How does it works?](/docs/v2020.11.06/addons/postgres/overview) gives an overview of how backup and restore process for PostgreSQL database works in Stash.
- [Setup](/docs/v2020.11.06/addons/postgres/setup/install) shows how to install and uninstall PostgreSQL addon for Stash.
- [Guides](/docs/v2020.11.06/addons/postgres/guides/11.2/standalone) contains step by step guides to backup and restore different versions of PostgreSQL databases.

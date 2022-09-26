---
title: PostgreSQL Addon Overview | Stash
description: PostgreSQL Addon Overview | Stash
menu:
  docs_v2022.09.29:
    identifier: stash-postgres-readme
    name: Readme
    parent: stash-postgres
    weight: -1
product_name: stash
menu_name: docs_v2022.09.29
section_menu_id: stash-addons
url: /docs/v2022.09.29/addons/postgres/
aliases:
- /docs/v2022.09.29/addons/postgres/README/
info:
  cli: v0.23.0
  community: v0.23.0
  elasticsearch:
  - 5.6.4-v19
  - 6.2.4-v19
  - 6.3.0-v19
  - 6.4.0-v19
  - 6.5.3-v19
  - 6.8.0-v19
  - 7.14.0-v5
  - 7.2.0-v19
  - 7.3.2-v19
  - 8.2.0-v2
  enterprise: v0.23.0
  etcd:
  - 3.5.0-v6
  installer: v2022.09.29
  kubedump:
  - 0.1.0-v2
  mariadb:
  - 10.5.8-v12
  mongodb:
  - 3.4.17-v19
  - 3.4.22-v19
  - 3.6.13-v19
  - 3.6.8-v19
  - 4.0.11-v19
  - 4.0.3-v19
  - 4.0.5-v19
  - 4.1.13-v19
  - 4.1.4-v19
  - 4.1.7-v19
  - 4.2.3-v19
  - 4.4.6-v10
  - 5.0.3-v7
  mysql:
  - 5.7.25-v19
  - 8.0.14-v19
  - 8.0.21-v13
  - 8.0.3-v19
  nats:
  - 2.6.1-v7
  - 2.8.2-v2
  percona-xtradb:
  - 5.7-v14
  postgres:
  - 10.14-v18
  - 11.9-v18
  - 12.4-v18
  - 13.1-v15
  - 14.0-v7
  - 9.6.19-v18
  redis:
  - 5.0.13-v7
  - 6.2.5-v7
  - 7.0.5
  ui-server: v0.5.0
  version: v2022.09.29
---

{{< notice type="warning" message="This is an Enterprise-only feature. Please install [Stash Enterprise Edition](/docs/v2022.09.29/setup/install/enterprise) to try this feature." >}}

# Stash PostgreSQL Addon

Stash 0.9.0+ supports extending its functionality through addons. Stash PostgreSQL addon enables Stash to backup and restore PostgreSQL databases.

This guide will give you an overview of which PostgreSQL versions are supported and how the docs are organized.

## Supported PostgreSQL Versions

Stash has the following addon versions for PostgreSQL:

{{< versionlist "postgres">}}

Here, the addon follows `M.M.P` versioning scheme where `M.M.P` (Major.Minor.Patch) represents the respective database version.

## Addon Version Compatibility

Any addon with matching major version with the database version should be able to take backup of that database. For example, PostgreSQL addon with version `12.x.x` should be able take backup of any PostgreSQL of `12.x.x` series. However, this might not be true for some versions. In that case, we will have separate addon for that version.

## Documentation Overview

Stash PostgreSQL documentations are organized as below:

- [How does it work?](/docs/v2022.09.29/addons/postgres/overview/) gives an overview of how backup and restore process for PostgreSQL database works in Stash.
- [Standalone PostgreSQL](/docs/v2022.09.29/addons/postgres/standalone/) shows how to backup and restore a standalone PostgreSQL database using Stash.
- [Auto-Backup](/docs/v2022.09.29/addons/postgres/auto-backup/) shows how to configure a generic backup template for all the PostgreSQL databases of a cluster.

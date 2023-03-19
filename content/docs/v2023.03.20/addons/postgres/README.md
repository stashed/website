---
title: PostgreSQL Addon Overview | Stash
description: PostgreSQL Addon Overview | Stash
menu:
  docs_v2023.03.20:
    identifier: stash-postgres-readme
    name: Readme
    parent: stash-postgres
    weight: -1
product_name: stash
menu_name: docs_v2023.03.20
section_menu_id: stash-addons
url: /docs/v2023.03.20/addons/postgres/
aliases:
- /docs/v2023.03.20/addons/postgres/README/
info:
  cli: v0.28.0
  community: v0.28.0
  elasticsearch:
  - 5.6.4-v24
  - 6.2.4-v24
  - 6.3.0-v24
  - 6.4.0-v24
  - 6.5.3-v24
  - 6.8.0-v24
  - 7.14.0-v10
  - 7.2.0-v24
  - 7.3.2-v24
  - 8.2.0-v7
  enterprise: v0.28.0
  etcd:
  - 3.5.0-v11
  installer: v2023.03.20
  kubedump:
  - 0.1.0-v7
  mariadb:
  - 10.5.8-v17
  mongodb:
  - 3.4.17-v24
  - 3.4.22-v24
  - 3.6.13-v24
  - 3.6.8-v24
  - 4.0.11-v24
  - 4.0.3-v24
  - 4.0.5-v24
  - 4.1.13-v24
  - 4.1.4-v24
  - 4.1.7-v24
  - 4.2.3-v24
  - 4.4.6-v15
  - 5.0.3-v12
  mysql:
  - 5.7.25-v24
  - 8.0.14-v24
  - 8.0.21-v18
  - 8.0.3-v24
  nats:
  - 2.6.1-v12
  - 2.8.2-v7
  percona-xtradb:
  - 5.7-v19
  postgres:
  - 10.14-v23
  - 11.9-v23
  - 12.4-v23
  - 13.1-v20
  - 14.0-v12
  - 15.1-v4
  - 9.6.19-v23
  redis:
  - 5.0.13-v12
  - 6.2.5-v12
  - 7.0.5-v5
  ui-server: v0.10.0
  vault:
  - 1.10.3-v4
  version: v2023.03.20
---

{{< notice type="warning" message="This is an Enterprise-only feature. Please install [Stash Enterprise Edition](/docs/v2023.03.20/setup/install/enterprise/) to try this feature." >}}

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

- [How does it work?](/docs/v2023.03.20/addons/postgres/overview/) gives an overview of how backup and restore process for PostgreSQL database works in Stash.
- [Standalone PostgreSQL](/docs/v2023.03.20/addons/postgres/standalone/) shows how to backup and restore a standalone PostgreSQL database using Stash.
- [Auto-Backup](/docs/v2023.03.20/addons/postgres/auto-backup/) shows how to configure a generic backup template for all the PostgreSQL databases of a cluster.

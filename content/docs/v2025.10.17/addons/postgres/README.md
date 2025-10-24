---
title: PostgreSQL Addon Overview | Stash
description: PostgreSQL Addon Overview | Stash
menu:
  docs_v2025.10.17:
    identifier: stash-postgres-readme
    name: Readme
    parent: stash-postgres
    weight: -1
product_name: stash
menu_name: docs_v2025.10.17
section_menu_id: stash-addons
url: /docs/v2025.10.17/addons/postgres/
aliases:
- /docs/v2025.10.17/addons/postgres/README/
info:
  cli: v0.42.0
  community: v0.42.0
  elasticsearch:
  - 5.6.4-v38
  - 6.2.4-v38
  - 6.3.0-v38
  - 6.4.0-v38
  - 6.5.3-v38
  - 6.8.0-v38
  - 7.14.0-v24
  - 7.2.0-v38
  - 7.3.2-v38
  - 8.2.0-v21
  enterprise: v0.42.0
  etcd:
  - 3.5.0-v25
  installer: v2025.10.17
  kubedump:
  - 0.2.0-v6
  mariadb:
  - 10.6.23-v1
  mongodb:
  - 3.4.17-v39
  - 3.4.22-v39
  - 3.6.13-v39
  - 3.6.8-v39
  - 4.0.11-v39
  - 4.0.3-v39
  - 4.0.5-v39
  - 4.1.13-v39
  - 4.1.4-v39
  - 4.1.7-v39
  - 4.2.3-v39
  - 4.4.6-v30
  - 5.0.15-v12
  - 5.0.3-v27
  - 6.0.5-v15
  mysql:
  - 5.7.25-v39
  - 8.0.14-v38
  - 8.0.21-v32
  - 8.0.3-v38
  nats:
  - 2.6.1-v26
  - 2.8.2-v21
  percona-xtradb:
  - 5.7-v33
  postgres:
  - 10.14-v37
  - 11.9-v37
  - 12.4-v37
  - 13.1-v34
  - 14.0-v26
  - 15.1-v18
  - 16.1-v7
  - 17.2-v5
  - 9.6.19-v37
  redis:
  - 5.0.13-v26
  - 6.2.5-v26
  - 7.0.5-v19
  ui-server: v0.23.0
  vault:
  - 1.10.3-v18
  version: v2025.10.17
---

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

- [How does it work?](/docs/v2025.10.17/addons/postgres/overview/) gives an overview of how backup and restore process for PostgreSQL database works in Stash.
- [Standalone PostgreSQL](/docs/v2025.10.17/addons/postgres/standalone/) shows how to backup and restore a standalone PostgreSQL database using Stash.
- [Auto-Backup](/docs/v2025.10.17/addons/postgres/auto-backup/) shows how to configure a generic backup template for all the PostgreSQL databases of a cluster.

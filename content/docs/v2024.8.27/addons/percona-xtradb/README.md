---
title: Percona XtraDB Addon Overview | Stash
description: Percona XtraDB Addon Overview | Stash
menu:
  docs_v2024.8.27:
    identifier: stash-percona-xtradb-readme
    name: Readme
    parent: stash-percona-xtradb
    weight: -1
product_name: stash
menu_name: docs_v2024.8.27
section_menu_id: stash-addons
url: /docs/v2024.8.27/addons/percona-xtradb/
aliases:
- /docs/v2024.8.27/addons/percona-xtradb/README/
info:
  cli: v0.35.0
  community: v0.35.0
  elasticsearch:
  - 5.6.4-v32
  - 6.2.4-v32
  - 6.3.0-v32
  - 6.4.0-v32
  - 6.5.3-v32
  - 6.8.0-v32
  - 7.14.0-v18
  - 7.2.0-v32
  - 7.3.2-v32
  - 8.2.0-v15
  enterprise: v0.35.0
  etcd:
  - 3.5.0-v19
  installer: v2024.8.27
  kubedump:
  - 0.1.0-v15
  mariadb:
  - 10.5.8-v26
  mongodb:
  - 3.4.17-v32
  - 3.4.22-v32
  - 3.6.13-v32
  - 3.6.8-v32
  - 4.0.11-v32
  - 4.0.3-v32
  - 4.0.5-v32
  - 4.1.13-v32
  - 4.1.4-v32
  - 4.1.7-v32
  - 4.2.3-v32
  - 4.4.6-v23
  - 5.0.15-v5
  - 5.0.3-v20
  - 6.0.5-v8
  mysql:
  - 5.7.25-v32
  - 8.0.14-v32
  - 8.0.21-v26
  - 8.0.3-v32
  nats:
  - 2.6.1-v20
  - 2.8.2-v15
  percona-xtradb:
  - 5.7-v27
  postgres:
  - 10.14-v31
  - 11.9-v31
  - 12.4-v31
  - 13.1-v28
  - 14.0-v20
  - 15.1-v12
  - 16.1-v1
  - 9.6.19-v31
  redis:
  - 5.0.13-v20
  - 6.2.5-v20
  - 7.0.5-v13
  ui-server: v0.16.0
  vault:
  - 1.10.3-v12
  version: v2024.8.27
---

# Stash Percona XtraDB Addon

Stash 0.9.0+ supports extending its functionality through addons. Stash Percona XtraDB addon enables Stash to backup and restore Percona XtraDB databases.

This guide will give you an overview of which Percona XtraDB versions are supported and how the docs are organized.

## Supported Percona XtraDB Versions


Stash has the following addon versions for Percona-XtraDB:

{{< versionlist "percona-xtradb">}}

Here, the addon follows `M.M.P` versioning scheme where `M.M.P` (Major.Minor.Patch) represents the respective database version.

## Addon Version Compatibility

Any addon with matching major version with the database version should be able to take backup of that database. For example, PerconaXtraDB addon with version `5.x.x` should be able take backup of any PerconaXtraDB of `5.x.x` series. However, this might not be true for some versions. In that case, we will have separate addon for that version.

## Documentation Overview

Stash Percona XtraDB documentations are organized as below:

- [How does it work?](/docs/v2024.8.27/addons/percona-xtradb/overview/) gives an overview of how backup and restore process for Percona XtraDB database works in Stash.
- [Standalone Percona-XtraDB](/docs/v2024.8.27/addons/percona-xtradb/standalone/) shows how to backup and restore a standalone Percona-XtraDB database.
- [Percona-XtraDB Cluster](/docs/v2024.8.27/addons/percona-xtradb/cluster/) shows how to backup & restore  a Percona-XtraDB cluster.
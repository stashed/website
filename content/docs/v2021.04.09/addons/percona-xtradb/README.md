---
title: Percona XtraDB Addon Overview | Stash
description: Percona XtraDB Addon Overview | Stash
menu:
  docs_v2021.04.09:
    identifier: stash-percona-xtradb-readme
    name: Readme
    parent: stash-percona-xtradb
    weight: -1
product_name: stash
menu_name: docs_v2021.04.09
section_menu_id: stash-addons
url: /docs/v2021.04.09/addons/percona-xtradb/
aliases:
- /docs/v2021.04.09/addons/percona-xtradb/README/
info:
  cli: v0.12.2
  community: v0.12.2
  elasticsearch:
  - 5.6.4-v8
  - 6.2.4-v8
  - 6.3.0-v8
  - 6.4.0-v8
  - 6.5.3-v8
  - 6.8.0-v8
  - 7.2.0-v8
  - 7.3.2-v8
  enterprise: v0.12.2
  installer: v2021.04.09
  mariadb:
  - 10.5.8-v2
  mongodb:
  - 3.4.17-v7
  - 3.4.22-v7
  - 3.6.13-v7
  - 3.6.8-v7
  - 4.0.11-v7
  - 4.0.3-v7
  - 4.0.5-v7
  - 4.1.13-v7
  - 4.1.4-v7
  - 4.1.7-v7
  - 4.2.3-v7
  mysql:
  - 5.7.25-v8
  - 8.0.14-v8
  - 8.0.21-v2
  - 8.0.3-v8
  percona-xtradb:
  - 5.7-v3
  postgres:
  - 10.14-v6
  - 11.9-v6
  - 12.4-v6
  - 13.1-v3
  - 9.6.19-v6
  version: v2021.04.09
---

{{< notice type="warning" message="This is an Enterprise-only feature. Please install [Stash Enterprise Edition](/docs/v2021.04.09/setup/install/enterprise) to try this feature." >}}

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

- [How does it works?](/docs/v2021.04.09/addons/percona-xtradb/overview/) gives an overview of how backup and restore process for Percona XtraDB database works in Stash.
- [Standalone Percona-XtraDB](/docs/v2021.04.09/addons/percona-xtradb/standalone/) shows how to backup and restore a standalone Percona-XtraDB database.
- [Percona-XtraDB Cluster](/docs/v2021.04.09/addons/percona-xtradb/cluster/) shows how to backup & restore  a Percona-XtraDB cluster.

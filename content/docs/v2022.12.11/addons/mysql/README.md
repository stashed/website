---
title: MySQL Addon Overview | Stash
description: MySQL Addon Overview | Stash
menu:
  docs_v2022.12.11:
    identifier: stash-mysql-readme
    name: Readme
    parent: stash-mysql
    weight: -1
product_name: stash
menu_name: docs_v2022.12.11
section_menu_id: stash-addons
url: /docs/v2022.12.11/addons/mysql/
aliases:
- /docs/v2022.12.11/addons/mysql/README/
info:
  cli: v0.24.0
  community: v0.24.0
  elasticsearch:
  - 5.6.4-v20
  - 6.2.4-v20
  - 6.3.0-v20
  - 6.4.0-v20
  - 6.5.3-v20
  - 6.8.0-v20
  - 7.14.0-v6
  - 7.2.0-v20
  - 7.3.2-v20
  - 8.2.0-v3
  enterprise: v0.24.0
  etcd:
  - 3.5.0-v7
  installer: v2022.12.11
  kubedump:
  - 0.1.0-v3
  mariadb:
  - 10.5.8-v13
  mongodb:
  - 3.4.17-v20
  - 3.4.22-v20
  - 3.6.13-v20
  - 3.6.8-v20
  - 4.0.11-v20
  - 4.0.3-v20
  - 4.0.5-v20
  - 4.1.13-v20
  - 4.1.4-v20
  - 4.1.7-v20
  - 4.2.3-v20
  - 4.4.6-v11
  - 5.0.3-v8
  mysql:
  - 5.7.25-v20
  - 8.0.14-v20
  - 8.0.21-v14
  - 8.0.3-v20
  nats:
  - 2.6.1-v8
  - 2.8.2-v3
  percona-xtradb:
  - 5.7-v15
  postgres:
  - 10.14-v19
  - 11.9-v19
  - 12.4-v19
  - 13.1-v16
  - 14.0-v8
  - "15.1"
  - 9.6.19-v19
  redis:
  - 5.0.13-v8
  - 6.2.5-v8
  - 7.0.5-v1
  ui-server: v0.6.0
  vault:
  - 1.10.3
  version: v2022.12.11
---

{{< notice type="warning" message="This is an Enterprise-only feature. Please install [Stash Enterprise Edition](/docs/v2022.12.11/setup/install/enterprise/) to try this feature." >}}

# Stash MySQL Addon

Stash 0.9.0+ supports extending its functionality through addons. Stash MySQL addon enables Stash to backup and restore MySQL databases.

This guide will give you an overview of which MySQL versions are supported and how the docs are organized.

## Supported MySQL Versions

Stash has the following addon versions for MySQL:

{{< versionlist "mysql">}}

Here, the addon follows `M.M.P` versioning scheme where `M.M.P` (Major.Minor.Patch) represents the respective database version.

## Addon Version Compatibility

Any addon with matching major version with the database version should be able to take backup of that database. For example, MySQL addon with version `8.x.x` should be able take backup of any MySQL of `8.x.x` series. However, this might not be true for some versions. In that case, we will have separate addon for that version.

## Documentation Overview

Stash MySQL documentations are organized as below:

- [How does it work?](/docs/v2022.12.11/addons/mysql/overview/) gives an overview of how backup and restore process for MySQL database works in Stash.
- [Standalone MySQL Database](/docs/v2022.12.11/addons/mysql/standalone/) shows how to backup and restore a standalone MySQL database.

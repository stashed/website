---
title: MariaDB Addon Overview | Stash
description: MariaDB Addon Overview | Stash
menu:
  docs_v2021.08.02:
    identifier: stash-mariadb-readme
    name: Readme
    parent: stash-mariadb
    weight: -1
product_name: stash
menu_name: docs_v2021.08.02
section_menu_id: stash-addons
url: /docs/v2021.08.02/addons/mariadb/
aliases:
- /docs/v2021.08.02/addons/mariadb/README/
info:
  cli: v0.15.0
  community: v0.15.0
  elasticsearch:
  - 5.6.4-v12
  - 6.2.4-v12
  - 6.3.0-v12
  - 6.4.0-v12
  - 6.5.3-v12
  - 6.8.0-v12
  - 7.2.0-v12
  - 7.3.2-v12
  enterprise: v0.15.0
  installer: v2021.08.02
  mariadb:
  - 10.5.8-v5
  mongodb:
  - 3.4.17-v11
  - 3.4.22-v11
  - 3.6.13-v11
  - 3.6.8-v11
  - 4.0.11-v11
  - 4.0.3-v11
  - 4.0.5-v11
  - 4.1.13-v11
  - 4.1.4-v11
  - 4.1.7-v11
  - 4.2.3-v11
  - 4.4.6-v2
  mysql:
  - 5.7.25-v12
  - 8.0.14-v12
  - 8.0.21-v6
  - 8.0.3-v12
  percona-xtradb:
  - 5.7-v7
  postgres:
  - 10.14-v10
  - 11.9-v10
  - 12.4-v10
  - 13.1-v7
  - 9.6.19-v10
  redis:
  - 5.0.13
  - 6.2.5
  version: v2021.08.02
---

{{< notice type="warning" message="This is an Enterprise-only feature. Please install [Stash Enterprise Edition](/docs/v2021.08.02/setup/install/enterprise) to try this feature." >}}

# Stash MariaDB Addon

Stash 0.9.0+ supports extending its functionality through addons. Stash MariaDB addon enables Stash to backup and restore MariaDB databases.

This guide will give you an overview of which MariaDB versions are supported and how the docs are organized.

## Supported MariaDB Versions

Stash has the following addon versions for MariaDB:

{{< versionlist "mariadb">}}

Here, the addon follows `M.M.P` versioning scheme where `M.M.P` (Major.Minor.Patch) represents the respective database version.

## Addon Version Compatibility

Any addon with matching major version with the database version should be able to take backup of that database. For example, MariaDB addon with version `10.x.x` should be able take backup of any MariaDB of `10.x.x` series. However, this might not be true for some versions. In that case, we will have separate addon for that version.

## Documentation Overview

Stash MariaDB documentations are organized as below:

- [How does it works?](/docs/v2021.08.02/addons/mariadb/overview/) gives an overview of how backup and restore process for MariaDB database works in Stash.
- [Helm managed MariaDB](/docs/v2021.08.02/addons/mariadb/helm/) shows how to backup and restore a Helm managed MariaDB database.
- [Auto-Backup](/docs/v2021.08.02/addons/mariadb/auto-backup/) shows how to configure a generic backup template for all the MariaDB databases of a cluster.
- [Customizing Backup & Restore Process](/docs/v2021.08.02/addons/mariadb/customization/) shows how to customize the backup & restore process.

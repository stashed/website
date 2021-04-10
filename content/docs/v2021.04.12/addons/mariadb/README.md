---
title: MariaDB Addon Overview | Stash
description: MariaDB Addon Overview | Stash
menu:
  docs_v2021.04.12:
    identifier: stash-mariadb-readme
    name: Readme
    parent: stash-mariadb
    weight: -1
product_name: stash
menu_name: docs_v2021.04.12
section_menu_id: stash-addons
url: /docs/v2021.04.12/addons/mariadb/
aliases:
- /docs/v2021.04.12/addons/mariadb/README/
info:
  cli: v0.12.3
  community: v0.12.3
  elasticsearch:
  - 5.6.4-v9
  - 6.2.4-v9
  - 6.3.0-v9
  - 6.4.0-v9
  - 6.5.3-v9
  - 6.8.0-v9
  - 7.2.0-v9
  - 7.3.2-v9
  enterprise: v0.12.3
  installer: v2021.04.12
  mariadb:
  - 10.5.8-v3
  mongodb:
  - 3.4.17-v8
  - 3.4.22-v8
  - 3.6.13-v8
  - 3.6.8-v8
  - 4.0.11-v8
  - 4.0.3-v8
  - 4.0.5-v8
  - 4.1.13-v8
  - 4.1.4-v8
  - 4.1.7-v8
  - 4.2.3-v8
  mysql:
  - 5.7.25-v9
  - 8.0.14-v9
  - 8.0.21-v3
  - 8.0.3-v9
  percona-xtradb:
  - 5.7-v4
  postgres:
  - 10.14-v7
  - 11.9-v7
  - 12.4-v7
  - 13.1-v4
  - 9.6.19-v7
  version: v2021.04.12
---

{{< notice type="warning" message="This is an Enterprise-only feature. Please install [Stash Enterprise Edition](/docs/v2021.04.12/setup/install/enterprise) to try this feature." >}}

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

- [How does it works?](/docs/v2021.04.12/addons/mariadb/overview/) gives an overview of how backup and restore process for MariaDB database works in Stash.
- [Helm managed MariaDB](/docs/v2021.04.12/addons/mariadb/helm/) shows how to backup and restore a Helm managed MariaDB database.
- [Auto-Backup](/docs/v2021.04.12/addons/mariadb/auto-backup/) shows how to configure a generic backup template for all the MariaDB databases of a cluster.
- [Customizing Backup & Restore Process](/docs/v2021.04.12/addons/mariadb/customization/) shows how to customize the backup & restore process.

---
title: MariaDB Addon Overview | Stash
description: MariaDB Addon Overview | Stash
menu:
  docs_v2021.03.11:
    identifier: stash-mariadb-readme
    name: Readme
    parent: stash-mariadb
    weight: -1
product_name: stash
menu_name: docs_v2021.03.11
section_menu_id: stash-addons
url: /docs/v2021.03.11/addons/mariadb/
aliases:
- /docs/v2021.03.11/addons/mariadb/README/
info:
  catalog: v2021.03.11
  cli: v0.11.11
  community: v0.11.11
  elasticsearch:
  - 5.6.4-v7
  - 6.2.4-v7
  - 6.3.0-v7
  - 6.4.0-v7
  - 6.5.3-v7
  - 6.8.0-v7
  - 7.2.0-v7
  - 7.3.2-v7
  enterprise: v0.11.11
  installer: v0.11.11
  mariadb:
  - 10.5.8-v1
  mongodb:
  - 3.4.17-v6
  - 3.4.22-v6
  - 3.6.13-v6
  - 3.6.8-v6
  - 4.0.11-v6
  - 4.0.3-v6
  - 4.0.5-v6
  - 4.1.13-v6
  - 4.1.4-v6
  - 4.1.7-v6
  - 4.2.3-v6
  mysql:
  - 5.7.25-v7
  - 8.0.14-v7
  - 8.0.21-v1
  - 8.0.3-v7
  percona-xtradb:
  - 5.7.0-v2
  postgres:
  - 10.14.0-v5
  - 11.9.0-v5
  - 12.4.0-v5
  - 13.1.0-v2
  - 9.6.19-v5
  version: v2021.03.11
---

{{< notice type="warning" message="This is an Enterprise-only feature. Please install [Stash Enterprise Edition](/docs/v2021.03.11/setup/install/enterprise) to try this feature." >}}

# Stash MariaDB Addon

Stash 0.9.0+ supports extending its functionality through addons. Stash MariaDB addon enables Stash to backup and restore MariaDB databases.

This guide will give you an overview of which MariaDB versions are supported and how the docs are organized.

## Supported MariaDB Versions

Stash supports backup and restore of the following MariaDB versions:

{{< versionlist "mariadb" "/docs/v2021.03.11/addons/mariadb/guides/%s/helm/logical" >}}

Here, the addon follows `M.M.P-vX` versioning scheme where `M.M.P` (Major.Minor.Patch) represents the respective database version and an optional `-vX` (here, `X` is a monotonically increasing integer) is added if there is any breaking change in the addon image compared to the previous release.

{{< notice type="warning" message="If you update Stash operator to a newer release and the supported addon versions in the newer release has different `-vX` suffix, you have to update the old addons too. Otherwise, backup may not work. In this case, just uninstall the old addons and install the new addons." >}}

## Addon Version Compatibility

Any addon with matching major version with the database version should be able to take backup of that database. For example, MariaDB addon with version `10.x.x-vX` should be able take backup of any MariaDB of `10.x.x` series. However, this might not be true for some versions. In that case, we will have separate addon for that version.

## Documentation Overview

Stash MariaDB documentations are organized as below:

- [How does it works?](/docs/v2021.03.11/addons/mariadb/overview) gives an overview of how backup and restore process for MariaDB database works in Stash.
- [Setup](/docs/v2021.03.11/addons/mariadb/setup/install) shows how to install and uninstall MariaDB addon for Stash.
- **Guides** contains step by step guides to backup and restore different versions of MariaDB databases.

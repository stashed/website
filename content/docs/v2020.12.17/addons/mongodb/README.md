---
title: MongoDB Addon Overview | Stash
description: MongoDB Addon Overview | Stash
menu:
  docs_v2020.12.17:
    identifier: stash-mongodb-readme
    name: Readme
    parent: stash-mongodb
    weight: -1
product_name: stash
menu_name: docs_v2020.12.17
section_menu_id: stash-addons
url: /docs/v2020.12.17/addons/mongodb/
aliases:
- /docs/v2020.12.17/addons/mongodb/README/
info:
  catalog: v2020.12.17
  cli: v0.11.8
  community: v0.11.8
  elasticsearch:
  - 5.6.4-v5
  - 6.2.4-v5
  - 6.3.0-v5
  - 6.4.0-v5
  - 6.5.3-v5
  - 6.8.0-v5
  - 7.2.0-v5
  - 7.3.2-v5
  enterprise: v0.11.8
  installer: v0.11.8
  mariadb:
  - 10.5.8
  mongodb:
  - 3.4.17-v5
  - 3.4.22-v5
  - 3.6.13-v5
  - 3.6.8-v5
  - 4.0.11-v5
  - 4.0.3-v5
  - 4.0.5-v5
  - 4.1.13-v5
  - 4.1.4-v5
  - 4.1.7-v5
  - 4.2.3-v5
  mysql:
  - 5.7.25-v5
  - 8.0.14-v5
  - 8.0.3-v5
  percona-xtradb:
  - 5.7.0-v1
  postgres:
  - 10.14.0-v4
  - 11.9.0-v4
  - 12.4.0-v4
  - 13.1.0-v1
  - 9.6.19-v4
  version: v2020.12.17
---

{{< notice type="warning" message="This is an Enterprise-only feature. Please install [Stash Enterprise Edition](/docs/v2020.12.17/setup/install/enterprise) to try this feature." >}}

# Stash MongoDB Addon

Stash 0.9.0+ supports extending its functionality through addons. Stash MongoDB addon enables Stash to backup and restore MongoDB databases.

This guide will give you an overview of which MongoDB versions are supported and how the docs are organized.

## Supported MongoDB Versions

Stash supports backup and restore of the following MongoDB versions:

{{< versionlist "mongodb" "/docs/v2020.12.17/addons/mongodb/guides/%s/mongodb" >}}

Here, the addon follows `M.M.P-vX` versioning scheme where `M.M.P` (Major.Minor.Patch) represents the respective database version and an optional `-vX` (here, `X` is a monotonically increasing integer) is added if there is any breaking change in the addon image compared to the previous release.

{{< notice type="warning" message="If you update Stash operator to a newer release and the supported addon versions in the newer release has different `-vX` suffix, you have to update the old addons too. Otherwise, backup may not work. In this case, just uninstall the old addons and install the new addons." >}}

## Addon Version Compatibility

Any addon with matching major version with the database version should be able to take backup of that database. For example, MongoDB addon with version `4.x.x-vX` should be able take backup of any MongoDB of `4.x.x` series. However, this might not be true for some versions. In that case, we will have separate addon for that version.

## Documentation Overview

Stash MongoDB documentations are organized as below:

- [How does it works?](/docs/v2020.12.17/addons/mongodb/overview) gives an overview of how backup and restore process for MongoDB database works in Stash.
- [Setup](/docs/v2020.12.17/addons/mongodb/setup/install) shows how to install and uninstall MongoDB addon for Stash.
- **Guides** contains step by step guides to backup and restore different versions of MongoDB databases.

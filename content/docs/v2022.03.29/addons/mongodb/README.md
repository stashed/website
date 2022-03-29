---
title: MongoDB Addon Overview | Stash
description: MongoDB Addon Overview | Stash
menu:
  docs_v2022.03.29:
    identifier: stash-mongodb-readme
    name: Readme
    parent: stash-mongodb
    weight: -1
product_name: stash
menu_name: docs_v2022.03.29
section_menu_id: stash-addons
url: /docs/v2022.03.29/addons/mongodb/
aliases:
- /docs/v2022.03.29/addons/mongodb/README/
info:
  cli: v0.19.0
  community: v0.19.0
  elasticsearch:
  - 5.6.4-v16
  - 6.2.4-v16
  - 6.3.0-v16
  - 6.4.0-v16
  - 6.5.3-v16
  - 6.8.0-v16
  - 7.14.0-v2
  - 7.2.0-v16
  - 7.3.2-v16
  enterprise: v0.19.0
  etcd:
  - 3.5.0-v3
  installer: v2022.03.29
  mariadb:
  - 10.5.8-v9
  mongodb:
  - 3.4.17-v15
  - 3.4.22-v15
  - 3.6.13-v15
  - 3.6.8-v15
  - 4.0.11-v15
  - 4.0.3-v15
  - 4.0.5-v15
  - 4.1.13-v15
  - 4.1.4-v15
  - 4.1.7-v15
  - 4.2.3-v15
  - 4.4.6-v6
  - 5.0.3-v3
  mysql:
  - 5.7.25-v16
  - 8.0.14-v16
  - 8.0.21-v10
  - 8.0.3-v16
  nats:
  - 2.6.1-v3
  percona-xtradb:
  - 5.7-v11
  postgres:
  - 10.14-v14
  - 11.9-v14
  - 12.4-v14
  - 13.1-v11
  - 14.0-v3
  - 9.6.19-v14
  redis:
  - 5.0.13-v4
  - 6.2.5-v4
  ui-server: v0.2.0
  version: v2022.03.29
---

{{< notice type="warning" message="This is an Enterprise-only feature. Please install [Stash Enterprise Edition](/docs/v2022.03.29/setup/install/enterprise) to try this feature." >}}

# Stash MongoDB Addon

Stash 0.9.0+ supports extending its functionality through addons. Stash MongoDB addon enables Stash to backup and restore MongoDB databases.

This guide will give you an overview of which MongoDB versions are supported and how the docs are organized.

## Supported MongoDB Versions

Stash has the following addon versions for MongoDB:

{{< versionlist "mongodb">}}

Here, the addon follows `M.M.P` versioning scheme where `M.M.P` (Major.Minor.Patch) represents the respective database version.

## Addon Version Compatibility

Any addon with matching major version with the database version should be able to take backup of that database. For example, MongoDB addon with version `4.x.x` should be able take backup of any MongoDB of `4.x.x` series. However, this might not be true for some versions. In that case, we will have separate addon for that version.

## Documentation Overview

Stash MongoDB documentations are organized as below:

- [How does it work?](/docs/v2022.03.29/addons/mongodb/overview/) gives an overview of how backup and restore process for MongoDB database works in Stash.
- [Standalone MongoDB](/docs/v2022.03.29/addons/mongodb/standalone/) shows how to backup and restore a standalone MongoDB database.
- [MongoDB ReplicaSet](/docs/v2022.03.29/addons/mongodb/replicaset/) shows how to backup & restore  a MongoDB ReplicaSet.
- [Sharded MongoDB Cluster](/docs/v2022.03.29/addons/mongodb/sharding/) shows how to backup & restore a sharded MongoDB cluster.

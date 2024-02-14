---
title: Redis Addon Overview | Stash
description: Redis Addon Overview | Stash
menu:
  docs_v2024.2.13:
    identifier: stash-redis-readme
    name: Readme
    parent: stash-redis
    weight: -1
product_name: stash
menu_name: docs_v2024.2.13
section_menu_id: stash-addons
url: /docs/v2024.2.13/addons/redis/
aliases:
- /docs/v2024.2.13/addons/redis/README/
info:
  cli: v0.33.0
  community: v0.33.0
  elasticsearch:
  - 5.6.4-v30
  - 6.2.4-v30
  - 6.3.0-v30
  - 6.4.0-v30
  - 6.5.3-v30
  - 6.8.0-v30
  - 7.14.0-v16
  - 7.2.0-v30
  - 7.3.2-v30
  - 8.2.0-v13
  enterprise: v0.33.0
  etcd:
  - 3.5.0-v17
  installer: v2024.2.13
  kubedump:
  - 0.1.0-v13
  mariadb:
  - 10.5.8-v24
  mongodb:
  - 3.4.17-v30
  - 3.4.22-v30
  - 3.6.13-v30
  - 3.6.8-v30
  - 4.0.11-v30
  - 4.0.3-v30
  - 4.0.5-v30
  - 4.1.13-v30
  - 4.1.4-v30
  - 4.1.7-v30
  - 4.2.3-v30
  - 4.4.6-v21
  - 5.0.15-v3
  - 5.0.3-v18
  - 6.0.5-v6
  mysql:
  - 5.7.25-v30
  - 8.0.14-v30
  - 8.0.21-v24
  - 8.0.3-v30
  nats:
  - 2.6.1-v18
  - 2.8.2-v13
  percona-xtradb:
  - 5.7-v25
  postgres:
  - 10.14-v29
  - 11.9-v29
  - 12.4-v29
  - 13.1-v26
  - 14.0-v18
  - 15.1-v10
  - 9.6.19-v29
  redis:
  - 5.0.13-v18
  - 6.2.5-v18
  - 7.0.5-v11
  ui-server: v0.14.0
  vault:
  - 1.10.3-v10
  version: v2024.2.13
---

# Stash Redis Addon

Stash `{{< param "info.version" >}}` supports extending its functionality through addons. Stash Redis addon enables Stash to backup and restore Redis databases.

This guide will give you an overview of which Redis versions are supported and how the docs are organized.

## Supported Redis Versions

Stash has the following addon versions for Redis:

{{< versionlist "redis">}}

Here, the addon follows `M.M.P` versioning scheme where `M.M.P` (Major.Minor.Patch) represents the respective database version.

## Addon Version Compatibility

Any addon with matching major version with the database version should be able to take backup of that database. For example, Redis addon with version `6.x.x` should be able take backup of any Redis of `6.x.x` series. However, this might not be true for some versions. In that case, we will have separate addon for that version.

## Documentation Overview

Stash Redis documentations are organized as below:

- [How does it work?](/docs/v2024.2.13/addons/redis/overview/) gives an overview of how backup and restore process for Redis database works in Stash.
- [Helm managed Redis](/docs/v2024.2.13/addons/redis/helm/) shows how to backup and restore a Helm managed Redis database.
- [Auto-Backup](/docs/v2024.2.13/addons/redis/auto-backup/) shows how to configure a generic backup template for all the Redis databases of a cluster.
- [Customizing Backup & Restore Process](/docs/v2024.2.13/addons/redis/customization/) shows how to customize the backup & restore process.

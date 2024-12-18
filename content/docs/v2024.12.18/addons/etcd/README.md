---
title: Etcd Addon Overview | Stash
description: Etcd Addon Overview | Stash
menu:
  docs_v2024.12.18:
    identifier: stash-etcd-readme
    name: Readme
    parent: stash-etcd
    weight: -1
product_name: stash
menu_name: docs_v2024.12.18
section_menu_id: stash-addons
url: /docs/v2024.12.18/addons/etcd/
aliases:
- /docs/v2024.12.18/addons/etcd/README/
info:
  cli: v0.37.0
  community: v0.37.0
  elasticsearch:
  - 5.6.4-v33
  - 6.2.4-v33
  - 6.3.0-v33
  - 6.4.0-v33
  - 6.5.3-v33
  - 6.8.0-v33
  - 7.14.0-v19
  - 7.2.0-v33
  - 7.3.2-v33
  - 8.2.0-v16
  enterprise: v0.37.0
  etcd:
  - 3.5.0-v20
  installer: v2024.12.18
  kubedump:
  - 0.1.0-v16
  mariadb:
  - 10.5.8-v27
  mongodb:
  - 3.4.17-v34
  - 3.4.22-v34
  - 3.6.13-v34
  - 3.6.8-v34
  - 4.0.11-v34
  - 4.0.3-v34
  - 4.0.5-v34
  - 4.1.13-v34
  - 4.1.4-v34
  - 4.1.7-v34
  - 4.2.3-v34
  - 4.4.6-v25
  - 5.0.15-v7
  - 5.0.3-v22
  - 6.0.5-v10
  mysql:
  - 5.7.25-v34
  - 8.0.14-v33
  - 8.0.21-v27
  - 8.0.3-v33
  nats:
  - 2.6.1-v21
  - 2.8.2-v16
  perconaxtradb:
  - 5.7-v28
  postgres:
  - 10.14-v32
  - 11.9-v32
  - 12.4-v32
  - 13.1-v29
  - 14.0-v21
  - 15.1-v13
  - 16.1-v2
  - "17.2"
  - 9.6.19-v32
  redis:
  - 5.0.13-v21
  - 6.2.5-v21
  - 7.0.5-v14
  ui-server: v0.18.0
  vault:
  - 1.10.3-v13
  version: v2024.12.18
---

# Stash Etcd Addon

Stash `{{< param "info.version" >}}` supports extending its functionality through its addons. Stash Etcd addon enables Stash to backup and restore Etcd databases.

This guide will give you an overview of which Etcd versions are supported and how the docs are organized.

## Supported Etcd Versions

Stash has the following addon versions for Etcd:

{{< versionlist "etcd">}}

Here, the addon follows `M.M.P` versioning scheme where `M.M.P` (Major.Minor.Patch) represents the respective database version.

## Addon Version Compatibility

Any addon with matching major version with the database version should be able to take backup of that database. For example, Etcd addon with version `3.x.x` should be able to take backup of any Etcd of `3.x.x` series. However, this might not be true for some versions. In that case, we will have a separate addon for that version.

## Documentation Overview

Stash Etcd documentations are organized as below:

- [How does it work?](/docs/v2024.12.18/addons/etcd/overview/) gives an overview of how backup and restore process for Etcd database works in Stash.
- [Etcd Cluster with Basic Auth](/docs/v2024.12.18/addons/etcd/basic-auth/) shows how to backup and restore an Etcd cluster with basic authentication enabled.
- [Etcd Cluster with TLS](/docs/v2024.12.18/addons/etcd/tls/) shows how to configure backup and restore process for TLS secured Etcd cluster.
- [Customizing Backup & Restore Process](/docs/v2024.12.18/addons/etcd/customization/) shows how to customize the backup & restore process.

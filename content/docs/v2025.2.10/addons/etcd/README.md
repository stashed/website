---
title: Etcd Addon Overview | Stash
description: Etcd Addon Overview | Stash
menu:
  docs_v2025.2.10:
    identifier: stash-etcd-readme
    name: Readme
    parent: stash-etcd
    weight: -1
product_name: stash
menu_name: docs_v2025.2.10
section_menu_id: stash-addons
url: /docs/v2025.2.10/addons/etcd/
aliases:
- /docs/v2025.2.10/addons/etcd/README/
info:
  cli: v0.39.0
  community: v0.39.0
  elasticsearch:
  - 5.6.4-v35
  - 6.2.4-v35
  - 6.3.0-v35
  - 6.4.0-v35
  - 6.5.3-v35
  - 6.8.0-v35
  - 7.14.0-v21
  - 7.2.0-v35
  - 7.3.2-v35
  - 8.2.0-v18
  enterprise: v0.39.0
  etcd:
  - 3.5.0-v22
  installer: v2025.2.10
  kubedump:
  - 0.2.0-v3
  mariadb:
  - 10.5.8-v29
  mongodb:
  - 3.4.17-v36
  - 3.4.22-v36
  - 3.6.13-v36
  - 3.6.8-v36
  - 4.0.11-v36
  - 4.0.3-v36
  - 4.0.5-v36
  - 4.1.13-v36
  - 4.1.4-v36
  - 4.1.7-v36
  - 4.2.3-v36
  - 4.4.6-v27
  - 5.0.15-v9
  - 5.0.3-v24
  - 6.0.5-v12
  mysql:
  - 5.7.25-v36
  - 8.0.14-v35
  - 8.0.21-v29
  - 8.0.3-v35
  nats:
  - 2.6.1-v23
  - 2.8.2-v18
  percona-xtradb:
  - 5.7-v30
  postgres:
  - 10.14-v34
  - 11.9-v34
  - 12.4-v34
  - 13.1-v31
  - 14.0-v23
  - 15.1-v15
  - 16.1-v4
  - 17.2-v2
  - 9.6.19-v34
  redis:
  - 5.0.13-v23
  - 6.2.5-v23
  - 7.0.5-v16
  ui-server: v0.20.0
  vault:
  - 1.10.3-v15
  version: v2025.2.10
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

- [How does it work?](/docs/v2025.2.10/addons/etcd/overview/) gives an overview of how backup and restore process for Etcd database works in Stash.
- [Etcd Cluster with Basic Auth](/docs/v2025.2.10/addons/etcd/basic-auth/) shows how to backup and restore an Etcd cluster with basic authentication enabled.
- [Etcd Cluster with TLS](/docs/v2025.2.10/addons/etcd/tls/) shows how to configure backup and restore process for TLS secured Etcd cluster.
- [Customizing Backup & Restore Process](/docs/v2025.2.10/addons/etcd/customization/) shows how to customize the backup & restore process.

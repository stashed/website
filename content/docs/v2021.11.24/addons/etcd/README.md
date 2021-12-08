---
title: Etcd Addon Overview | Stash
description: Etcd Addon Overview | Stash
menu:
  docs_v2021.11.24:
    identifier: stash-etcd-readme
    name: Readme
    parent: stash-etcd
    weight: -1
product_name: stash
menu_name: docs_v2021.11.24
section_menu_id: stash-addons
url: /docs/v2021.11.24/addons/etcd/
aliases:
- /docs/v2021.11.24/addons/etcd/README/
info:
  cli: v0.17.0
  community: v0.17.0
  elasticsearch:
  - 5.6.4-v14
  - 6.2.4-v14
  - 6.3.0-v14
  - 6.4.0-v14
  - 6.5.3-v14
  - 6.8.0-v14
  - 7.14.0
  - 7.2.0-v14
  - 7.3.2-v14
  enterprise: v0.17.0
  etcd:
  - 3.5.0-v1
  installer: v2021.11.24
  mariadb:
  - 10.5.8-v7
  mongodb:
  - 3.4.17-v13
  - 3.4.22-v13
  - 3.6.13-v13
  - 3.6.8-v13
  - 4.0.11-v13
  - 4.0.3-v13
  - 4.0.5-v13
  - 4.1.13-v13
  - 4.1.4-v13
  - 4.1.7-v13
  - 4.2.3-v13
  - 4.4.6-v4
  - 5.0.3-v1
  mysql:
  - 5.7.25-v14
  - 8.0.14-v14
  - 8.0.21-v8
  - 8.0.3-v14
  nats:
  - 2.6.1-v1
  percona-xtradb:
  - 5.7-v9
  postgres:
  - 10.14-v12
  - 11.9-v12
  - 12.4-v12
  - 13.1-v9
  - 14.0-v1
  - 9.6.19-v12
  redis:
  - 5.0.13-v2
  - 6.2.5-v2
  version: v2021.11.24
---

{{< notice type="warning" message="This is an Enterprise-only feature. Please install [Stash Enterprise Edition](/docs/v2021.11.24/setup/install/enterprise) to try this feature." >}}

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

- [How does it work?](/docs/v2021.11.24/addons/etcd/overview/) gives an overview of how backup and restore process for Etcd database works in Stash.
- [Etcd Cluster with Basic Auth](/docs/v2021.11.24/addons/etcd/basic-auth/) shows how to backup and restore an Etcd cluster with basic authentication enabled.
- [Etcd Cluster with TLS](/docs/v2021.11.24/addons/etcd/tls/) shows how to configure backup and restore process for TLS secured Etcd cluster.
- [Customizing Backup & Restore Process](/docs/v2021.11.24/addons/etcd/customization/) shows how to customize the backup & restore process.

---
title: Etcd Addon Overview | Stash
description: Etcd Addon Overview | Stash
menu:
  docs_v2022.02.22:
    identifier: stash-etcd-readme
    name: Readme
    parent: stash-etcd
    weight: -1
product_name: stash
menu_name: docs_v2022.02.22
section_menu_id: stash-addons
url: /docs/v2022.02.22/addons/etcd/
aliases:
- /docs/v2022.02.22/addons/etcd/README/
info:
  cli: v0.18.0
  community: v0.18.0
  elasticsearch:
  - 5.6.4-v15
  - 6.2.4-v15
  - 6.3.0-v15
  - 6.4.0-v15
  - 6.5.3-v15
  - 6.8.0-v15
  - 7.14.0-v1
  - 7.2.0-v15
  - 7.3.2-v15
  enterprise: v0.18.0
  etcd:
  - 3.5.0-v2
  installer: v2022.02.22
  mariadb:
  - 10.5.8-v8
  mongodb:
  - 3.4.17-v14
  - 3.4.22-v14
  - 3.6.13-v14
  - 3.6.8-v14
  - 4.0.11-v14
  - 4.0.3-v14
  - 4.0.5-v14
  - 4.1.13-v14
  - 4.1.4-v14
  - 4.1.7-v14
  - 4.2.3-v14
  - 4.4.6-v5
  - 5.0.3-v2
  mysql:
  - 5.7.25-v15
  - 8.0.14-v15
  - 8.0.21-v9
  - 8.0.3-v15
  nats:
  - 2.6.1-v2
  percona-xtradb:
  - 5.7-v10
  postgres:
  - 10.14-v13
  - 11.9-v13
  - 12.4-v13
  - 13.1-v10
  - 14.0-v2
  - 9.6.19-v13
  redis:
  - 5.0.13-v3
  - 6.2.5-v3
  ui-server: v0.1.0
  version: v2022.02.22
---

{{< notice type="warning" message="This is an Enterprise-only feature. Please install [Stash Enterprise Edition](/docs/v2022.02.22/setup/install/enterprise) to try this feature." >}}

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

- [How does it work?](/docs/v2022.02.22/addons/etcd/overview/) gives an overview of how backup and restore process for Etcd database works in Stash.
- [Etcd Cluster with Basic Auth](/docs/v2022.02.22/addons/etcd/basic-auth/) shows how to backup and restore an Etcd cluster with basic authentication enabled.
- [Etcd Cluster with TLS](/docs/v2022.02.22/addons/etcd/tls/) shows how to configure backup and restore process for TLS secured Etcd cluster.
- [Customizing Backup & Restore Process](/docs/v2022.02.22/addons/etcd/customization/) shows how to customize the backup & restore process.

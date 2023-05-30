---
title: KubeDump Backup Addon Overview | Stash
description: KubeDump Backup Addon Overview | Stash
menu:
  docs_v2023.05.31:
    identifier: stash-kubedump-readme
    name: Readme
    parent: stash-kubedump
    weight: -1
product_name: stash
menu_name: docs_v2023.05.31
section_menu_id: stash-addons
url: /docs/v2023.05.31/addons/kubedump/
aliases:
- /docs/v2023.05.31/addons/kubedump/README/
info:
  cli: v0.30.0
  community: v0.30.0
  elasticsearch:
  - 5.6.4-v26
  - 6.2.4-v26
  - 6.3.0-v26
  - 6.4.0-v26
  - 6.5.3-v26
  - 6.8.0-v26
  - 7.14.0-v12
  - 7.2.0-v26
  - 7.3.2-v26
  - 8.2.0-v9
  enterprise: v0.30.0
  etcd:
  - 3.5.0-v13
  installer: v2023.05.31
  kubedump:
  - 0.1.0-v9
  mariadb:
  - 10.5.8-v19
  mongodb:
  - 3.4.17-v26
  - 3.4.22-v26
  - 3.6.13-v26
  - 3.6.8-v26
  - 4.0.11-v26
  - 4.0.3-v26
  - 4.0.5-v26
  - 4.1.13-v26
  - 4.1.4-v26
  - 4.1.7-v26
  - 4.2.3-v26
  - 4.4.6-v17
  - 5.0.3-v14
  - 6.0.5-v2
  mysql:
  - 5.7.25-v26
  - 8.0.14-v26
  - 8.0.21-v20
  - 8.0.3-v26
  nats:
  - 2.6.1-v14
  - 2.8.2-v9
  percona-xtradb:
  - 5.7-v21
  postgres:
  - 10.14-v25
  - 11.9-v25
  - 12.4-v25
  - 13.1-v22
  - 14.0-v14
  - 15.1-v6
  - 9.6.19-v25
  redis:
  - 5.0.13-v14
  - 6.2.5-v14
  - 7.0.5-v7
  ui-server: v0.12.0
  vault:
  - 1.10.3-v6
  version: v2023.05.31
---

{{< notice type="warning" message="This is an Enterprise-only feature. Please install [Stash Enterprise Edition](/docs/v2023.05.31/setup/install/enterprise/) to try this feature." >}}

# Stash KubeDump Backup Addon

Stash `{{< param "info.version" >}}` supports extending its functionality through addons. Stash KubeDump backup addon enables Stash to backup and restore Kubernetes manifests. You can backup the manifest of your entire cluster, a particular namespace, or a particular application.

This guide will give you an overview of how the docs are organized.

## Documentation Overview

Stash KubeDump backup documentations are organized as below:

- [How does it work?](/docs/v2023.05.31/addons/kubedump/overview/) gives an overview of how manifest backup works in Stash.
- [Cluster Backup](/docs/v2023.05.31/addons/kubedump/cluster/) shows how to backup all the cluster manifests.
- [Namespace Backup](/docs/v2023.05.31/addons/kubedump/namespace/) shows how to backup the manifests only for a particular namespace.
- [Application Backup](/docs/v2023.05.31/addons/kubedump/application/) shows how to manifests of a particular application.
- [Customizing Backup](/docs/v2023.05.31/addons/kubedump/customization/) shows how to customize the backup process.

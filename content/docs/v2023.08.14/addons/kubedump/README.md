---
title: KubeDump Backup Addon Overview | Stash
description: KubeDump Backup Addon Overview | Stash
menu:
  docs_v2023.08.14:
    identifier: stash-kubedump-readme
    name: Readme
    parent: stash-kubedump
    weight: -1
product_name: stash
menu_name: docs_v2023.08.14
section_menu_id: stash-addons
url: /docs/v2023.08.14/addons/kubedump/
aliases:
- /docs/v2023.08.14/addons/kubedump/README/
info:
  cli: v0.31.0
  community: v0.31.0
  elasticsearch:
  - 5.6.4-v27
  - 6.2.4-v27
  - 6.3.0-v27
  - 6.4.0-v27
  - 6.5.3-v27
  - 6.8.0-v27
  - 7.14.0-v13
  - 7.2.0-v27
  - 7.3.2-v27
  - 8.2.0-v10
  enterprise: v0.31.0
  etcd:
  - 3.5.0-v14
  installer: v2023.08.14
  kubedump:
  - 0.1.0-v10
  mariadb:
  - 10.5.8-v20
  mongodb:
  - 3.4.17-v27
  - 3.4.22-v27
  - 3.6.13-v27
  - 3.6.8-v27
  - 4.0.11-v27
  - 4.0.3-v27
  - 4.0.5-v27
  - 4.1.13-v27
  - 4.1.4-v27
  - 4.1.7-v27
  - 4.2.3-v27
  - 4.4.6-v18
  - 5.0.15
  - 5.0.3-v15
  - 6.0.5-v3
  mysql:
  - 5.7.25-v27
  - 8.0.14-v27
  - 8.0.21-v21
  - 8.0.3-v27
  nats:
  - 2.6.1-v15
  - 2.8.2-v10
  percona-xtradb:
  - 5.7-v22
  postgres:
  - 10.14-v26
  - 11.9-v26
  - 12.4-v26
  - 13.1-v23
  - 14.0-v15
  - 15.1-v7
  - 9.6.19-v26
  redis:
  - 5.0.13-v15
  - 6.2.5-v15
  - 7.0.5-v8
  ui-server: v0.13.0
  vault:
  - 1.10.3-v7
  version: v2023.08.14
---

{{< notice type="warning" message="This is an Enterprise-only feature. Please install [Stash Enterprise Edition](/docs/v2023.08.14/setup/install/enterprise/) to try this feature." >}}

# Stash KubeDump Backup Addon

Stash `{{< param "info.version" >}}` supports extending its functionality through addons. Stash KubeDump backup addon enables Stash to backup and restore Kubernetes manifests. You can backup the manifest of your entire cluster, a particular namespace, or a particular application.

This guide will give you an overview of how the docs are organized.

## Documentation Overview

Stash KubeDump backup documentations are organized as below:

- [How does it work?](/docs/v2023.08.14/addons/kubedump/overview/) gives an overview of how manifest backup works in Stash.
- [Cluster Backup](/docs/v2023.08.14/addons/kubedump/cluster/) shows how to backup all the cluster manifests.
- [Namespace Backup](/docs/v2023.08.14/addons/kubedump/namespace/) shows how to backup the manifests only for a particular namespace.
- [Application Backup](/docs/v2023.08.14/addons/kubedump/application/) shows how to manifests of a particular application.
- [Customizing Backup](/docs/v2023.08.14/addons/kubedump/customization/) shows how to customize the backup process.

---
title: KubeDump Backup Addon Overview | Stash
description: KubeDump Backup Addon Overview | Stash
menu:
  docs_v2022.05.12:
    identifier: stash-kubedump-readme
    name: Readme
    parent: stash-kubedump
    weight: -1
product_name: stash
menu_name: docs_v2022.05.12
section_menu_id: stash-addons
url: /docs/v2022.05.12/addons/kubedump/
aliases:
- /docs/v2022.05.12/addons/kubedump/README/
info:
  cli: v0.20.0
  community: v0.20.0
  elasticsearch:
  - 5.6.4-v17
  - 6.2.4-v17
  - 6.3.0-v17
  - 6.4.0-v17
  - 6.5.3-v17
  - 6.8.0-v17
  - 7.14.0-v3
  - 7.2.0-v17
  - 7.3.2-v17
  enterprise: v0.20.0
  etcd:
  - 3.5.0-v4
  installer: v2022.05.12
  kubedump:
  - 0.1.0
  mariadb:
  - 10.5.8-v10
  mongodb:
  - 3.4.17-v16
  - 3.4.22-v16
  - 3.6.13-v16
  - 3.6.8-v16
  - 4.0.11-v16
  - 4.0.3-v16
  - 4.0.5-v16
  - 4.1.13-v16
  - 4.1.4-v16
  - 4.1.7-v16
  - 4.2.3-v16
  - 4.4.6-v7
  - 5.0.3-v4
  mysql:
  - 5.7.25-v17
  - 8.0.14-v17
  - 8.0.21-v11
  - 8.0.3-v17
  nats:
  - 2.6.1-v4
  percona-xtradb:
  - 5.7-v12
  postgres:
  - 10.14-v15
  - 11.9-v15
  - 12.4-v15
  - 13.1-v12
  - 14.0-v4
  - 9.6.19-v15
  redis:
  - 5.0.13-v5
  - 6.2.5-v5
  ui-server: v0.2.0
  version: v2022.05.12
---

{{< notice type="warning" message="This is an Enterprise-only feature. Please install [Stash Enterprise Edition](/docs/v2022.05.12/setup/install/enterprise) to try this feature." >}}

# Stash KubeDump Backup Addon

Stash `{{< param "info.version" >}}` supports extending its functionality through addons. Stash KubeDump backup addon enables Stash to backup and restore Kubernetes manifests. You can backup the manifest of your entire cluster, a particular namespace, or a particular application.

This guide will give you an overview of how the docs are organized.

## Documentation Overview

Stash KubeDump backup documentations are organized as below:

- [How does it work?](/docs/v2022.05.12/addons/kubedump/overview/) gives an overview of how manifest backup works in Stash.
- [Cluster Backup](/docs/v2022.05.12/addons/kubedump/cluster/) shows how to backup all the cluster manifests.
- [Namespace Backup](/docs/v2022.05.12/addons/kubedump/namespace/) shows how to backup the manifests only for a particular namespace.
- [Application Backup](/docs/v2022.05.12/addons/kubedump/application/) shows how to manifests of a particular application.
- [Customizing Backup](/docs/v2022.05.12/addons/kubedump/customization/) shows how to customize the backup process.

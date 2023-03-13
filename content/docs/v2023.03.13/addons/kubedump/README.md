---
title: KubeDump Backup Addon Overview | Stash
description: KubeDump Backup Addon Overview | Stash
menu:
  docs_v2023.03.13:
    identifier: stash-kubedump-readme
    name: Readme
    parent: stash-kubedump
    weight: -1
product_name: stash
menu_name: docs_v2023.03.13
section_menu_id: stash-addons
url: /docs/v2023.03.13/addons/kubedump/
aliases:
- /docs/v2023.03.13/addons/kubedump/README/
info:
  cli: v0.27.0
  community: v0.27.0
  elasticsearch:
  - 5.6.4-v23
  - 6.2.4-v23
  - 6.3.0-v23
  - 6.4.0-v23
  - 6.5.3-v23
  - 6.8.0-v23
  - 7.14.0-v9
  - 7.2.0-v23
  - 7.3.2-v23
  - 8.2.0-v6
  enterprise: v0.27.0
  etcd:
  - 3.5.0-v10
  installer: v2023.03.13
  kubedump:
  - 0.1.0-v6
  mariadb:
  - 10.5.8-v16
  mongodb:
  - 3.4.17-v23
  - 3.4.22-v23
  - 3.6.13-v23
  - 3.6.8-v23
  - 4.0.11-v23
  - 4.0.3-v23
  - 4.0.5-v23
  - 4.1.13-v23
  - 4.1.4-v23
  - 4.1.7-v23
  - 4.2.3-v23
  - 4.4.6-v14
  - 5.0.3-v11
  mysql:
  - 5.7.25-v23
  - 8.0.14-v23
  - 8.0.21-v17
  - 8.0.3-v23
  nats:
  - 2.6.1-v11
  - 2.8.2-v6
  percona-xtradb:
  - 5.7-v18
  postgres:
  - 10.14-v22
  - 11.9-v22
  - 12.4-v22
  - 13.1-v19
  - 14.0-v11
  - 15.1-v3
  - 9.6.19-v22
  redis:
  - 5.0.13-v11
  - 6.2.5-v11
  - 7.0.5-v4
  ui-server: v0.9.0
  vault:
  - 1.10.3-v3
  version: v2023.03.13
---

{{< notice type="warning" message="This is an Enterprise-only feature. Please install [Stash Enterprise Edition](/docs/v2023.03.13/setup/install/enterprise/) to try this feature." >}}

# Stash KubeDump Backup Addon

Stash `{{< param "info.version" >}}` supports extending its functionality through addons. Stash KubeDump backup addon enables Stash to backup and restore Kubernetes manifests. You can backup the manifest of your entire cluster, a particular namespace, or a particular application.

This guide will give you an overview of how the docs are organized.

## Documentation Overview

Stash KubeDump backup documentations are organized as below:

- [How does it work?](/docs/v2023.03.13/addons/kubedump/overview/) gives an overview of how manifest backup works in Stash.
- [Cluster Backup](/docs/v2023.03.13/addons/kubedump/cluster/) shows how to backup all the cluster manifests.
- [Namespace Backup](/docs/v2023.03.13/addons/kubedump/namespace/) shows how to backup the manifests only for a particular namespace.
- [Application Backup](/docs/v2023.03.13/addons/kubedump/application/) shows how to manifests of a particular application.
- [Customizing Backup](/docs/v2023.03.13/addons/kubedump/customization/) shows how to customize the backup process.

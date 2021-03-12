---
title: Table of Contents | Guides
description: Table of Contents | Guides
menu:
  docs_v2021.03.11:
    identifier: latest-guides-readme
    name: Readme
    parent: latest-guides
    weight: -1
product_name: stash
menu_name: docs_v2021.03.11
section_menu_id: guides
url: /docs/v2021.03.11/guides/latest/
aliases:
- /docs/v2021.03.11/guides/latest/README/
info:
  catalog: v2021.03.11
  cli: v0.11.11
  community: v0.11.11
  elasticsearch:
  - 5.6.4-v7
  - 6.2.4-v7
  - 6.3.0-v7
  - 6.4.0-v7
  - 6.5.3-v7
  - 6.8.0-v7
  - 7.2.0-v7
  - 7.3.2-v7
  enterprise: v0.11.11
  installer: v0.11.11
  mariadb:
  - 10.5.8-v1
  mongodb:
  - 3.4.17-v6
  - 3.4.22-v6
  - 3.6.13-v6
  - 3.6.8-v6
  - 4.0.11-v6
  - 4.0.3-v6
  - 4.0.5-v6
  - 4.1.13-v6
  - 4.1.4-v6
  - 4.1.7-v6
  - 4.2.3-v6
  mysql:
  - 5.7.25-v7
  - 8.0.14-v7
  - 8.0.21-v1
  - 8.0.3-v7
  percona-xtradb:
  - 5.7.0-v2
  postgres:
  - 10.14.0-v5
  - 11.9.0-v5
  - 12.4.0-v5
  - 13.1.0-v2
  - 9.6.19-v5
  version: v2021.03.11
---

# Guides

Guides show how to perform different operations with Stash. We have divided guides section into the following sub-sections:

- **Supported Backends :** This section describes how to configure different backend for Stash. It shows how to create `Storage Secret` and `Repository` object for different backends. Start learning to configure different backends from [here](/docs/v2021.03.11/guides/latest/backends/overview).

- **Workloads :** Workloads section describes how to backup and restore data from/to inside various Kubernetes resources such as Deployment, StatefulSet, DaemonSet, ReplicaSet etc. Get started with backup workloads data using Stash from [here](/docs/v2021.03.11/guides/latest/workloads/overview).

- **Volumes :** Volumes section describes how to backup and restore stand-alone Kubernetes volumes using Stash. Learn how to backup Kubernetes volumes using Stash from [here](/docs/v2021.03.11/guides/latest/volumes/overview).

- **Databases :** Databases section describes how to backup and restore databases using Stash. Check how database backup works in Stash from [here](/docs/v2021.03.11/guides/latest/addons/overview).

- **Auto Backup :** This section describes how you can write blueprint for backup process to backup resources using some annotations. Start learning to write backup blueprint from [here](/docs/v2021.03.11/guides/latest/auto-backup/overview).

- **Advanced Use Cases :** This section describes some advance uses of Stash such as instant backup, restoring in different namespace/cluster, restoring in different host etc. See how to trigger an instant backup from [here](/docs/v2021.03.11/guides/latest/advanced-use-case/instant-backup).

- **Monitoring :** Monitoring section describes how to monitor backup and restore process using Prometheus.

- **Stash CLI :** Stash provides a CLI to perform various operations such as unlocking a repository, restoring backed up data locally, triggering backup instantly etc. This section describes how you can perform such operations.

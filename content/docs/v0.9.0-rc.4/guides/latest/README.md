---
title: Table of Contents | Guides
description: Table of Contents | Guides
menu:
  docs_v0.9.0-rc.4:
    identifier: latest-guides-readme
    name: Readme
    parent: latest-guides
    weight: -1
product_name: stash
menu_name: docs_v0.9.0-rc.4
section_menu_id: guides
url: /docs/v0.9.0-rc.4/guides/latest/
aliases:
- /docs/v0.9.0-rc.4/guides/latest/README/
info:
  catalog: v0.2.0
  cli: v0.3.0
  version: v0.9.0-rc.4
---

# Guides

Guides show how to perform different operations with Stash. We have divided guides section into the following sub-sections:

- **Supported Backends :** This section describes how to configure different backend for Stash. It shows how to create `Storage Secret` and `Repository` object for different backends. Start learning to configure different backends from [here](/docs/v0.9.0-rc.4/guides/latest/backends/overview).

- **Workloads :** Workloads section describes how to backup and restore data from/to inside various Kubernetes resources such as Deployment, StatefulSet, DaemonSet, ReplicaSet etc. Get started with backup workloads data using Stash from [here](/docs/v0.9.0-rc.4/guides/latest/workloads/overview).

- **Volumes :** Volumes section describes how to backup and restore stand-alone Kubernetes volumes using Stash. Learn how to backup Kubernetes volumes using Stash from [here](/docs/v0.9.0-rc.4/guides/latest/volumes/overview).

- **Databases :** Databases section describes how to backup and restore databases using Stash. Check how database backup works in Stash from [here](/docs/v0.9.0-rc.4/guides/latest/addons/overview).

- **Auto Backup :** This section describes how you can write blueprint for backup process to backup resources using some annotations. Start learning to write backup blueprint from [here](/docs/v0.9.0-rc.4/guides/latest/auto-backup/overview).

- **Advanced Use Cases :** This section describes some advance uses of Stash such as instant backup, restoring in different namespace/cluster, restoring in different host etc. See how to trigger an instant backup from [here](/docs/v0.9.0-rc.4/guides/latest/advanced-use-case/instant-backup).

- **Monitoring :** Monitoring section describes how to monitor backup and restore process using Prometheus.

- **Stash CLI :** Stash provides a CLI to perform various operations such as unlocking a repository, restoring backed up data locally, triggering backup instantly etc. This section describes how you can perform such operations.

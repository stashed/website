---
title: Table of Contents | Guides
description: Table of Contents | Guides
menu:
  docs_v2023.10.9:
    identifier: guides-readme
    name: Readme
    parent: guides
    weight: -1
product_name: stash
menu_name: docs_v2023.10.9
section_menu_id: guides
url: /docs/v2023.10.9/guides/
aliases:
- /docs/v2023.10.9/guides/README/
info:
  cli: v0.32.0
  community: v0.32.0
  elasticsearch:
  - 5.6.4-v28
  - 6.2.4-v28
  - 6.3.0-v28
  - 6.4.0-v28
  - 6.5.3-v28
  - 6.8.0-v28
  - 7.14.0-v14
  - 7.2.0-v28
  - 7.3.2-v28
  - 8.2.0-v11
  enterprise: v0.32.0
  etcd:
  - 3.5.0-v15
  installer: v2023.10.9
  kubedump:
  - 0.1.0-v11
  mariadb:
  - 10.5.8-v21
  mongodb:
  - 3.4.17-v28
  - 3.4.22-v28
  - 3.6.13-v28
  - 3.6.8-v28
  - 4.0.11-v28
  - 4.0.3-v28
  - 4.0.5-v28
  - 4.1.13-v28
  - 4.1.4-v28
  - 4.1.7-v28
  - 4.2.3-v28
  - 4.4.6-v19
  - 5.0.15-v1
  - 5.0.3-v16
  - 6.0.5-v4
  mysql:
  - 5.7.25-v28
  - 8.0.14-v28
  - 8.0.21-v22
  - 8.0.3-v28
  nats:
  - 2.6.1-v16
  - 2.8.2-v11
  percona-xtradb:
  - 5.7-v23
  postgres:
  - 10.14-v27
  - 11.9-v27
  - 12.4-v27
  - 13.1-v24
  - 14.0-v16
  - 15.1-v8
  - 9.6.19-v27
  redis:
  - 5.0.13-v16
  - 6.2.5-v16
  - 7.0.5-v9
  ui-server: v0.13.0
  vault:
  - 1.10.3-v8
  version: v2023.10.9
---

# Guides

Guides show how to perform different operations with Stash. We have divided guides section into the following sub-sections:

- [Supported Backends](/docs/v2023.10.9/guides/backends/overview/): Describes how to configure different storage for storing backed up data.
- [Workload Volume Backup](/docs/v2023.10.9/guides/workloads/overview/): Shows how to use Stash to backup and restore volumes of a workload (i.e. `Deployment`, `StatefulSet`, `DaemonSet`, etc).
- [Stand-alone Volume Backup](/docs/v2023.10.9/guides/volumes/overview/): Shows how to use Stash to backup and restore stand-alone volumes(i.e. `PersistentVolumeClaim`).
- [Batch Backup](/docs/v2023.10.9/guides/batch-backup/overview/): Shows how to backup multiple co-related components using a single configuration known as `BackupBatch`.
- [Auto Backup](/docs/v2023.10.9/guides/auto-backup/overview/): Shows how to configure automatic backup of any stateful workload in your cluster.
- [Volume Snapshot](/docs/v2023.10.9/guides/volumesnapshot/overview/): Shows how Stash takes snapshot of `PersistentVolumeClaim`s and restore them from snapshot using Kubernetes `VolumeSnapshot` API.

- **Different Use Cases:**
Shows different uses cases of Stash like instant backup, pause backup, cross-namespace backup and restore etc.

  - [Instant Backup](/docs/v2023.10.9/guides/use-cases/instant-backup/): Shows you how to take an instant backup in Stash.
  - [Exclude/Include Files](/docs/v2023.10.9/guides/use-cases/exclude-include-files/): Shows how to exclude/include subset of files during backup/restore process.
  - [Pause Backup](/docs/v2023.10.9/guides/use-cases/pause-backup/): Shows how to pause a backup temporarily.
  - [Clone Data Volumes](/docs/v2023.10.9/guides/use-cases/clone-pvc/): Shows how to clone data volumes of a workload into a different namespace in a cluster using Stash.
  - [File Ownership](/docs/v2023.10.9/guides/use-cases/ownership/): Explains when and how ownership of the restored files can be changed.
  - [Cross-Cluster Backup and Restore](/docs/v2023.10.9/guides/use-cases/cross-cluster-backup/): Shows how to take backup and restore across clusters using Stash.
  - [Customize Backup and Restore](/docs/v2023.10.9/guides/use-cases/customize-backup-restore/): Shows how to customize backup and restore processes in Stash according to your needs.

- **Managed Backup**
Shows how backup and restore can be managed in some specific scenerios.
  - [Dedicated Backup Namespace](/docs/v2023.10.9/guides/managed-backup/dedicated-backup-namespace/): Shows you how to manage backup and restore from a dedicated namespace for targets of different namespaces using Stash.
  - [Dedicated Storage Namespace](/docs/v2023.10.9/guides/managed-backup/dedicated-storage-namespace/): Shows you how to take backup and restore by keeping the storage resources (Repository and backend Secret) in a dedicated namespace using Stash.

- [Platforms](/docs/v2023.10.9/guides/platforms/eks-irsa/): Shows how to use Stash to backup and restore volumes of a Kubernetes workload running in different platforms.
- [Monitoring](/docs/v2023.10.9/guides/monitoring/overview/): Shows how Prometheus monitoring works with Stash, what metrics Stash exports, and how to enable monitoring.
- [Hooks](/docs/v2023.10.9/guides/hooks/overview/): Shows how to execute different actions before/after the backup/restore process.
- [CLI](/docs/v2023.10.9/guides/cli/kubectl-plugin/): Shows how to manage Stash objects quickly and easily using Stash `kubectl` plugin.
- [Troubleshooting](/docs/v2023.10.9/guides/troubleshooting/how-to-troubleshoot/): Gives an overview of how you can gather the necessary information to identify the issue that causes the backup/restore failure.
- [Security](/docs/v2023.10.9/guides/security/rbac/): Describes different built-in cluster security support by Stash.

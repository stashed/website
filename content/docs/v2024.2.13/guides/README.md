---
title: Table of Contents | Guides
description: Table of Contents | Guides
menu:
  docs_v2024.2.13:
    identifier: guides-readme
    name: Readme
    parent: guides
    weight: -1
product_name: stash
menu_name: docs_v2024.2.13
section_menu_id: guides
url: /docs/v2024.2.13/guides/
aliases:
- /docs/v2024.2.13/guides/README/
info:
  cli: v0.33.0
  community: v0.33.0
  elasticsearch:
  - 5.6.4-v30
  - 6.2.4-v30
  - 6.3.0-v30
  - 6.4.0-v30
  - 6.5.3-v30
  - 6.8.0-v30
  - 7.14.0-v16
  - 7.2.0-v30
  - 7.3.2-v30
  - 8.2.0-v13
  enterprise: v0.33.0
  etcd:
  - 3.5.0-v17
  installer: v2024.2.13
  kubedump:
  - 0.1.0-v13
  mariadb:
  - 10.5.8-v24
  mongodb:
  - 3.4.17-v30
  - 3.4.22-v30
  - 3.6.13-v30
  - 3.6.8-v30
  - 4.0.11-v30
  - 4.0.3-v30
  - 4.0.5-v30
  - 4.1.13-v30
  - 4.1.4-v30
  - 4.1.7-v30
  - 4.2.3-v30
  - 4.4.6-v21
  - 5.0.15-v3
  - 5.0.3-v18
  - 6.0.5-v6
  mysql:
  - 5.7.25-v30
  - 8.0.14-v30
  - 8.0.21-v24
  - 8.0.3-v30
  nats:
  - 2.6.1-v18
  - 2.8.2-v13
  percona-xtradb:
  - 5.7-v25
  postgres:
  - 10.14-v29
  - 11.9-v29
  - 12.4-v29
  - 13.1-v26
  - 14.0-v18
  - 15.1-v10
  - 9.6.19-v29
  redis:
  - 5.0.13-v18
  - 6.2.5-v18
  - 7.0.5-v11
  ui-server: v0.14.0
  vault:
  - 1.10.3-v10
  version: v2024.2.13
---

# Guides

Guides show how to perform different operations with Stash. We have divided guides section into the following sub-sections:

- [Supported Backends](/docs/v2024.2.13/guides/backends/overview/): Describes how to configure different storage for storing backed up data.
- [Workload Volume Backup](/docs/v2024.2.13/guides/workloads/overview/): Shows how to use Stash to backup and restore volumes of a workload (i.e. `Deployment`, `StatefulSet`, `DaemonSet`, etc).
- [Stand-alone Volume Backup](/docs/v2024.2.13/guides/volumes/overview/): Shows how to use Stash to backup and restore stand-alone volumes(i.e. `PersistentVolumeClaim`).
- [Batch Backup](/docs/v2024.2.13/guides/batch-backup/overview/): Shows how to backup multiple co-related components using a single configuration known as `BackupBatch`.
- [Auto Backup](/docs/v2024.2.13/guides/auto-backup/overview/): Shows how to configure automatic backup of any stateful workload in your cluster.
- [Volume Snapshot](/docs/v2024.2.13/guides/volumesnapshot/overview/): Shows how Stash takes snapshot of `PersistentVolumeClaim`s and restore them from snapshot using Kubernetes `VolumeSnapshot` API.

- **Different Use Cases:**
Shows different uses cases of Stash like instant backup, pause backup, cross-namespace backup and restore etc.

  - [Instant Backup](/docs/v2024.2.13/guides/use-cases/instant-backup/): Shows you how to take an instant backup in Stash.
  - [Exclude/Include Files](/docs/v2024.2.13/guides/use-cases/exclude-include-files/): Shows how to exclude/include subset of files during backup/restore process.
  - [Pause Backup](/docs/v2024.2.13/guides/use-cases/pause-backup/): Shows how to pause a backup temporarily.
  - [Clone Data Volumes](/docs/v2024.2.13/guides/use-cases/clone-pvc/): Shows how to clone data volumes of a workload into a different namespace in a cluster using Stash.
  - [File Ownership](/docs/v2024.2.13/guides/use-cases/ownership/): Explains when and how ownership of the restored files can be changed.
  - [Cross-Cluster Backup and Restore](/docs/v2024.2.13/guides/use-cases/cross-cluster-backup/): Shows how to take backup and restore across clusters using Stash.
  - [Customize Backup and Restore](/docs/v2024.2.13/guides/use-cases/customize-backup-restore/): Shows how to customize backup and restore processes in Stash according to your needs.

- **Managed Backup**
Shows how backup and restore can be managed in some specific scenerios.
  - [Dedicated Backup Namespace](/docs/v2024.2.13/guides/managed-backup/dedicated-backup-namespace/): Shows you how to manage backup and restore from a dedicated namespace for targets of different namespaces using Stash.
  - [Dedicated Storage Namespace](/docs/v2024.2.13/guides/managed-backup/dedicated-storage-namespace/): Shows you how to take backup and restore by keeping the storage resources (Repository and backend Secret) in a dedicated namespace using Stash.

- [Platforms](/docs/v2024.2.13/guides/platforms/eks-irsa/): Shows how to use Stash to backup and restore volumes of a Kubernetes workload running in different platforms.
- [Monitoring](/docs/v2024.2.13/guides/monitoring/overview/): Shows how Prometheus monitoring works with Stash, what metrics Stash exports, and how to enable monitoring.
- [Hooks](/docs/v2024.2.13/guides/hooks/overview/): Shows how to execute different actions before/after the backup/restore process.
- [CLI](/docs/v2024.2.13/guides/cli/kubectl-plugin/): Shows how to manage Stash objects quickly and easily using Stash `kubectl` plugin.
- [Troubleshooting](/docs/v2024.2.13/guides/troubleshooting/how-to-troubleshoot/): Gives an overview of how you can gather the necessary information to identify the issue that causes the backup/restore failure.
- [Security](/docs/v2024.2.13/guides/security/rbac/): Describes different built-in cluster security support by Stash.

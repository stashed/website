---
title: Welcome | Stash
description: Welcome to Stash
menu:
  docs_v2025.7.31:
    identifier: readme-stash
    name: Readme
    parent: welcome
    weight: -1
product_name: stash
menu_name: docs_v2025.7.31
section_menu_id: welcome
url: /docs/v2025.7.31/welcome/
aliases:
- /docs/v2025.7.31/
- /docs/v2025.7.31/README/
info:
  cli: v0.41.0
  community: v0.41.0
  elasticsearch:
  - 5.6.4-v37
  - 6.2.4-v37
  - 6.3.0-v37
  - 6.4.0-v37
  - 6.5.3-v37
  - 6.8.0-v37
  - 7.14.0-v23
  - 7.2.0-v37
  - 7.3.2-v37
  - 8.2.0-v20
  enterprise: v0.41.0
  etcd:
  - 3.5.0-v24
  installer: v2025.7.31
  kubedump:
  - 0.2.0-v5
  mariadb:
  - 10.5.8-v31
  mongodb:
  - 3.4.17-v38
  - 3.4.22-v38
  - 3.6.13-v38
  - 3.6.8-v38
  - 4.0.11-v38
  - 4.0.3-v38
  - 4.0.5-v38
  - 4.1.13-v38
  - 4.1.4-v38
  - 4.1.7-v38
  - 4.2.3-v38
  - 4.4.6-v29
  - 5.0.15-v11
  - 5.0.3-v26
  - 6.0.5-v14
  mysql:
  - 5.7.25-v38
  - 8.0.14-v37
  - 8.0.21-v31
  - 8.0.3-v37
  nats:
  - 2.6.1-v25
  - 2.8.2-v20
  percona-xtradb:
  - 5.7-v32
  postgres:
  - 10.14-v36
  - 11.9-v36
  - 12.4-v36
  - 13.1-v33
  - 14.0-v25
  - 15.1-v17
  - 16.1-v6
  - 17.2-v4
  - 9.6.19-v36
  redis:
  - 5.0.13-v25
  - 6.2.5-v25
  - 7.0.5-v18
  ui-server: v0.22.0
  vault:
  - 1.10.3-v17
  version: v2025.7.31
---

# Stash

[Stash](https://stash.run) by AppsCode is a cloud native data backup and recovery solution for Kubernetes workloads. If you are running production workloads in Kubernetes, you might want to take backup of your disks, databases etc. Traditional tools are too complex to set up and maintain in a dynamic compute environment like Kubernetes. Stash is a Kubernetes operator that uses [restic](https://github.com/restic/restic) or Kubernetes CSI Driver VolumeSnapshotter functionality to address these issues. Using Stash, you can backup Kubernetes volumes mounted in workloads, stand-alone volumes and databases. Users may even extend Stash via [addons](https://stash.run/docs/{{< param "info.version" >}}/guides/addons/overview/) for any custom workload.

Here, we are going to give you an overview of Stash documentation structure.

## [Concepts](/docs/v2025.7.31/concepts/)

Concept explains some significant aspect of Stash. This is where you can learn about what Stash does and how it does it.

- [Stash Overview](/docs/v2025.7.31/concepts/what-is-stash/overview/) Provides an introduction to Stash and gives an overview of the features it provides.
- [Stash Architecture](/docs/v2025.7.31/concepts/what-is-stash/architecture/) Provides an overview of Stash architecture and it's core components.
- [Stash API](/docs/v2025.7.31/concepts/crds/repository/) Introduces Stash CRDs.

## [Setup](/docs/v2025.7.31/setup/)

Setup contains instruction for installing, uninstalling, and upgrading Stash.

- **Install Stash:** Provides installation instructions for Stash and its various components.
  - [Stash](/docs/v2025.7.31/setup/install/stash/): Provides installation instructions for Stash.
  - [Stash kubectl Plugin](/docs/v2025.7.31/setup/install/kubectl-plugin/): Provides installation instructions for Stash `kubectl` plugin.
  - [Troubleshooting](/docs/v2025.7.31/setup/install/troubleshooting/): Provides troubleshooting guide for various installation problems.
- **Uninstall Stash:** Provides uninstallation instructions for Stash and its various components.
  - [Stash](/docs/v2025.7.31/setup/uninstall/stash/): Provides uninstallation instructions for Stash.
  - [Stash kubectl Plugin](/docs/v2025.7.31/setup/uninstall/kubectl-plugin/): Provides uninstallation instructions for Stash `kubectl` plugin.
- [Upgrade Stash](/docs/v2025.7.31/setup/upgrade/): Provides instruction for updating Stash license and upgrading between various Stash versions.

## [Guides](/docs/v2025.7.31/guides/)

Guides show how to perform different operations with Stash.

- [Supported Backends](/docs/v2025.7.31/guides/backends/overview/): Describes how to configure different storage for storing backed up data.
- [Workload Volume Backup](/docs/v2025.7.31/guides/workloads/overview/): Shows how to use Stash to backup and restore volumes of a workload (i.e. `Deployment`, `StatefulSet`, `DaemonSet`, etc).
- [Stand-alone Volume Backup](/docs/v2025.7.31/guides/volumes/overview/): Shows how to use Stash to backup and restore stand-alone volumes(i.e. `PersistentVolumeClaim`).
- [Batch Backup](/docs/v2025.7.31/guides/batch-backup/overview/): Shows how to backup multiple co-related components using a single configuration known as `BackupBatch`.
- [Auto Backup](/docs/v2025.7.31/guides/auto-backup/overview/): Shows how to configure automatic backup of any stateful workload in your cluster.
- [Volume Snapshot](/docs/v2025.7.31/guides/volumesnapshot/overview/): Shows how Stash takes snapshot of `PersistentVolumeClaim`s and restore them from snapshot using Kubernetes `VolumeSnapshot` API.

- **Different Use Cases:**
Shows different uses cases of Stash like instant backup, pause backup, cross-namespace backup and restore etc.

  - [Instant Backup](/docs/v2025.7.31/guides/use-cases/instant-backup/): Shows you how to take an instant backup in Stash.
  - [Exclude/Include Files](/docs/v2025.7.31/guides/use-cases/exclude-include-files/): Shows how to exclude/include subset of files during backup/restore process.
  - [Pause Backup](/docs/v2025.7.31/guides/use-cases/pause-backup/): Shows how to pause a backup temporarily.
  - [Clone Data Volumes](/docs/v2025.7.31/guides/use-cases/clone-pvc/): Shows how to clone data volumes of a workload into a different namespace in a cluster using Stash.
  - [File Ownership](/docs/v2025.7.31/guides/use-cases/ownership/): Explains when and how ownership of the restored files can be changed.
  - [Cross-Cluster Backup and Restore](/docs/v2025.7.31/guides/use-cases/cross-cluster-backup/): Shows how to take backup and restore across clusters using Stash.
  - [Customize Backup and Restore](/docs/v2025.7.31/guides/use-cases/customize-backup-restore/): Shows how to customize backup and restore processes in Stash according to your needs.

- **Managed Backup**
Shows how backup and restore can be managed in some specific scenerios.
  - [Dedicated Backup Namespace](/docs/v2025.7.31/guides/managed-backup/dedicated-backup-namespace/): Shows you how to manage backup and restore from a dedicated namespace for targets of different namespaces using Stash.
  - [Dedicated Storage Namespace](/docs/v2025.7.31/guides/managed-backup/dedicated-storage-namespace/): Shows you how to take backup and restore by keeping the storage resources (Repository and backend Secret) in a dedicated namespace using Stash.

- [Platforms](/docs/v2025.7.31/guides/platforms/eks-irsa/): Shows how to use Stash to backup and restore volumes of a Kubernetes workload running in different platforms.
- [Monitoring](/docs/v2025.7.31/guides/monitoring/overview/): Shows how Prometheus monitoring works with Stash, what metrics Stash exports, and how to enable monitoring.
- [Hooks](/docs/v2025.7.31/guides/hooks/overview/): Shows how to execute different actions before/after the backup/restore process.
- [CLI](/docs/v2025.7.31/guides/cli/kubectl-plugin/): Shows how to manage Stash objects quickly and easily using Stash `kubectl` plugin.
- [Troubleshooting](/docs/v2025.7.31/guides/troubleshooting/how-to-troubleshoot/): Gives an overview of how you can gather the necessary information to identify the issue that causes the backup/restore failure.
- [Security](/docs/v2025.7.31/guides/security/rbac/): Describes different built-in cluster security support by Stash.

We're always looking for help improving our documentation, so please don't hesitate to [file an issue](https://github.com/stashed/project/issues/new) if you see some problem.

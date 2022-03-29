---
title: Welcome | Stash
description: Welcome to Stash
menu:
  docs_v2022.03.29:
    identifier: readme-stash
    name: Readme
    parent: welcome
    weight: -1
product_name: stash
menu_name: docs_v2022.03.29
section_menu_id: welcome
url: /docs/v2022.03.29/welcome/
aliases:
- /docs/v2022.03.29/
- /docs/v2022.03.29/README/
info:
  cli: v0.19.0
  community: v0.19.0
  elasticsearch:
  - 5.6.4-v16
  - 6.2.4-v16
  - 6.3.0-v16
  - 6.4.0-v16
  - 6.5.3-v16
  - 6.8.0-v16
  - 7.14.0-v2
  - 7.2.0-v16
  - 7.3.2-v16
  enterprise: v0.19.0
  etcd:
  - 3.5.0-v3
  installer: v2022.03.29
  mariadb:
  - 10.5.8-v9
  mongodb:
  - 3.4.17-v15
  - 3.4.22-v15
  - 3.6.13-v15
  - 3.6.8-v15
  - 4.0.11-v15
  - 4.0.3-v15
  - 4.0.5-v15
  - 4.1.13-v15
  - 4.1.4-v15
  - 4.1.7-v15
  - 4.2.3-v15
  - 4.4.6-v6
  - 5.0.3-v3
  mysql:
  - 5.7.25-v16
  - 8.0.14-v16
  - 8.0.21-v10
  - 8.0.3-v16
  nats:
  - 2.6.1-v3
  percona-xtradb:
  - 5.7-v11
  postgres:
  - 10.14-v14
  - 11.9-v14
  - 12.4-v14
  - 13.1-v11
  - 14.0-v3
  - 9.6.19-v14
  redis:
  - 5.0.13-v4
  - 6.2.5-v4
  ui-server: v0.2.0
  version: v2022.03.29
---

# Stash

[Stash](https://stash.run) by AppsCode is a cloud native data backup and recovery solution for Kubernetes workloads. If you are running production workloads in Kubernetes, you might want to take backup of your disks, databases etc. Traditional tools are too complex to set up and maintain in a dynamic compute environment like Kubernetes. Stash is a Kubernetes operator that uses [restic](https://github.com/restic/restic) or Kubernetes CSI Driver VolumeSnapshotter functionality to address these issues. Using Stash, you can backup Kubernetes volumes mounted in workloads, stand-alone volumes and databases. Users may even extend Stash via [addons](https://stash.run/docs/{{< param "info.version" >}}/guides/addons/overview/) for any custom workload.

Here, we are going to give you an overview of Stash documentation structure.

## [Concepts](/docs/v2022.03.29/concepts/)

Concept explains some significant aspect of Stash. This is where you can learn about what Stash does and how it does it.

- [Stash Overview](/docs/v2022.03.29/concepts/what-is-stash/overview) Provides an introduction to Stash and gives an overview of the features it provides.
- [Stash Architecture](/docs/v2022.03.29/concepts/what-is-stash/architecture) Provides an overview of Stash architecture and it's core components.
- [Stash API](/docs/v2022.03.29/concepts/crds/repository) Introduces Stash CRDs.

## [Setup](/docs/v2022.03.29/setup/)

Setup contains instruction for installing, uninstalling, and upgrading Stash.

- **Install Stash:** Provides installation instructions for Stash and its various components.
  - [Community Edition](/docs/v2022.03.29/setup/install/community): Provides installation instructions for Stash Community Edition.
  - [Enterprise Edition](/docs/v2022.03.29/setup/install/enterprise): Provides installation instructions for Stash Enterprise Edition.
  - [Stash kubectl Plugin](/docs/v2022.03.29/setup/install/kubectl_plugin): Provides installation instructions for Stash `kubectl` plugin.
  - [Troubleshooting](/docs/v2022.03.29/setup/install/troubleshoting): Provides troubleshooting guide for various installation problems.
- **Uninstall Stash:** Provides uninstallation instructions for Stash and its various components.
  - [Community Edition](/docs/v2022.03.29/setup/uninstall/community): Provides uninstallation instructions for Stash Community Edition.
  - [Enterprise Edition](/docs/v2022.03.29/setup/uninstall/enterprise): Provides uninstallation instructions for Stash Enterprise Edition.
  - [Stash kubectl Plugin](/docs/v2022.03.29/setup/uninstall/kubectl_plugin): Provides uninstallation instructions for Stash `kubectl` plugin.
- [Upgrade Stash](/docs/v2022.03.29/setup/upgrade/): Provides instruction for updating Stash license and upgrading between various Stash versions.

## [Guides](/docs/v2022.03.29/guides/)

Guides show how to perform different operations with Stash.

- [Supported Backends](/docs/v2022.03.29/guides/backends/overview): Describes how to configure different storage for storing backed up data.
- [Workload Volume Backup](/docs/v2022.03.29/guides/workloads/overview): Shows how to use Stash to backup and restore volumes of a workload (i.e. `Deployment`, `StatefulSet`, `DaemonSet`, etc).
- [Stand-alone Volume Backup](/docs/v2022.03.29/guides/volumes/overview): Shows how to use Stash to backup and restore stand-alone volumes(i.e. `PersistentVolumeClaim`).
- [Batch Backup](/docs/v2022.03.29/guides/batch-backup/overview): Shows how to backup multiple co-related components using a single configuration known as `BackupBatch`.
- [Auto Backup](/docs/v2022.03.29/guides/auto-backup/overview): Shows how to configure automatic backup of any stateful workload in your cluster.
- [Volume Snapshot](/docs/v2022.03.29/guides/volumesnapshot/overview): Shows how Stash takes snapshot of `PersistentVolumeClaim`s and restore them from snapshot using Kubernetes `VolumeSnapshot` API.

- **Different Use Cases:**
Shows different uses cases of Stash like instant backup, pause backup, cross-namespace backup and restore etc.

  - [Instant Backup](/docs/v2022.03.29/guides/use-cases/instant-backup): Shows you how to take an instant backup in Stash.
  - [Exclude/Include Files](/docs/v2022.03.29/guides/use-cases/exclude-include-files/): Shows how to exclude/include subset of files during backup/restore process.
  - [Pause Backup](/docs/v2022.03.29/guides/use-cases/pause-backup): Shows how to pause a backup temporarily.
  - [Clone Data Volumes](/docs/v2022.03.29/guides/use-cases/clone-pvc): Shows how to clone data volumes of a workload into a different namespace in a cluster using Stash.
  - [File Ownership](/docs/v2022.03.29/guides/use-cases/ownership): Explains when and how ownership of the restored files can be changed.
  - [Cross-Namespace Backup and Restore](/docs/v2022.03.29/guides/use-cases/cross-namespace-backup/): Shows how to take backup and restore across namespaces using Stash.
  - [Cross-Cluster Backup and Restore](/docs/v2022.03.29/guides/use-cases/cross-cluster-backup/): Shows how to take backup and restore across clusters using Stash.
  - [Customize Backup and Restore](/docs/v2022.03.29/guides/use-cases/customize-backup-restore/): Shows how to customize backup and restore processes in Stash according to your needs.
- [Platforms](/docs/v2022.03.29/guides/platforms/eks): Shows how to use Stash to backup and restore volumes of a Kubernetes workload running in different platforms.
- [Monitoring](/docs/v2022.03.29/guides/monitoring/overview/): Shows how Prometheus monitoring works with Stash, what metrics Stash exports, and how to enable monitoring.
- [Hooks](/docs/v2022.03.29/guides/hooks/overview): Shows how to execute different actions before/after the backup/restore process.
- [CLI](/docs/v2022.03.29/guides/cli/cli): Shows how to manage Stash objects quickly and easily using Stash `kubectl` plugin.
- [Troubleshooting](/docs/v2022.03.29/guides/troubleshooting/how-to-troubleshooot/): Gives an overview of how you can gather the necessary information to identify the issue that causes the backup/restore failure.
- [Security](/docs/v2022.03.29/guides/security/rbac): Describes different built-in cluster security support by Stash.

We're always looking for help improving our documentation, so please don't hesitate to [file an issue](https://github.com/stashed/project/issues/new) if you see some problem.

---
title: Backup and Restore Workload Data Overview | Stash
description: An overview on how Backup and Restore of workload data works in Stash.
menu:
  docs_v2022.05.18:
    identifier: workload-overview
    name: How Backup and Restore works?
    parent: workload
    weight: 10
product_name: stash
menu_name: docs_v2022.05.18
section_menu_id: guides
info:
  cli: v0.20.0
  community: v0.20.1
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
  - 8.2.0
  enterprise: v0.20.1
  etcd:
  - 3.5.0-v4
  installer: v2022.05.18
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
  - 2.6.1-v5
  - 2.8.2
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
  version: v2022.05.18
---

# Backup and Restore Workloads using Stash

This guide will show you how Stash backs up and restores volumes of various workload types (Deployment, StatefulSet, DaemonSet etc.).

## Before You Begin

- You should be familiar with the following `Stash` concepts:
  - [BackupConfiguration](/docs/v2022.05.18/concepts/crds/backupconfiguration)
  - [BackupSession](/docs/v2022.05.18/concepts/crds/backupsession)
  - [RestoreSession](/docs/v2022.05.18/concepts/crds/restoresession)
  - [Repository](/docs/v2022.05.18/concepts/crds/repository)

## How Backup Process Works

The following diagram shows how Stash takes backup of the volumes of a workload. Open the image in a new tab to see the enlarged version.

<figure align="center">
  <img alt="Stash Backup Flow" src="/docs/v2022.05.18/images/guides/workloads/backup_overview.svg">
<figcaption align="center">Fig: Backup process of Workload volumes in Stash</figcaption>
</figure>

The backup process consists of the following steps:

1. At first, a user creates a Secret. This secret holds the credentials to access the backend where the backed up data will be stored.

2. Then, she creates a `Repository` crd which represents the original repository in the backend.

3. Then, she creates a `BackupConfiguration` crd which specifies the targeted workload and desired file paths to backup. It also specifies the `Repository` object that holds the backend information where the backed up data will be stored.

4. Stash operator watches for `BackupConfiguration` objects.

5. When it finds a `BackupConfiguration` object, it finds the targeted workload and injects a sidecar named `stash`.

6. It also creates a `CronJob` to trigger backups periodically.

7. The`CronJob` triggers backup on each scheduled slot by creating a `BackupSession` crd.

8. The `stash` sidecar inside the workload watches for `BackupSession` crd.

9. When it finds a `BackupSession` crd, it initiates backup of the targeted file paths.

10. Once the backup process is completed, the `sidecar` sends Prometheus metrics to the Pushgateway running inside the `stash-operator` pod. It also updates respective `BackupSession` and `Repository` status to reflect the backup process.

## How Restore Process Works

The following diagram shows how Stash restores backed up data inside a workload. Open the image in a new tab to see the enlarged version.

<figure align="center">
  <img alt="Stash Backup Flow" src="/docs/v2022.05.18/images/guides/workloads/restore_overview.svg">
<figcaption align="center">Fig: Restore process of Workload volumes in Stash</figcaption>
</figure>

The restore process consists of the following steps:

1. At first, the user creates a workload where the data will be restored.

2. Then, she creates a `RestoreSession` crd that specifies the targeted workload where the backed up data will be restored. It also specifies the respective `Repository` that holds the respective backend information.

3. Stash operator watches for `RestoreSession` crds.

4. When it finds a `RestoreSession` crd, it injects an init-container named `stash-init` to the workload and restart it.

5. The init-container restores the desired data from the backend on start-up.

6. Finally, when the restore process is completed it sends Prometheus metrics to the `pushgateway` running inside the stash operator. It also update the `RestoreSession` status to reflect the restore process.

> **Note:** If your workload restarts with the `stash-init` init-container for any reason, the init-container will skip running restore process if there is no pending `RestoreSession` for this workload.

## Next Steps

1. See a step by step guide to backup/restore volumes of a Deployment [here](/docs/v2022.05.18/guides/workloads/deployment).
2. See a step by step guide to backup/restore volumes of a StatefulSet [here](/docs/v2022.05.18/guides/workloads/statefulset).
3. See a step by step guide to backup/restore a Daemonset's volumes [here](/docs/v2022.05.18/guides/workloads/daemonset).
